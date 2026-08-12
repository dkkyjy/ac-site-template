---
title: "CR Pulse Interpolator - 基础 2D 自动傅里叶插值走查"
category: "专项工具 / 科研"
description: "crpulse 基础 2D 傅里叶插值逐行讲解。"
pubDate: "2026-08-12"
badge: "guide"
tags: ["cr-pulse-interpolator", "物理"]
---

# minimal_usage_demo.py 逐行讲解

本文档逐行讲解 `minimal_usage_demo.py` 的完整工作流程。该脚本演示了如何使用 **cr-pulse-interpolator** 包对宇宙射线大气簇射（CoREAS 模拟）的射电信号进行傅里叶插值。

## 1. 导入模块

```bash
head -8 minimal_usage_demo.py
```

```output
# Minimal usage demo script of Fourier interpolation of pulse signals along simulated CR radio footprints
# Author: A. Corstanje (a.corstanje@astro.ru.nl), 2023

import numpy as np

import cr_pulse_interpolator.signal_interpolation_fourier as sigF

import demo_helper
```



- **`numpy`**: 数值计算基础库，用于数组操作和 FFT。
- **`signal_interpolation_fourier`**（别名为 `sigF`）：核心波形插值模块。其中的 `interp2d_signal` 类负责接收一组天线位置及对应的时域信号，构建完整的插值器。
- **`demo_helper`**: 演示辅助模块，负责数据读取（`read_data_hdf5`）、互相关分析（`get_crosscorrelation`）和绘图（`plot_pulse_and_spectrum`）。

## 2. 数据加载

```bash
sed -n '23,25p' minimal_usage_demo.py
```

```output
# Read in demo data
demo_filename = 'demo_shower.h5'
(zenith, azimuth, xmax, footprint_pos_x, footprint_pos_y, test_pos_x, test_pos_y, footprint_antenna_data, test_antenna_data, footprint_time_axis, test_time_axis) = demo_helper.read_data_hdf5(demo_filename)
```

```bash
sed -n '150,175p' demo_helper.py
```

```output
def read_data_hdf5(filename):
    """
    Reading in the demo data hdf5 file.
    The time traces inside are CoREAS E-fields, converted to 2 'on-sky' polarizations.
    """
    try:
        demo_file = h5py.File(filename, 'r')
    except:
        raise ValueError('Cannot read data file; demo data downloaded with download_demo_data.sh?')
    zenith = demo_file.get('zenith')[()]
    azimuth = demo_file.get('azimuth')[()]
    xmax = demo_file.get('xmax')[()]
    footprint_positions = np.array(demo_file.get('footprint_positions'))
    test_positions = np.array(demo_file.get('test_positions'))
    (footprint_pos_x, footprint_pos_y) = (footprint_positions[:, 0], footprint_positions[:, 1])
    (test_pos_x, test_pos_y) = (test_positions[:, 0], test_positions[:, 1])

    footprint_antenna_data = np.array(demo_file.get('footprint_antennas'))
    test_antenna_data = np.array(demo_file.get('test_antennas'))

    footprint_time_axis = np.array(demo_file.get('time_axis_footprint_antennas'))
    test_time_axis = np.array(demo_file.get('time_axis_test_antennas'))

    demo_file.close()

    return (zenith, azimuth, xmax, footprint_pos_x, footprint_pos_y, test_pos_x, test_pos_y, footprint_antenna_data, test_antenna_data, footprint_time_axis, test_time_axis)
```



`read_data_hdf5` 从 HDF5 文件中读取 CoREAS 模拟数据：

| 返回值 | 含义 | 形状 |
|---|---|---|
| `zenith, azimuth, xmax` | 模拟的几何参数 | 标量值 |
| `footprint_pos_x/y` | 208 个训练天线的 (x, y) 坐标（星形排列） | (208,) |
| `test_pos_x/y` | 250 个测试天线的位置（用于评估插值精度） | (250,) |
| `footprint_antenna_data` | 训练天线的时域电场波形 | (208, 4082, 2) |
| `test_antenna_data` | 测试天线的真实波形（用作 ground truth） | (250, 4082, 2) |
| `footprint/test_time_axis` | 各天线的绝对起始时间 | (N, 4082) |

**数据形状解释**: (Nant, Nsamples, Npols) = (天线数, 每迹线采样点数, 偏振数)。这里 2 个偏振对应两个 on-sky 方向。

## 3. 初始化插值器

```bash
sed -n '33,38p' minimal_usage_demo.py
```

```output
print('Initializing interpolator object...')
phase_method = "phasor" # other option is "timing" which takes longer to initialize (to check which performs better in use cases not yet tested)
signal_interpolator = sigF.interp2d_signal(
    footprint_pos_x, footprint_pos_y, footprint_antenna_data,
    verbose=False, phase_method=phase_method, signals_start_times=footprint_time_axis[:, 0]
)
```


#### __init__ 工作流程 — 步骤 1: FFT 频谱分解

`get_spectra` 对每个天线的两个偏振分别做 **实 FFT** (`np.fft.rfft`)，得到：

- **幅度谱** `abs_spectrum`：|FFT(signal)|，反映信号在各频率的强度
- **相位谱** `phasespectrum`：∠FFT(signal) ∈ (-π, π]，反映各频率分量的相位延迟
- 频率轴转换为 MHz 单位

`rfft` 比完整 FFT 更快，因为实信号的频谱是对称的。

```bash
sed -n '56,80p' ../src/cr_pulse_interpolator/signal_interpolation_fourier.py
```

```output
    def get_spectra(self, signals):
        """
        Do FFT of 'signals', assumed shape (Nants, Nsamples, Npol) i.e. the time traces are along the second axis.
        Produce absolute-amplitude spectrum and phase spectrum

        Parameters
        ----------
        signals : np.ndarray
            the input time traces, shaped as (Nants, Nsamples, Npol)
        """
        Nsamples = signals.shape[1]

        if self.verbose:
            print('Doing FFTs...', end=' ')
        all_antennas_spectrum = np.fft.rfft(signals, axis=1)
        abs_spectrum = np.abs(all_antennas_spectrum)
        phasespectrum = np.angle(all_antennas_spectrum)
        unwrapped_phases = np.unwrap(phasespectrum, axis=1, discont=0.7 * np.pi)
        if self.verbose:
            print('done.')

        freqs = np.fft.rfftfreq(Nsamples, d=self.sampling_period)
        freqs /= 1.0e6  # in MHz

        return freqs, all_antennas_spectrum, abs_spectrum, phasespectrum, unwrapped_phases
```


#### __init__ 工作流程 — 步骤 2: Hilbert 包络定时

1. **带通滤波**：将信号滤波到 30–80 MHz 范围（移除低频噪声和高频噪声）
2. **上采样**：将滤波后的信号上采样 10 倍，达到亚采样点级的时间精度
3. **Hilbert 包络**：计算 `scipy.signal.hilbert` → 取绝对值得到包络 → 合并两个偏振
4. **峰值检测**：找到包络的最大值位置 → 转换为以秒为单位的脉冲到达时间

为什么需要这一步？—— 各天线到空气簇射核心的距离不同，脉冲到达时间也不同。插值前需要把 **延迟时间** 从相位谱里移除，使后续插值针对的是 **对齐后** 的脉冲。

```bash
sed -n '128,143p' ../src/cr_pulse_interpolator/signal_interpolation_fourier.py
```

```output
        signals_upsampled = resample(filtered_signals, upsample_factor * Nsamples, axis=1)

        nof_samples = signals_upsampled.shape[1]
        signals_upsampled = np.roll(
            signals_upsampled, nof_samples // 2, axis=1
        )  # Put in the middle of the block, avoiding negative values in timing

        if do_hilbert_envelope:
            if self.verbose:
                print('Hilbert envelope')
            hilbert_envelope = np.abs(hilbert(signals_upsampled, axis=1))

            if sum_over_pol:
                hilbert_sum_over_pol = np.sqrt(np.sum(hilbert_envelope ** 2, axis=2))

                pulse_timings = (np.argmax(hilbert_sum_over_pol, axis=1) - nof_samples // 2) * (timestep / upsample_factor)
```



#### __init__ 工作流程 — 步骤 3: 相位校正与常量相位

**移除脉冲到达时间**：`timing_corrected_phases` 对每个天线和偏振执行 Δφ(f) = 2π × f × Δt。物理含义：脉冲到达时间的延迟在频域表现为线性相位梯度，减去后所有脉冲被对齐到 t=0。

**常量相位（Hilbert 相位）**：`get_constant_phases` 实现了论文中的 Eq. (2.3)，将校正后的相位谱与幅度谱加权相加，取总和的幅角。这个值决定了脉冲是类余弦（≈ 0）还是类正弦（≈ π/2）。

**相干性**（degree of coherency, Eq. 2.4）：γ = |Σ A(f)·exp(i φ_c(f))| / Σ A(f)。值越接近 1，信号越接近理想冲击脉冲。低于阈值（默认 0.9）时对应频率被标记为截止频率。

```bash
sed -n '240,285p' ../src/cr_pulse_interpolator/signal_interpolation_fourier.py
```

```output
    def sum_corrected_spectrum(self, high_freq, low_freq):
        complex_phases = np.exp(1.0j * self.phasespectrum_corrected)
        spectrum_corrected = self.abs_spectrum * complex_phases

        freq_range = np.where((self.freqs > low_freq) & (self.freqs < high_freq))[0]
        complex_sum = np.sum(spectrum_corrected[:, freq_range, :], axis=1)

        return complex_sum, freq_range

    def degree_of_coherency(self, low_freq=30.0, high_freq=500.0):
        """
        This implements Eq. (2.4) in the article, for given frequency band limits

        Parameters
        ----------
        low_freq : float, default=30.0
        high_freq : float, default=500.0
        """
        complex_sum, freq_range = self.sum_corrected_spectrum(high_freq, low_freq)

        abs_sum = np.sum(self.abs_spectrum[:, freq_range, :], axis=1)

        coherency = np.abs(complex_sum) / abs_sum

        return coherency

    def get_constant_phases(self, low_freq=30.0, high_freq=500.0):
        """
        This implements Eq. (2.3) in the article, for given frequency band limits.
        Phases have been corrected to have maximum Hilbert envelope at "t"=0
        So, add up the complex phases, weighted by the amplitudes, to get the constant phase
        which determines if the pulse is cos-like or sin-like, or a value in between

        Parameters
        ----------
        low_freq : float, default=30.0
        high_freq : float, default=500.0
        """
        complex_sum, freq_range = self.sum_corrected_spectrum(high_freq, low_freq)

        const_phases = np.angle(complex_sum)

        # Do unwrapping in 2D to avoid 2 pi periodicity mismatches (spurious jumps of 2 pi)
        const_phases_unwrapped = self.phase_unwrap_2d(self.pos_x, self.pos_y, const_phases)

        return const_phases_unwrapped
```


#### 核心算法：interp2d_fourier 的工作原理

`interp2d_fourier` 是底层 2D 标量插值器：

**初始化**：

1. 极坐标排序：将 (x, y) 转换为 (r, φ)，按星形网格排序为 (Nradius, Nphi) 二维数组
2. 角度方向 FFT：对每个半径圈上的值做 rfft（沿 φ 方向），得到角向傅里叶模式系数
3. 径向插值器：用 scipy.interpolate.interp1d 对每个模式沿 r 方向构建插值函数

**调用时**：

1. 对任意查询点 (x, y)，计算 (r, φ)
2. 用径向插值器在该 r 处获取所有模式的傅里叶系数
3. 傅里叶级数展开：f(r, φ) = Σ c_k(r)cos(kφ) + s_k(r)sin(kφ)

这就是初始化时构建 ~1236 个 `interp2d_fourier` 实例的原因（每个频率通道 x 每个偏振都有幅度、cos/sin 插值器）。

```bash
sed -n '83,117p' ../src/cr_pulse_interpolator/interpolation_fourier.py
```

```output
    def __init__(self, x, y, values, radial_method='cubic', fill_value='extrapolate', recover_concentric_rings=False):
        # Convert (x, y) to (r, phi), make 2d position array, sorting positions and values by r and phi
        radius = np.sqrt(x ** 2 + y ** 2)

        ordering_indices = self.get_ordering_indices(x, y)
        values_ordered = np.copy(values)[ordering_indices]

        # Store the (unique) radius values
        self.radial_axis = radius[ordering_indices][:, 0]
        # Check if the radius does not vary along angular direction (with tolerance)
        if np.max(np.std(radius[ordering_indices], axis=1)) > 0.1 * np.min(radius):
            if not recover_concentric_rings:
                raise ValueError("Radius must be (approx.) constant along angular direction. "
                                 "You can try to \"fix\" that by using \"recover_concentric_rings=True\"")
            else:
                self.radial_axis = np.mean(radius[ordering_indices], axis=1)
                values_ordered_interpolated = []
                for x, y in zip(radius[ordering_indices].T, values_ordered.T):
                    intpf = intp.interp1d(
                        x, y, axis=0, kind=radial_method, fill_value='extrapolate')
                    values_ordered_interpolated.append(intpf(self.radial_axis))
                values_ordered = np.array(values_ordered_interpolated).T

        # FFT over the angular direction, for each radius
        self.angular_FFT = np.fft.rfft(values_ordered, axis=1)
        length = values_ordered.shape[-1]
        self.angular_FFT /= float(length)  # normalize

        # Produce interpolator function, interpolating the FFT components as a function of radius

        if fill_value is None:
            fill_value = (self.angular_FFT[0], np.zeros_like(self.angular_FFT[0]))
        self.interpolator_radius = intp.interp1d(
            self.radial_axis, self.angular_FFT, axis=0, kind=radial_method, fill_value=fill_value, bounds_error=False
        )  # Interpolates the Fourier components along the radial axis
```

```bash
sed -n '119,155p' ../src/cr_pulse_interpolator/interpolation_fourier.py
```

```output
    def __call__(self, x, y, max_fourier_mode=None):
        """
        Interpolate the input used in __init__ for input positions (x, y)

        Parameters
        ----------
        x : float or np.ndarray
            x positions as float or numpy ND array
        y : float or np.ndarray
            idem for y
        max_fourier_mode : int, optional
            cutoff for spatial frequencies along circles, i.e. do Fourier sum up to (incl.) this mode.
            Default None i.e. do all modes
        """
        radius = np.sqrt(x ** 2 + y ** 2)
        phi = np.arctan2(y, x) - self._phi0

        # Interpolate Fourier components over all values of radius
        fourier = self.interpolator_radius(radius)
        fourier_len = fourier.shape[-1]

        (cos_components, sin_components) = interp2d_fourier.cos_sin_components(fourier)

        # Multipliers for Fourier modes, as k in cos(k*phi), sin(k*phi)
        limit = max_fourier_mode + 1 if max_fourier_mode is not None else fourier_len
        mult = np.linspace(0, limit - 1, limit).astype(int)

        # The Fourier sum done explicitly, as sum_k( c_k cos(k phi) + s_k sin(k phi) )
        result = np.zeros_like(radius)
        if isinstance(phi, float):
            result += np.sum(cos_components[..., 0:limit] * np.cos(phi * mult))
            result += np.sum(sin_components[..., 0:limit] * np.sin(phi * mult))
        else:
            result += np.sum(cos_components[..., 0:limit] * np.cos(phi[..., np.newaxis] * mult), axis=-1)
            result += np.sum(sin_components[..., 0:limit] * np.sin(phi[..., np.newaxis] * mult), axis=-1)

        return result
```


## 4. 执行插值（__call__）

```bash
sed -n '40,53p' minimal_usage_demo.py
```

```output
test_index = 124 # take one of the 250 test positions
pol = 0
this_x, this_y = test_pos_x[test_index], test_pos_y[test_index]
print('Interpolating pulse at position x = %3.2f, y = %3.2f m' % (this_x, this_y))

orig_pulse = test_antenna_data[test_index] # shape is (Nsamples, Npol)

"""
Call the interpolator object to obtain the interpolated pulse at the desired position
Return shape is (Nsamples, Npol)
Optionally, it can be low-pass filtered to an estimated reliable cutoff frequency
This is a reliable yet sometimes overly conservative estimate up to which frequency the interpolation is accurate
"""
interpolated_pulse, timings, _, _ = signal_interpolator(this_x, this_y, full_output=True)
```


phasor 模式下的 __call__ 工作流程：

1. 对每个频率通道和偏振，分别插值得到该位置的幅度 A(f) 和相位 φ(f)
2. `interpolators_cosphi/sinphi` 插值 cos(φ) 和 sin(φ)，再通过 `np.angle(cos + i·sin)` 合成相位
3. 恢复脉冲到达时间和常量相位（之前移除，现在加回）
4. IFFT 将频域信号变换回时域

返回形状 (Nsamples, Npols) 的时域脉冲，以及（若 full_output=True）起始时间和频谱。

```bash
sed -n '668,682p' ../src/cr_pulse_interpolator/signal_interpolation_fourier.py
```

```output
        elif self.method == 'phasor':
            for freq_channel in range(Nfreqs):
                for pol in range(Npols):
                    # Interpolate abs-amplitude spectrum and phasors
                    thisPower = self.interpolators_abs_spectrum[pol, freq_channel](x, y)
                    this_realpart = self.interpolators_cosphi[pol, freq_channel](x, y)
                    this_imagpart = self.interpolators_sinphi[pol, freq_channel](x, y)

                    thisPhase = np.angle(this_realpart + 1.0j * this_imagpart)
                    # making unit vector by dividing by abs(re**2 + im**2) and multiplying that may be significantly faster

                    abs_spectrum[freq_channel, pol] = thisPower
                    phasespectrum[freq_channel, pol] = thisPhase

        else:
```


## 5. 时间轴处理

插值器返回的 `timings` 包含了脉冲起始时间和居中偏移量。通过构建新的时间轴与原始波形对齐：

- `arange(len(pulse)) * sampling_period` → 按采样间隔生成等间距时间点
- `+ timings` → 将时间轴整体偏移到正确的绝对时间

这样原始波形和插值波形具有可比的时间基准。

```bash
sed -n '55,61p' minimal_usage_demo.py
```

```output
"""
Because the trace start times were provided during the interpolator initialisation, the returned variable
`timings` is the start time of the interpolated pulse, including the offset induced by centering the pulse 
(which is the default behaviour). To compare to the original pulse, we create a new time axis for the 
interpolated pulse.
"""
interpolated_time_axis = np.arange(len(interpolated_pulse)) * signal_interpolator.sampling_period + timings
```


## 6. 结果可视化

`plot_pulse_and_spectrum` 生成并列双图（1×2 subplots）：

**左侧：时域脉冲对比**
- 绘制原始脉冲和插值脉冲（单位 μV/m）
- 计算残差（对齐后相减），直观评估插值质量

**右侧：功率谱对比**
- 显示两个脉冲的 |rfft(signal)|²
- 标注位置 (x, y)、核心距离 r、偏振编号
- 标注归一化互相关（CC）和时间偏差 Δt

**互相关计算**：两个信号上采样 10 倍后计算归一化互相关：
CC = ⟨test | orig⟩ / √(⟨test | test⟩ · ⟨orig | orig⟩)
CC = 1 表示完美匹配。

```bash
sed -n '64,69p' minimal_usage_demo.py
```

```output
demo_helper.plot_pulse_and_spectrum(
    test_time_axis[test_index], orig_pulse[:, pol],
    interpolated_time_axis, interpolated_pulse[:, pol],
    this_x, this_y, -1, pol,
    save_path='interpolation_result.png'
)
```


完整代码参考 `demo_helper.py` 中的 `plot_pulse_and_spectrum` 和 `get_crosscorrelation` 函数。

```bash
sed -n '79,147p' demo_helper.py
```

```output
def plot_pulse_and_spectrum(orig_time_axis, orig_pulse, interpolated_time_axis, interpolated_pulse, x, y, cutoff_freq, pol, save_path=None):
    """
    Plots an interpolated pulse together with a 'true' simulated pulse

    Parameters
    ----------
    orig_time_axis : np.ndarray
        Time axis for the original pulse
    orig_pulse : np.ndarray
        time trace, 1D array
    interpolated_pulse : np.ndarray
    interpolated_time_axis : np.ndarray
        Time axis for the interpolated pulse
    x : the x position (float), for annotation in the plot
    y : idem for y
    cutoff_freq : value of estimated cutoff frequency, for annotation only
    pol : polarization number
    save_path : str, optional
        If provided, save figure to path instead of showing it.
    """
    radius = np.sqrt(x**2 + y**2)
    freqs = get_freq_axis(orig_pulse)

    (CC_zeroshift, CC_optimized_timeshift, delta_t, energy_rel_diff) = get_crosscorrelation(orig_pulse, interpolated_pulse)

    fig, ax = plt.subplots(figsize=(10.67, 4), nrows=1, ncols=2)
    ax1, ax2 = ax[0], ax[1]

    #plt.figure()
    ax1.plot(orig_time_axis, 1.0e6 * orig_pulse, label='orig pulse', lw=2)
    ax1.plot(interpolated_time_axis, 1.0e6 * interpolated_pulse, label='interpolated pulse', lw=2)

    time_offset = int(orig_pulse.argmax() - interpolated_pulse.argmax())
    residual = 1.0e6 * (np.roll(interpolated_pulse, time_offset) - orig_pulse)
    ax1.plot(orig_time_axis, residual, label='difference', lw=2, c='g')

    interpolated_energy = np.sum(interpolated_pulse**2)
    orig_energy = np.sum(orig_pulse**2)
    #fixed_pulse = interpolated_pulse * np.sqrt(orig_energy / interpolated_energy)
    #plt.plot(time_axis, fixed_pulse, label='fixed pulse', c='r', lw=1)
    ax1.grid()
    ax1.set_xlabel('Time [ ns ]')
    ax1.set_ylabel(r'E-field [ $\mu$V/m ]')
    ax1.set_xlim(orig_time_axis[0], orig_time_axis[500])
    ax1.legend(loc='best')

    #plt.figure()
    orig_pulse_powerspec = np.abs(np.fft.rfft(orig_pulse))**2
    interp_pulse_powerspec = np.abs(np.fft.rfft(interpolated_pulse))**2
    ax2.plot(freqs, orig_pulse_powerspec, label='Orig pulse')
    ax2.plot(freqs, interp_pulse_powerspec, label='Interpolated pulse')
    ax2.text(0.98, 0.40, 'Position x = %3.1f, y = %3.1f, r = %3.2f m, pol = %d' % (x, y, radius, pol), transform=plt.gca().transAxes, ha='right') #, va='right')
    ax2.text(0.98, 0.30, 'CC = %1.5f, CC_max = %1.5f' % (CC_zeroshift, CC_optimized_timeshift), transform=plt.gca().transAxes, ha='right')
    ax2.text(0.98, 0.20, 'delta_t = %1.2f ns, cutoff freq = %3.1f MHz' % (delta_t, cutoff_freq), transform=plt.gca().transAxes, ha='right')

    #plt.yscale('log')
    ax2.grid()
    ax2.set_xlabel('Frequency [ MHz ]')
    ax2.set_ylabel('Power spectrum [ a.u. ]')
    ax2.set_xlim(0, 500)
    ax2.set_ylim(0.0, 1.2*np.max(orig_pulse_powerspec))
    ax2.legend(loc='best')

    if save_path:
        plt.savefig(save_path, bbox_inches='tight', dpi=150)
        plt.close(fig)
        print('Figure saved to %s' % save_path)
    else:
        plt.show()
```


## 7. 总结：完整工作流程

### 数据流图

HDF5 文件 (demo_shower.h5)
  |
  v
read_data_hdf5() -> 足印天线 (208) + 测试天线 (250)
  |
  v
interp2d_signal.__init__()
  |
  +-- get_spectra() ---> 幅度谱 + 相位谱 (FFT)
  +-- hilbert_envelope_timing() ---> 脉冲到达时间
  +-- timing_corrected_phases() ---> 移除时间延迟
  +-- get_constant_phases() ---> 常量相位 (Hilbert 相位)
  +-- get_coherency_vs_frequency() ---> 截止频率
  |
  +-- 构建 ~1236 个 interp2d_fourier 插值器
        |
        +-- 幅度谱插值器 (2 pols x ~412 freq channels)
        +-- cos(phi) / sin(phi) 插值器 (2 pols x ~412 freq channels)
        +-- 脉冲定时插值器 (2)
        +-- 常量相位插值器 (2)
        +-- 截止频率插值器 (2)
              |
              v
interp2d_signal.__call__(x, y)
  |
  +-- 对每个频率通道和偏振:
  |     interp2d_fourier 插值 -> 幅度 A(f), 相位 phi(f)
  |
  +-- 恢复脉冲到达时间和常量相位
  +-- 合成复数频谱: S(f) = A(f) * exp(i * phi(f))
  +-- 带通滤波到有效频率范围
  |
  +-- IFFT -> 时域脉冲 (Nsamples, Npols)

### 关键设计思想

| 问题 | 解决方案 |
|---|---|
| 天线网格不是矩形而是星形 | 极坐标 (r, phi) 重新排序 |
| 脉冲到达时间随位置变化 | Hilbert 包络检测 + 频域相位校正 |
| 不同频率的插值可靠性不同 | 相干性分析 -> 自适应截止频率 |
| 相位需要在空间上平滑 | 极坐标上 2D 相位展开 |
| 高频细节的相位建模 | Phasor 模式 vs Timing 模式（详见论文） |

```bash {image}
![插值结果: 原始 vs 插值脉冲及其频谱](/cr-pulse-interpolator/interpolation_result.png)
```

