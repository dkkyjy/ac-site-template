---
title: "CR Pulse Interpolator - 2D 标量插值逐行讲解"
category: "专项工具 / 科研"
description: "crpulse 2D 标量插值走查。"
pubDate: "2026-08-12"
badge: "guide"
tags: ["cr-pulse-interpolator", "物理"]
---

# demo_interpolation_fourier.py 逐行讲解: 2D 标量插值

*2026-07-30T04:25:04Z by Showboat 0.6.1*
本文档逐行讲解 `demo_interpolation_fourier.py`。该脚本演示了 **cr-pulse-interpolator** 包最核心的底层模块 — `interp2d_fourier` 类，它实现了在星形极坐标网格上的 2D 傅里叶插值。

## 1. 导入与数据加载

```bash
head -8 demo_interpolation_fourier.py
```

```output
# Demonstration script for interpolation_Fourier.py
# Author: A. Corstanje, (a.corstanje@astro.ru.nl), 2023

import numpy as np
import matplotlib.pyplot as plt
# plt.ion()
from matplotlib import cm
import cr_pulse_interpolator.interpolation_fourier as interpF
```



脚本导入：

- **`numpy`**: 数值计算
- **`matplotlib`**: 结果可视化
- **`interpolation_fourier`**（别名为 `interpF`）: 核心 2D 傅里叶插值模块

数据来自 `sample_data.txt`，包含三列：(x, y, values)。这是一个星形极坐标网格上的采样点：

- 多个同心圆环，同心圆上等角度间隔排列
- 每个点有一个标量值 f(x, y)

```bash
sed -n '70,73p' demo_interpolation_fourier.py
```

```output
fname = 'sample_data.txt'
data = np.loadtxt(fname)
(x, y, values) = data.T

```

让我们看一下原始数据的结构。

```bash
wc -l sample_data.txt && head -5 sample_data.txt && echo '...' && awk '{print sqrt($1*$1+$2*$2)}' sample_data.txt | sort -n | uniq -c | head -10
```

```output
     184 sample_data.txt
3.125000e+00 -2.971353e-16 9.741148e-01
2.209709e+00 2.209709e+00 9.719506e-01
7.094395e-16 3.125000e+00 8.911635e-01
-2.209709e+00 2.209709e+00 8.296795e-01
-3.125000e+00 6.798374e-16 7.942001e-01
...
   8 3.125
   8 6.25
   8 12.5
   8 25
   8 50
   8 75
   8 100
   8 125
   8 150
   8 175
```


数据显示：

- 总采样点数：由行数决定
- 数据格式为三列：x(m), y(m), value
- 星形网格：每个半径上有多个等角度间隔的采样点

## 2. 初始化插值器

```bash
sed -n '74,75p' demo_interpolation_fourier.py
```

```output
### Get instance of interpolator, using given values for (x, y)
fourier_interpolator = interpF.interp2d_fourier(x, y, values)
```


`interp2d_fourier` 的 `__init__` 是核心初始化和算法所在。

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


#### __init__ 详解

**1. 极坐标排序** (get_ordering_indices)：

- 将 (x, y) 转换为 (r, φ)，φ ∈ [0, 2π)
- 按 φ 排序后，检测星形网格的跳变点，确定半径步数和角度步数
- 在每个角度方向内按半径排序
- 最终输出形状为 (Nradius, Nphi) 的索引数组

**2. 角向 FFT**：

- 对每个半径圈上的值数组做 `np.fft.rfft`（沿角度方向）
- 归一化：除以角度方向的点数

**3. 径向插值器**：

- 使用 `scipy.interpolate.interp1d` 对每个傅里叶模式沿径向做插值
- 默认使用 cubic 样条，支持外推

```bash
sed -n '34,64p' ../src/cr_pulse_interpolator/interpolation_fourier.py
```

```output
    def get_ordering_indices(cls, x, y):
        """
        Produces ordering indices to create (radius, phi) 2D-array from unordered x and y (1D-)arrays.

        Parameters
        ----------
        x : np.ndarray
            1D array of x positions
        y : np.ndarray
            1D array of y positions
        """

        radius = np.sqrt(x ** 2 + y ** 2)
        phi = np.arctan2(y, x)  # uses interval -pi..pi
        phi = np.around(phi, 15)  # based on observation that offsets from 0 up to 1e-16 can result from arctan2
        phi[phi < 0] += 2 * np.pi  # put into 0..2pi for ordering.
        phi_sorting = np.argsort(phi)
        # Assume star-shaped pattern, i.e. radial # steps = number of (almost) identical phi-values
        # May not work very near (0, 0)
        cls._phi0 = phi[phi_sorting][0]

        test = phi[phi_sorting] - cls._phi0
        radial_steps = len(np.where(np.abs(test) < 0.0001)[0])
        phi_steps = len(phi_sorting) // radial_steps
        phi_sorting = phi_sorting.reshape((phi_steps, radial_steps))
        indices = np.argsort(radius[phi_sorting], axis=1)
        for i in range(phi_steps):  # Sort by radius; should be possible without for-loop...
            phi_sorting[i] = phi_sorting[i][indices[i]]
        ordering_indices = phi_sorting.T  # get shape (radial_steps, phi_steps)

        return ordering_indices
```



## 3. __call__ 调用: 任意位置的插值

初始化后，插值器是一个 callable 对象。传入任意 (x, y) 坐标即可得到插值结果。

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


#### __call__ 解读

1. 对任意查询点 (x, y)，计算其极坐标 (r, φ)
2. 使用径向插值器 `interpolator_radius(r)` 获取该半径处所有角向模式的复数傅里叶系数
3. 通过 `cos_sin_components` 将复数系数转换为实数的 cos 分量和 sin 分量
4. 执行傅里叶级数求和：
   f(r, φ) = Σ_k c_k(r) cos(kφ) + s_k(r) sin(kφ)
5. 支持 `max_fourier_mode` 参数，可截断到前 k 阶模式

## 4. 径向分析: do_plot_radial

演示的第一个可视化：查看各傅里叶模式随半径的变化。

```bash
sed -n '10,42p' demo_interpolation_fourier.py
```

```output
def do_plot_radial(interp_fourier, max_mode=2):
    radial_interpolator = interp_fourier.get_angular_FFT_interpolator()
    fourier = interp_fourier.get_angular_FFT()
    radial_axis = interp_fourier.get_radial_axis()

    fine_radius = np.arange(0.0, 500.0, 0.5)

    fourier_interpolated = radial_interpolator(fine_radius)

    (cosines, sines) = interpF.interp2d_fourier.cos_sin_components(fourier)
    (cosines_fine, sines_fine) = interpF.interp2d_fourier.cos_sin_components(fourier_interpolated)

    y_0 = cosines_fine[:, 0]
    y_1 = cosines_fine[:, 1]

    plt.figure() # Plot cosine modes
    for k in range(max_mode+1):
        plt.plot(radial_axis, cosines[:, k], 'o', label='cos({0} phi) mode'.format(k) if k > 0 else 'Zero mode')

    for k in range(max_mode+1):
        plt.plot(fine_radius, cosines_fine[:, k])
    plt.legend(loc='best')
    plt.xlabel('Radial distance [ m ]')
    plt.ylabel('Value')

    plt.figure() # Sine modes
    for k in range(1, max_mode+1):
        plt.plot(radial_axis, sines[:, k], 'o', label='sin({0} phi) mode'.format(k))

    for k in range(1, max_mode+1):
        plt.plot(fine_radius, sines_fine[:, k])
    plt.legend(loc='best')
    plt.xlabel('Radial distance [ m ]')
```


`do_plot_radial` 的工作流程：

1. 通过 `get_angular_FFT()` 获取原始角向 FFT 系数（离散半径上的值）
2. 通过 `get_angular_FFT_interpolator()` 获取连续径向插值器
3. 在精细半径网格（0 到 500 m, 步长 0.5 m）上获取插值后的 FFT 系数
4. 使用 `cos_sin_components` 提取每个傅里叶模式的 cos 和 sin 分量
5. 绘制 cos 模式（左图）和 sin 模式（右图），用圆圈标记原始数据点，曲线显示插值结果

这验证了径向插值的质量：插值曲线应该平滑地穿过原始数据点。


**cos_sin_components 辅助函数**：将复数傅里叶系数转换为实数表示。

```bash
sed -n '66,81p' ../src/cr_pulse_interpolator/interpolation_fourier.py
```

```output
    @classmethod
    def cos_sin_components(cls, fourier):
        """
        Convert complex FFT as from np.fft.rfft to real-valued cos, sin components

        Parameters
        -----------
        fourier : np.ndarray
            complex Fourier components, with Fourier series running along the last axis.
        """
        cos_components = 2 * np.real(fourier)
        cos_components[..., 0] *= 0.5
        cos_components[..., -1] *= 0.5
        sin_components = -2 * np.imag(fourier)

        return cos_components, sin_components
```


## 5. 角度插值: do_plot_angular

第二个可视化：在固定半径上，比较原始值、完整傅里叶级数重建、以及截断至二阶模式的重建。

```bash
sed -n '46,68p' demo_interpolation_fourier.py
```

```output
def do_plot_angular(interp_fourier, fixed_radius, values_for_radius):
    # Plots angular interpolation at a fixed radius.
    # Inputs: instance of interp2d_fourier, the fixed radius, and 1D-array of values for that radius
    phi_steps = len(values_for_radius)
    phi_step_degrees = 360.0 / phi_steps
    raw_phi_degrees = np.linspace(0.0, 360.0 - phi_step_degrees , phi_steps)

    fine_phi = np.linspace(0.0, 2*np.pi, 1000)
    fine_points_x = fixed_radius * np.cos(fine_phi)
    fine_points_y = fixed_radius * np.sin(fine_phi)

    interp_values = interp_fourier(fine_points_x, fine_points_y)
    interp_values_truncated = interp_fourier(fine_points_x, fine_points_y, max_fourier_mode=2)

    plt.figure()
    plt.plot(raw_phi_degrees, values_for_radius, 'o', label='Values at r={0:.1f} m'.format(fixed_radius))

    plt.plot(fine_phi*180/np.pi, interp_values, label='Fourier series')
    plt.plot(fine_phi*180/np.pi, interp_values_truncated, '--', label='Up to 2nd Fourier mode')
    plt.xlabel('Phi [ deg ]')
    plt.ylabel('Value')
    plt.legend(loc='best')

```


`do_plot_angular` 的工作流程：

1. 计算该半径上原始数据点的角度位置（度）
2. 在精细角度网格（1000 个点，0 到 2π）上构造坐标点
3. 用完整的傅里叶级数插值器计算所有精细点的值
4. 用 `max_fourier_mode=2` 截断计算，仅使用前 2 阶模式
5. 绘制三组数据：原始数据点（圆圈）、完整傅里叶级数（实线）、截断到二阶（虚线）

这验证了角度方向的插值质量，并展示了使用少量傅里叶模式的逼近效果。


脚本在两个半径上执行此分析。

```bash
sed -n '82,88p' demo_interpolation_fourier.py
```

```output
all_radius = np.sqrt(x**2 + y**2)
ordering_indices = fourier_interpolator.get_ordering_indices(x, y)
radius_values = all_radius[ordering_indices][:, 0] # unique radius values
for radius_stepnr in [4, 7]:
    fixed_radius = radius_values[radius_stepnr]
    values_for_radius = values[ordering_indices][radius_stepnr, :]
    do_plot_angular(fourier_interpolator, fixed_radius, values_for_radius)
```


## 6. 二维填色图: 完整网格插值

最后一个可视化：在 500×500 m 的网格上对整个二维函数进行插值并绘制填色图。

```bash
sed -n '91,117p' demo_interpolation_fourier.py
```

```output
dist_scale = 250.0
ti = np.linspace(-dist_scale, dist_scale, 1000)
XI, YI = np.meshgrid(ti, ti)

### Get interpolated values at each grid point, calling the instance of interp2d_fourier
ZI = fourier_interpolator(XI, YI)
###

# And plot it
maxp = np.max(ZI)
fig, ax = plt.subplots()

ax.pcolor(XI, YI, ZI, vmax=maxp, vmin=0, cmap=cm.jet)
ax.scatter(x, y, marker='+', s=3, color='w')

mm = cm.ScalarMappable(cmap=cm.jet)
mm.set_array([0.0, maxp])

cbar = fig.colorbar(mm, ax=ax)
cbar.set_label('Values of f(x, y)')

ax.set_xlabel('x [ m ]')
ax.set_ylabel('y [ m ]')
ax.set_xlim(-250, 250)
ax.set_ylim(-250, 250)
ax.set_aspect('equal')

```


填色图部分的关键点：

1. 在 [-250, 250] × [-250, 250] m 范围内创建 1000×1000 的网格
2. 将网格坐标批量传入插值器（**NumPy 数组广播支持**：`__call__` 中的 `np.sum(cos_components[..., 0:limit] * np.cos(phi[..., np.newaxis] * mult), axis=-1)` 支持矢量化计算）
3. 用白色十字标记原始采样点位置
4. 结果显示插值函数平滑地填充了整个区域

## 7. 总结

### 完整算法流程

   原始数据 (x, y, values)
          |
          v
     get_ordering_indices()
     (x, y) -> (r, phi) 极坐标排序
          |
          v
     np.fft.rfft(values_ordered, axis=1)
     角向 FFT：对每个半径圈做傅里叶变换
          |
          v
     scipy.interpolate.interp1d(radial_axis, angular_FFT)
     对每个傅里叶模式沿径向做样条插值
          |
          v
     __call__(x, y)
          |
          +-- (x, y) -> (r, phi)
          +-- interpolator_radius(r) -> 所有模式的复数系数
          +-- cos_sin_components -> c_k, s_k
          +-- f(r, phi) = Σ c_k cos(k*phi) + s_k sin(k*phi)

### 与波形插值的关系

`interp2d_fourier` 是整个 **cr-pulse-interpolator** 包的数学核心。全波形插值器 `interp2d_signal` 将信号分解为幅度谱、相位谱、到达时间等标量场后，对 **每个标量场单独** 使用 `interp2d_fourier` 做空间插值。

| 波形插值的标量场 | 形状 | 物理含义 |
|---|---|---|
| 幅度谱 A(f) | (Nant, Nfreq, Npol) | 各频率的强度 |
| cos(φ(f)) 和 sin(φ(f)) | (Nant, Nfreq, Npol) | 校正后相位谱的余弦/正弦 |
| 脉冲到达时间 | (Nant, Npol) | Hilbert 包络峰值位置 |
| 常量相位 | (Nant, Npol) | 脉冲的余弦/正弦性 |
| 截止频率 | (Nant, Npol) | 可靠插值的频率上限 |

