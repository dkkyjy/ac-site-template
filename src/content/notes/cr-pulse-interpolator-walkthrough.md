---
title: "CR Pulse Interpolator - 完整走查"
category: "专项工具 / 科研"
description: "crpulse 傅里叶插值完整走查:原理、2D标量插值、全波形插值与最小用法,含逐步输出与图。"
pubDate: "2026-08-12"
badge: "guide"
tags: ["cr-pulse-interpolator", "物理"]
---

# cr-pulse-interpolator 完整走查（Walkthrough）

> 项目：CoREAS 宇宙射线大气簇射射电足迹的傅里叶插值（含 E 场全波形插值）
> 论文：A. Corstanje et al. 2023, JINST 18 P09005, arXiv 2306.13514,
> doi 10.1088/1748-0221/18/09/P09005 — 使用请引用。
> 本文档对项目 3 个演示脚本逐步重现，逐步给出运行输出与生成的图像，最终整理为一条完整理解链路。

---

## 0. 环境与运行方式

项目为标准 Python 包，依赖 `numpy / scipy / matplotlib / h5py`。仓库自带虚拟环境 `.venv`：

```bash
# 在仓库根目录下
export PYTHONPATH="$PWD/src"
.venv/bin/python demo/demo_signal_interpolation.py     # 完整波形插值（主要）
.venv/bin/python demo/demo_interpolation_fourier.py    # 2D 标量函数插值（原理基础）
.venv/bin/python demo/minimal_usage_demo.py            # 最小用法（单个测试点）
```

演示数据 `demo/demo_shower.h5`（CoREAS 模拟的 10^17 eV 质子簇射脚印 E 场）与 `demo/sample_data.txt`（2D 抽样值）。运行时各脚本均以 `plt.show()` 展示图像；本文档用无界面的 Agg 后端重跑，并把每步弹出的图像保存为图片文件以便逐张核对。

---

## 1. 物理背景：为什么要插值

一台宇宙射线实验（如 LOFAR / NenuFAR / SKA）的地面天线阵列只覆盖有限的离散点。要重建**没有天线位置**的电场脉冲波形（如判断簇射到达时间、重建足迹能量沉积），需要一种稳定的空间插值方法。

本库采用 **角向傅里叶级数 + 径向 1D 样条** 的做法：把天线按"（半径 r，方位角 φ）"的极坐标重排，沿 φ 方向做傅里叶展开，再让各阶傅里叶系数沿 r 用 cubic 插值，从而在任意 `(x, y)` 处重建函数值。对完整波形，则在频域对每个频率 bin 分别插值（`phasor` 模式：幅度 + 相位）。

全仓库分两层：

| 模块 | 类 | 插值对象 |
|---|---|---|
| `interpolation_fourier.py` | `interp2d_fourier` | 2D 标量函数 `f(x,y)`（原理演示） |
| `signal_interpolation_fourier.py` | `interp2d_signal` | 完整波形 `一(t, pol)`（真实应用） |

以下按依赖顺序：**先原理（Demo A）→ 再全波形（Demo B）→ 最后最小用法（Demo C）**。

---

## 2. Demo A — `demo_interpolation_fourier.py`：2D 标量函数插值原理

> 文件：`demo/demo_interpolation_fourier.py`，输入：`sample_data.txt`（三列：x、y、value）。

### 步骤 A1：读入数据并实例化插值器

```python
fname = 'sample_data.txt'
data = np.loadtxt(fname)
(x, y, values) = data.T
fourier_interpolator = interpF.interp2d_fourier(x, y, values)
```

`interp2d_fourier.__init__` 内部执行核心预处理：

1. **`get_ordering_indices(x, y)`**：把 (x,y,value) 重排成规则的 `(Nrad, Nphi)` 网格（按半径分桶、桶内按方位角排）。
2. **沿角度轴做 FFT**（`get_angular_FFT`）：每个半径处得到角度方向上的傅里叶变换。
3. **`cos_sin_components(fourier)`**：把复傅里叶系数拆成余弦分量 `c_k` 与正弦分量 `s_k`（`fourier` 第 0 列是 zero mode）。
4. 构建 `get_radial_interpolator`：对每个角度模式 `c_k(r)`、`s_k(r)` 用 `scipy.interpolate.interp1d`（默认 cubic）沿半径插值。

调用 `fourier_interpolator(X, Y)` 就是：查各 `(x,y)` 对应半径 → 用径向插值器给出各角度模式的值 → 按
`f(x,y) = Σ_k c_k(r)·cos(kφ) + s_k(r)·sin(kφ)`
拼回（`max_fourier_mode` 可截断到前几阶）。

### 步骤 A2：画出最低傅里叶模式的径向依赖（fig_01、fig_02）

```python
do_plot_radial(fourier_interpolator)   # 默认 max_mode=2
```

取较密半径序列 `fine_radius = 0~500 m, 步长 0.5 m`，用径向插值器插出各 `k` 的系数，再把离散采样点（`'o'`）与插值结果（实线）叠画。

- **fig_01 — 余弦模式**：zero mode `cos(0φ)` 与 `cos(1φ)`、`cos(2φ)` 沿半径的实测/插值曲线。
- **fig_02 — 正弦模式**：`sin(1φ)`、`sin(2φ)` 沿半径的实测/插值曲线。

![cos 模式](/walkthrough_assets/fig_01.png)
![sin 模式](/walkthrough_assets/fig_02.png)

> 注意到 zero mode 大体随半径单调衰减，一阶模式在 ≤100 m 处显著 —— 这反映了簇射足印的角向不对称主要由低阶模支配。

### 步骤 A3：在固定半径上验证角向展开（fig_03、fig_04）

用两个代表性半径（`radius_values[4]` 与 `radius_values[7]`）：

```python
for radius_stepnr in [4, 7]:
    fixed_radius = radius_values[radius_stepnr]
    values_for_radius = values[ordering_indices][radius_stepnr, :]
    do_plot_angular(fourier_interpolator, fixed_radius, values_for_radius)
```

每个半径画：
- 原始采样点（圆圈）；
- **完整傅里叶级数**重建（实线）；
- 截断到 `max_fourier_mode=2` 的重建（短划线，虚线）。

![angular r=4](/walkthrough_assets/fig_03.png)
![angular r=7](/walkthrough_assets/fig_04.png)

可以看到：少数几个角度模式即可把角向函数拉得很平缓（3 阶以内 m 平滑），两类重建几乎重合，说明足印主要是低阶角结构。

### 步骤 A4：全空间颜色图（fig_05）

```python
ti = np.linspace(-250, 250, 1000)
XI, YI = np.meshgrid(ti, ti)
ZI = fourier_interpolator(XI, YI)         # 10²×10² = 10 万点一次批量求值
ax.pcolor(XI, YI, ZI, vmax=maxp, vmin=0, cmap=cm.jet)
ax.scatter(x, y, marker='+', s=3, color='w')
```

![2D 颜色图](/walkthrough_assets/fig_05.png)

白十字为原始采样位置，彩色是由插值器重建的完整连续函数 —— 覆盖整个 ±250 m 圆形足印且光滑，说明插值在未知区域有效。

---

## 3. Demo B — `demo_signal_interpolation.py`：全波形插值（主要演示）

这是光学里的重头戏：不仅有 `f(x,y)`，而是每个位置都带一整个时间电场波形。输入 `demo_shower.h5` 含大量天线的时域脉冲。

### 步骤 B1：读入数据 + 显示簇射信息

```output
Shower data from a 10^17 proton, azimuth = 90.0, zenith = 30.0 deg, Xmax = 636.30 g/cm2
(azimuth as clockwise from North)
```

### 步骤 B2：实例化全波形插值器（打印初始化过程）

```python
interp = sigF.interp2d_signal(
    footprint_pos_x, footprint_pos_y,
    footprint_antenna_data,   # 各天线 E(t) 阵列
)
```

真实参数（见 `demo/demo_signal_interpolation.py`）：`lowfreq=30.0, highfreq=500.0 MHz, sampling_period=1e-9（=1ns）, phase_method='phasor', radial_method='cubic', upsample_factor=5, coherency_cutoff_threshold=0.9`。

**初始化打印输出（正是重跑得到的）：**
```output
(208,)                                            # 天线数（足印内有 208 根）
Setting sampling period to 1.0e-10 seconds
Doing FFTs... done.
Doing timings using hilbert envelope...
Bandpass filtering 30 to 80 MHz                  # 定时用窄带 30–80 MHz
Strongest polarization is 0
Upsampling by a factor 5
Hilbert envelope
Timings done
Getting coherency and freq cutoff
Creating interpolators... Done.
Done.
```

**这背后 `interp2d_signal.__init__` 依次做了：**

1. `get_spectra()`：对每根天线、每个极化做 FFT，得幅度谱与还保留相位；
2. `hilbert_envelope_timing(30, 80)`：在各天线低频带（30–80 MHz，慢变包络）用 Hilbert 包络找**到达时间**（比全频段更稳）；
3. `timing_corrected_phases()`：把到达时间引起的线性相位剔除，留下**仅跟角向结构有关的相位**；
4. `get_constant_phases()`：剩余相位再按一个参考做常数相位处理；
5. `get_coherency_vs_frequency()`：检测各极化在各频率下的"相干度"，低于阈值 `0.9` 的频率被标记为噪声，给出**自适应频率上限（cutoff）**；
6. 对 `abs_spectrum`、`cos 相位`、`sin 相位`、`频率→到达时间` 等量批量构建 `interp2d_fourier` 插值器（即 A 部分的做法，但作用于频域数据）。

### 步骤 B3：画天线足印图（fig_06）

```python
fig, ax = plt.subplots()
ax.scatter(footprint_pos_x, footprint_pos_y, ...)   # 208 根足印天线
ax.scatter(test_pos_x, test_pos_y, color='r', marker='x')  # 测试位置
```

![天线足印与测试点](/walkthrough_assets/fig_06.png)

> 注：足印图里圆形白点是 208 根采集天线，红色叉是预留的"测试位置"——测试点不在训练集内，用于独立评估插值精度。

### 步骤 B4：对 4 个指定测试点插值并验证

```python
for index in range(nof_test_positions=4):
    this_x, this_y = test_pos_x[index], test_pos_y[index]
    interpolated_pulse = interpolator(this_x, this_y)
    ...
    print('Normalized cross correlation (CC) = %1.4f, time mismatch = %1.3f ns' % (CC_zeroshift, delta_t))
    do_plot_...  # 波形 + 频谱
```

**4 个测试点重跑输出：**

| 测试点 (x, y) / m | 归一化互相关 CC | 时差 time mismatch / ns | 出图 |
|---|---|---|---|
| (-21.80, -50.21) | 0.9983 | −0.050 | fig_07 |
| (114.19, -71.67) | 0.9998 | −0.010 | fig_08 |
| (-220.02, -5.31) | 0.9993 | 0.020 | fig_09 |
| (-286.77, 100.78) | 0.9966 | 0.000 | fig_10 |

![测试点1](/walkthrough_assets/fig_07.png)
![测试点2](/walkthrough_assets/fig_08.png)
![测试点3](/walkthrough_assets/fig_09.png)
![测试点4](/walkthrough_assets/fig_10.png)

每图左边是原始波形（标记）叠插值波形（实线），右边是频谱 —— 几乎完全重合，CC 全在 0.996 以上。

### 步骤 B5：遍历 250 个测试点 + 散射统计图（fig_11、fig_12）

```python
for index in range(nof_test_positions):   # 250 个
    this_x, this_y = test_pos_x[index], test_pos_y[index]
    interpolated_pulse, trace_start_time, abs_spec, phase_spec = interpolator(
        this_x, this_y, full_output=True)
    CC_optimized, delta_t = get_crosscorrelation(test_antenna_data[index], interpolated_pulse)
    timemismatches[index] = delta_t
```

重跑尾部统计输出：
```
warning: negative values in abs_spectrum found: 3 times. Setting to zero.   # 插值偶有负幅度，代码自动置零
...
Start time mismatches stddev (i.e. timing error) = 0.0354 ns
```

**![起始时间误差随核距离](/walkthrough_assets/fig_11.png)**
**![CC 随核距离](/walkthrough_assets/fig_12.png)**

- fig_11：横轴天线 core 距离，纵轴起始时间失配——点子贴中间零线附近，std = **0.0354 ns**，表现优异；
- fig_12：随 core 距离看的互相关 CC——近核处 >0.999，远核（>300 m）略降到 ~0.99，仍符合预期。

结论：**250 个盲测点上，到达时间误差约 3.5 十皮秒，互相关 >0.99** → 该插值对未知足印位置可高保真重建 E 场波形。

---

## 4. Demo C — `minimal_usage_demo.py`：最小可用写法

### 步骤 C1：读数据并实例化插值器

```python
zenith, azimuth, xmax, footprint_x, footprint_y, test_x, test_y, ...
  = demo_helper.read_data_hdf5('demo_shower.h5')
interpinator = sigF.interp2d_signal(footprint_x, footprint_y, footprint_pulses, ...)
```

重跑输出：
```
Initializing interpolator object...
```

### 步骤 C2：单测试点插值 + 图（fig_13）

```python
interpolated_pulse = interpolator(114.19, -71.67)   # 只给一个 (x,y)
do_plot_pulse_and_spectrum(原始, 插值, x, y, cutoff, pol)
```

重跑输出：
```
Interpolating pulse at position x = 114.19, y = -71.67 m
```

这与 Demo B 里第 2 测试点是同一位置，得到完全一致的结果。

![minimal 单点](/walkthrough_assets/fig_13.png)

**最小用法本质就三行：**
```python
import cr_pulse_interpolator.signal_interpolation_fourier as sigF
interpinator = sigF.interp2d_signal(所有天线 x, 所有天线 y, 所有天线 E(t))
pulse = interpolator(x_query, y_query)       # 拿回该点完整时域波形
```

---

## 5. 总结

- **原理**（Demo A）：函数样本极坐标化 → 角度向 FFT 拆成 `cos_k/sin_k` 模式 → 各模式沿半径 cubic 插值 → 反演任意点。低阶模式主导，2 阶截断已足够平滑。
- **全波形**（Demo B/C）：在频域对每个 bin 的**幅度 + 相位**分别用同一极坐标傅里叶插值法（`interp2d_fourier` 批量版），再 `irfft` 组装时域；配合 Hilbert 定时、const-phase、相干性自适应频带。
- **精度**：4 指定测试点 CC∈[0.9966, 0.9998]；250 盲测点起始时间误差 std ≈ 0.0354 ns。
- **易用**：最小用法仅 2 次调用即可插任意位置的完整波形。

---

*本文档由重跑 3 个演示脚本、逐步保存 13 张中间图生成（Agg 后端离屏运行），所有数据均为实际运行输出。*