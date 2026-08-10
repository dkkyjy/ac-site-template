---
title: "CRPropa3 - 软件功能总结"
description: "CRPropa3 功能总结与使用文档。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["crpropa3", "物理"]
---

# CRPropa3 功能总结

## 是什么

**CRPropa3** 是一个 C++ 天体粒子物理模拟框架，用于模拟**超高能宇宙射线 (UHECR)**、**伽马射线** 和**中微子**在宇宙中的传播。提供了完整的 Python 绑定 (SWIG)。

---

## 核心功能模块

### 1. 粒子传播 (Propagation)

| 模块 | 用途 |
|------|------|
| `SimplePropagation` | 直线传播（无磁场偏转），适合一维模拟或仅能量损失 |
| `PropagationCK` | **Cash-Karp 自适应步长**传播，在磁场中精确追踪带电粒子轨迹 |
| `PropagationBP` | **Boris Push** 算法，适用强磁场环境 |

### 2. 磁场模型 (MagneticField)

| 磁场 | 描述 |
|------|------|
| `UniformMagneticField` | 均匀磁场（简单测试用） |
| `JF12Field` / `JF12FieldSolenoidal` | **Jansson-Farrar 12** 银河系磁场模型 |
| `PlanckJF12bField` | Planck 优化的 JF12 模型 |
| `PT11Field` / `TF17Field` | Pshirkov-Tinyakov 2011 模型 |
| `TurbulentField` | 湍流磁场 |
| `MagneticFieldGrid` | 自定义网格磁场 |
| `KST24Field` / `UF23Field` | 最新银河系磁场模型 (2023–2024) |
| `ArchimedeanSpiralField` / `LogarithmicSpiralField` | 螺旋磁场 |
| `CMZField` | 银河系中心磁场 |
| `PeriodicMagneticField` | 周期边界磁场 |
| `MagneticDipoleField` | 偶极磁场 |

### 3. 光子背景场 (PhotonField)

| 光子场 | 模拟的宇宙辐射背景 |
|--------|-------------------|
| `CMB` | **宇宙微波背景辐射** (2.7K) |
| `IRB_Dominguez11` / `IRB_Finke10` / `IRB_Finke22` | **红外/光学背景辐射**（多个模型可选） |
| `IRB_Franceschini08` / `IRB_Gilmore12` / `IRB_Kneiske04` | |
| `IRB_Saldana21` / `IRB_Stecker05` / `IRB_Stecker16_lower/upper` | |
| `URB_Fixsen11` / `URB_Nitu21` / `URB_Protheroe96` | **射电背景辐射** (MHz–GHz) |
| `TabularPhotonField` | 用户自定义表格光子场 |
| `TabularSpatialPhotonField` | 含空间分布的自定义光子场 |

### 4. 物理相互作用

| 模块 | 粒子类型 | 物理过程 |
|------|---------|---------|
| `ElectronPairProduction` | γ + γ_bg → e⁺e⁻ | 伽马射线被背景光子吸收产生电子对 |
| `PhotoPionProduction` | p/Fe + γ_bg → Δ⁺ → pπ⁰/nπ⁺ | **光介子产生**（GZK 效应的主过程） |
| `PhotoDisintegration` | A + γ_bg → (A-1) + n | 重核被背景光子击碎 |
| `NuclearDecay` | 不稳定核 → 子核 | 放射性衰变（β⁺/β⁻/α） |
| `SynchrotronRadiation` | e⁻/p + B → γ | **同步辐射**（在磁场中） |
| `EMInverseComptonScattering` | e⁻ + γ_bg → e⁻ + γ | 逆康普顿散射 |
| `AdiabaticCooling` | 所有粒子 | 宇宙膨胀绝热冷却 |

### 5. 观察者 (Observer)

| 模块 | 用途 |
|------|------|
| `ObserverSurface` | 粒子到达某几何面时触发（球面/柱面/平面） |
| `ObserverThresholdEject` | 超出距离/红移阈值时触发 |
| `ObserverSmallDeflection` | 偏转角小于阈值时记录 |
| `ObserverPhotonVeto` | 过滤伽马射线 |
| `ObserverNeutrinoVeto` | 过滤中微子 |
| `ObserverElectronVeto` | 过滤电子 |
| `ObserverNucleusVeto` | 过滤原子核 |
| `ObserverParticleIdVeto` | 按粒子 ID 过滤 |
| `ObserverFeature` | 自定义特征提取 |
| `ObserverDetectAll` | 检测所有粒子（记录到达的次级粒子） |

### 6. 输出格式

| 输出 | 格式 |
|------|------|
| `TextOutput` | 文本文件（支持 1D/3D/轨迹等输出分组——`Trajectory1D`, `Trajectory3D`, `Event1D`, `Event3D`, `Everything`） |
| `HDF5Output` | HDF5 格式（需编译时启用） |
| `ROOTOutput` | ROOT 格式（需编译时启用） |
| `PhotonOutput1D` | 专门输出光子的一维数据 |
| 自定义 | 通过 `ObserverFeature` 提取任意物理量 |

### 7. 粒子源 (Source)

- `SourceParticleType` — 粒子种类（质子/铁核/伽马/中微子/自定义）
- `SourceEnergy` — 单能量/幂律谱/自定义谱
- `SourcePosition` — 点/球壳/球体/自定义分布
- `SourceDirection` — 各向同性/锥形/指向
- `SourceMultipleParticleTypes` — 混合成分
- `SourceRedshift` — 红移

### 8. 工具函数

| 函数 | 用途 |
|------|------|
| `nucleusId(A, Z)` | 核种→PDG 粒子 ID |
| `isNucleus(pid)` | 判断是否为原子核 |
| `nucleusName(pid)` | 粒子 ID→名称（C++ API，SWIG 绑定可能缺失） |
| `nucleusMass(pid)` | 粒子 ID→质量（C++ API，SWIG 绑定可能缺失） |
| `fromMagneticField(strength)` | 根据磁场强度创建场对象 |

### 9. 宇宙学

- 红移相关传播
- `ObserverRedshiftWindow` — 红移窗口过滤
- `MagneticFieldEvolution` — 磁场随红移演化

---

## 典型的模拟任务

### 任务 1: GZK 截断模拟

研究超高能质子与 CMB 反应的能量损失：

```
输入: 100 EeV 质子源 → 传播数百 Mpc → 观察到达地球的能量分布
```

**产出**: 不同距离下质子能谱 → 验证 GZK 截断 (≈50 EeV)

---

### 任务 2: 宇宙线各向异性

模拟宇宙射线在银河系磁场中的偏转：

```
磁场: JF12Field | 源: 银河系外均匀分布 | 观察: 到达方向
```

**产出**: 到达方向的各向异性图，与观测数据对比

---

### 任务 3: 重核组分研究

不同核种（质子、氦、碳、铁）的传播差异：

```
PhotoDisintegration 模拟重核碎裂，对比不同核种的能量损失率
```

**产出**: 到达地球的核组分随能量的变化

---

### 任务 4: 伽马射线天文

高能伽马射线被 EBL/CMB 吸收：

```
ElectronPairProduction 模拟 γ→e⁺e⁻，计算不同红移源的光学深度
```

**产出**: 伽马射线能谱衰减曲线，AGN/GRB 可观测性预测

---

### 任务 5: 中微子产生

pγ → π⁺ → μ⁺ν_μ → e⁺ν_eν̅_μν_μ 级联：

```
光介子产生 → 衰变链 → 到达地球的中微子流
```

**产出**: 宇宙中微子流强预测（与 IceCube 数据对比）

---

### 任务 6: 同步辐射建模

电子/质子在银河系磁场中的同步辐射：

```
SynchrotronRadiation 模块，计算辐射谱
```

**产出**: 射电/伽马波段辐射分布（超新星遗迹、活动星系核）

---

### 任务 7: 跨磁场模型对比

| 磁场模型 | 适用场景 |
|----------|---------|
| JF12 | 银河系大尺度磁场基准模型 |
| PlanckJF12b | 与 Planck 数据一致的优化版本 |
| PT11 | 侧重银河系旋臂结构 |
| TF17 | 高分辨率数值模拟磁场 |
| TurbulentField | 分子云/超新星遗迹湍流磁场 |
| KST24 | 2024 最新模型 |
| CMZField | 银河系中心区域 |
| UniformMagneticField | 简单测试/基准验证 |

---

## 技术指标

| 指标 | 数据 |
|------|------|
| Python API 总数 | **472** |
| 支持 Python | 3.12 / 3.13 / **3.14**（需 SWIG ≥ 4.4.1） |
| 支持的传播算法 | Simple, Cash-Karp, Boris Push |
| 宇宙学红移支持 | z ≤ 5（扩展模型可更高） |
| 最小能量 | ~1 MeV（同步辐射/IC） |
| 最大能量 | > 10¹² GeV（宇宙射线） |
| 数据文件 | CMB/IRB/URB 背景谱（表格，存放于 `share/crpropa/`） |
| 编译依赖 | SWIG ≥ 4.4.1, CMake, OpenMP, GSL, FFtw3, zlib |
| 可选依赖 | HDF5, ROOT |

---

## 已知局限

1. **EPP + PPP 同时开启**时，若粒子能量较低 (< 1 EeV)，可能在 pair production 循环中计算量爆炸 → 建议高能场景 (> 10 EeV) 才同时启用
2. `nucleusName()` / `nucleusMass()` 等部分 C++ 函数未通过 SWIG 暴露到 Python 绑定
3. HDF5/ROOT 输出需编译时额外启用
4. 构建依赖较多（SWIG, OpenMP, GSL, FFtw3, HDF5 可选）

---

## 安装验证（macOS Apple Silicon + Python 3.14）

```bash
# 源码构建
git clone https://github.com/CRPropa/CRPropa3.git
cd CRPropa3
mkdir build && cd build
cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/.local \
         -DPYTHON_EXECUTABLE=$(which python3)
make -j$(sysctl -n hw.logicalcpu)
make install

# 验证
python3 -c "import crpropa; print(len(dir(crpropa)))"  # => 472
```

**关键修复**: Python 3.14 需 SWIG ≥ 4.4.1（4.3.1 因缺少 `ht_token` slot 支持而编译失败）。

---

*文档生成时间: 2026-07-23*
*Python 3.14 / SWIG 4.4.1 / macOS Apple Silicon / CRPropa3 (源码构建)*
