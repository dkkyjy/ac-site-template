---
title: "GaussPy - AGD 流程走查"
category: "专项工具 / 科研"
description: "GaussPy AGD 中间流程可复现走查：合成谱到分量拟合的完整流程。"
pubDate: "2026-08-12"
badge: "guide"
tags: ["gausspy", "物理"]
---

# AGD 中间流程示例:合成谱到分量拟合的完整可复现走查

*2026-08-11T05:32:38Z by Showboat 0.6.1*

本走查用可复现的代码块,重新讲解我们构建的 AGD 中间流程示例:从合成谱出发,经过正则化导数、分量探测、去混合、单相与两相分解,到 AGD_double 吸收+发射混合。每一节都对应 Lindner et al. 2015 论文的一个章节与 gausspy 代码的一个阶段,输出可被 make 一键重生成。

先看公共辅助模块 common.py:它提供两条可复现例程——合成数据 synth_data()(返回速度轴 v、含噪数据 data、无噪声真值 true、真实分量 comps)与正则化导数 reg_ders()(python 模式下 TVdiff 四阶链)。

```bash
sed -n '1,19p' common.py
```

```output
"""Shared helpers for AGD walkthrough figures."""
import os
import numpy as np
from gausspy.tvdiff import TVdiff

_PKG_ROOT = os.path.abspath(os.path.join(os.path.dirname(__file__), '..', '..'))

def synth_data(seed=0, n=512, noise_rms=0.05):
    rng = np.random.default_rng(seed)
    v = np.arange(n) - n / 2.0
    # (amp, FWHM, mean) in tau space
    comps = [(1.2, 6.0, -40.0), (0.8, 4.0, 0.0),
             (1.5, 12.0, 35.0), (0.5, 3.0, 60.0)]
    true = np.zeros(n)
    for a, fw, mu in comps:
        s = fw / 2.354820045
        true += a * np.exp(-(v - mu) ** 2 / (2 * s ** 2))
    data = true + rng.normal(0, noise_rms, n)
    return v, data, true, comps
```

### 第 1 步:合成谱与正则化导数 (fig1_2)

合成谱建在光学深度空间(tau space),含 4 个真实高斯分量,叠加高斯噪声(rms 0.05)。这是 AGD 的正确输入形态——吸收谱需先转 tau = -ln(I/I0),发射谱用线性强度。

用 python 模式(论文 mode="python")对数据链式调用 TVdiff 四次,得到一至四阶正则化导数 u1..u4。alpha = 10^log_alpha 把论文的对数编码还原为 TV 正则参数。运行 fig1_2.py 可复现该图与中间向量。

```bash
/Users/dkk/anaconda3/bin/python fig1_2.py
```

```output
saved /Users/dkk/storage/github/gausspy/demos/fig1_2_input_derivs.png
```

![第1步:合成谱与正则化导数](/gausspy/fig1_2_input_derivs.png)

### 第 2 步:掩码探测 (fig2)

检测分量的核心是二阶导数 u2 的负区:对每个通道,当 u2 低于该时刻零均值二阶导的 -5 倍标准差(即 < -5 sigma,代码中 log_cut),且数据本身高于噪声(SNR),且四阶导数 u4 > 0(支持局部极值),且 u2 < -5 sigma 时,标记为候选峰值。最后用三阶导数 u3 的过零确定精确中心。运行 fig2.py 会输出探测到的中心(速度)、内禀 FWHM 与幅值初值。

```bash
/Users/dkk/anaconda3/bin/python fig2.py
```

```output
saved /Users/dkk/storage/github/gausspy/demos/fig2_masks_detect.png
detected: [-39.5  -0.5  34.5] FWHMs: [24.61 26.14 21.87] amps: [1.2   0.715 1.413]
```

![第2步:掩码与分量探测](/gausspy/fig2_masks_detect.png)

### 第 3 步:去混合 (fig3 / fig3b)

探测到分量后,幅值初值 naive_amp 直接用峰值处数据近似,但重叠分量会互相抬高,故用带宽矩阵 FF 反演去混合。FF 元素为两高斯(由各自的 FWHM 与位置)在对方中心处的交叠、并以列宽(即第 j 列分量的 FWHM)归一——因此当各分量 FWHM 不同时 FF 非对称(max|FF-F^T| != 0)。解线性方程组 pairs = lstsq(FF, naive) 得去混合幅值。

稀疏例(fig3):重叠小,naive 与 deblend 差别很小,FF 近似对称(max|FF-F^T| 约 0.006)。

拥挤例(fig3b):两峰相距仅 15 通道,FWHM ~20,高度重叠。naive 幅值 [1.081, 0.8] 明显低估真值 [1.5, 1.0],去混合后 [0.981, 0.72] 回到真值附近。

```bash
/Users/dkk/anaconda3/bin/python fig3.py
```

```output
FF (non-symmetric? max|FF-F^T|= 0.006120653907271588 )
amps_naive : [1.2   0.715 1.413]
amps_deblend: [1.198 0.713 1.408]
all>0? accepted: True
saved /Users/dkk/storage/github/gausspy/demos/fig3_deblend.png
```

![第3步:稀疏去混合](/gausspy/fig3_deblend.png)

```bash
/Users/dkk/anaconda3/bin/python fig3b.py
```

```output
detected centers: [-8.5  9.5] FWHMs: [18.91 21.31]
amps_naive : [1.081 0.8  ]
amps_deblend: [0.981 0.72 ] accepted: True
true amps  : [1.5, 1.0]
saved /Users/dkk/storage/github/gausspy/demos/fig3b_deblend_crowded.png
```

![第3步:拥挤去混合](/gausspy/fig3b_deblend_crowded.png)

### 第 4 步:单相最终拟合 (fig4)

去掉混合后的初值(幅值、内禀 FWHM、中心)进入 lmfit 非线性最小二乘做最终优化(perform_final_fit)。关键观察:二阶导数给出的内禀 FWHM 初值往往偏宽——本例如探测自带 FWHM ~21-26,而真值约 6-12,LM 拟合后收窄到约 6-12(rchi2 降到 1.73)。这就是 deriv 宽度初值粗糙、变形(reshape)与最终 LM 校正的重要体现。

```bash
/Users/dkk/anaconda3/bin/python fig4.py
```

```output
status 1  N_components(guess)= 3  n_fit= 3  rchi2= 1.734
guess amps/width/mean:
   [1.408 1.198 0.713]
   [21.868 24.605 26.143]
   [ 34.5 -39.5  -0.5]
final amps/width/mean:
   [1.487 1.195 0.793]
   [11.836  6.143 -4.162]
   [ 3.5132e+01 -4.0021e+01 -2.4000e-02]
saved /Users/dkk/storage/github/gausspy/demos/fig4_finalfit.png
```

![第4步:单相最终拟合](/gausspy/fig4_finalfit.png)

### 第 5 步:两相分解 (fig5)

单相只用一个 alpha 拟合一种尺度,而真实谱常同时含窄(CNM,FWHM~3)与宽(WNM,FWHM~20)分量。两相把分解分成两趟:

- 第一相 phase="one" 用小 alpha1 检测窄分量;第二相 phase="two" 从残差出发,在去除窄分量后的残差里检测宽分量(通过 fitmask 窗口围住窄分量避免重复)。
- alpha 是关键超参数:论文训练数据 log_a1=1.12 是为它的训练谱定的。本例窄分量 FWHM 只有 3,需要调小 log_a1(0.5) 才能探测到;宽分量用较大的 log_a2(2.7)。这正说明 alpha 必须针对数据形态学习的道理。

本图四面板:输入+真值、两相最终拟合(含各分量)、残差、初值分量。log_a1=0.5, log_a2=2.7,最终 n=3, rchi2=1.21。

```bash
/Users/dkk/anaconda3/bin/python fig5.py
```

```output
status 1  n_guess= 3  n_fit= 3  rchi2= 1.211
saved /Users/dkk/storage/github/gausspy/demos/fig5_twophase.png
```

![第5步:两相分解](/gausspy/fig5_twophase.png)

### 第 6 步:AGD_double 吸收+发射混合 (fig6)

到这一步把吸收与发射同时解:Murray et al. 2018 的混合 21 cm 方法。吸收谱在 tau 空间、发射谱用线性强度,两条谱共用一个速度轴。AGD_double 同时输出 best_fit_parameters(吸收)与 best_fit_parameters_em(发射),fit_labels 标记每个发射分量是纯发射(0)还是被吸收分量耦合(1)。

本例吸收有 2 个分量,发射 3 个。abs log_alphas = (0.7, 2.4), em log_alpha = 1.0。rchi2_abs=0.9,发射标签 [1,1,0]——前两个发射峰被吸收耦合。图上半为吸收(tau 空间)数据+拟合+各分量,下半为发射数据+拟合+分量(绿=耦合,橙=纯发射)。

```bash
/Users/dkk/anaconda3/bin/python fig6.py
```

```output
saved /Users/dkk/storage/github/gausspy/demos/fig6_agd_double.png
abs n= 2  em n= 3  rchi2_abs= 0.9
em labels (0=em-only,1=absorb-coupled): [1. 1. 0.]
```

![第6步:吸收+发射混合](/gausspy/fig6_agd_double.png)

### 复现:make 一键重生成全部图

Makefile 把每一张图与其脚本、common.py 的依赖关系写清楚。以上六张图全部由各自脚本生成,键入 make 即可在一个命令里重放整条 AGD 流水线(合成→导数→探测→去混合→拟合→混合)。`make clean` 删除所有生成的图。
