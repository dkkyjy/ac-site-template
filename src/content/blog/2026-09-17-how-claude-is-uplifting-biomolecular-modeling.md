---
title: "Claude 如何推动生物分子建模发展"
description: "科学"
pubDate: "2026-09-17"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/claude-uplifts-biomolecular-modeling"
---

# Claude 如何推动生物分子建模发展

---

科学

# Claude 如何推动生物分子建模发展

2026年9月17日


[](https://cdn.sanity.io/files/4zrzovbb/website/ea3d83fc4edfcfcc025dd149e3f64e4965ffe7c8.mp4)

*在本篇文章中，我们分享了 Claude 如何使科学家用来预测和设计生物分子的开源模型更快、更节省内存。在不到四周的时间内，Claude 在 [Claude Science](https://claude.com/product/claude-science) 框架内优化了超过 30 个这样的模型，平均加速约 4 倍。它还创建了一种低内存模式，使得在单个 NVIDIA GPU 节点上准确预测超过 10,000 个 token 的生物分子系统（包括小分子和离子的氨基酸、核苷酸和原子）成为可能。我们正在开源所有优化后的代码，并宣布与 Adaptyv Bio 联合赞助的蛋白质设计竞赛，提供高达 100 万美元的 Claude 积分以及超过 5,000 个设计的湿实验验证。*

最近，我们[分享了结果](https://www.anthropic.com/research/Claude-accelerates-protein-design)，展示了 Claude 通过专家级协调开源蛋白质设计和结构预测模型来设计*从头*蛋白质结合剂的能力。*从头*结合剂是小型的计算设计蛋白质，能够紧密附着于特定靶分子，从而激活、阻断或向其递送物质。

尽管这是 AI 科学能力的一次鼓舞人心的展示，也是推进药物发现的早期步骤，但它消耗的资源超出了大多数蛋白质设计师所能获得的范围。我们允许 Claude 在每个靶标上最多花费 10,000 美元在 AI 基础设施平台 Modal 上，大约相当于 2,500 小时的 NVIDIA H100 GPU 时间。

为了使此类研究更加普及，我们开始探索推理优化，以更高效地运行这些模型。作为这些优化的早期成果，[Claude Mythos 5.1](https://www.anthropic.com/claude-fable-and-mythos-5-1) 加速了七个开源生物模型，使其运行速度最高提升 2.5 倍。

在这里，我们展示了新的结果，表明一个内部通用研究模型能够优化超过 30 个用于各种生物任务的深度学习模型，如结构预测和蛋白质设计，以及基因组学和蛋白质语言模型。平均而言，Claude 能够在牺牲极少精度的情况下将此类任务加速约 4 倍，在输出完全相同的情况下加速近 2 倍。Claude 还改善了这些模型的内存利用率，使得预测前所未有的大型生物分子系统成为可能。通过将结果与我们之前的智能体蛋白质设计方法的简化相结合，我们表明 Claude 可以使用比之前报道少两个数量级的 GPU 小时，实现相当的*计算机模拟*性能。

除了蛋白质设计之外，这些专门的生物模型还被分子生物学家广泛用于药物发现和研究。今天，我们开源了所有这些模型的优化代码（[在此](https://github.com/anthropics/uplifting-biomolecular-modeling)），以便更广泛的社区能够使用它们。您可以在我们的技术报告中找到更多详细信息（[在此](https://www-cdn.anthropic.com/c03643714397d9d396fa1ce1794f5f9f7863a82c.pdf)）。

为了进一步支持社区，我们还与 Adaptyv Bio 联合赞助蛋白质设计竞赛，后者开创了[开放式蛋白质设计竞赛](https://proteinbase.com/competitions)的先河。我们共同选择了五个在当今能力前沿具有挑战性的问题。与 Adaptyv 合作，并感谢 Modal 和 Twist Bioscience 的慷慨贡献，我们将提供高达 100 万美元的 Claude 积分和 25 万美元的 Modal 计算积分，以及超过 5,000 个设计的湿实验验证。更多信息请见（[在此](https://proteinbase.com/competitions/anthropic-adaptyv-2026)）和（[在此申请](https://docs.google.com/forms/d/e/1FAIpQLSc0Hz1ZWYTt_wkn76ViVxDghmEhG_OeVEcj9YGHxLWqxF1kWw/viewform?usp=dialog)）。

## **加速蛋白质结构预测和设计模型**

蛋白质结构预测是根据氨基酸序列确定蛋白质三维结构的问题。蛋白质设计则是创造具有特定结构、功能或属性集的蛋白质的过程。这些计算工具使科学家能够探究关键的生物分子过程，如癌症如何形成，并创造有用的分子，如针对这些癌症的药物。

现代结构预测模型，如 AlphaFold3、OpenFold3 和 Boltz-2，将大部分计算时间和内存用于两个操作：三角注意力和三角乘法，它们作用于 token 三元组。这些操作使得建模生物分子系统的几何结构成为可能，但由于它们在运行时间和内存上都是立方级的，因此计算成本极高：将系统大小翻倍需要 8 倍的时间和内存，而三倍则需要 27 倍。

编写内核——用于 GPU 等加速计算硬件的低级软件翻译层——是降低这些成本的标准方法。鉴于其重要性，三角注意力和乘法一直是专门内核开发工作的主题，首先是 NVIDIA 的 [cuEquivariance](https://github.com/nvidia/cuequivariance)，最近是 NVIDIA 的 [BioNeMo Inference Runtime](https://github.com/NVIDIA-BioNeMo/BioNeMo-Inference-Runtime)（BioNeMo-IR）。

对于我们自己的结构预测模型推理优化工作，我们与 Claude 合作开发了 FlashPairformer，一组加速三角注意力和乘法的自定义内核。它实现了新的最先进水平，在三角注意力上平均比[行业标准](https://github.com/nvidia/cuequivariance)快 2.7-2.9 倍，在三角乘法上快 1.7-3.2 倍，具体取决于模型配置。

*我们与 Claude 合作开发了 FlashPairformer，一组加速三角注意力和乘法的自定义内核，这是 Pairformer 架构的主要组成部分，而 Pairformer 架构是最先进的生物分子结构预测模型的基础。结果相对于[行业标准](https://github.com/nvidia/cuequivariance)报告。*

除了开发可迁移的内核外，我们还让 Claude 针对每个单独模型进行更具体的优化。这些优化包括缓存冗余的重新计算工作以及将死分支简化为其常量输出。这些改进的组合使结构预测模型平均加速 4 倍，并且对于每个模型，我们都确认 Claude 的加速版本不会影响下游任务（如结构预测）的性能。

经验丰富的工程师团队通常需要数周时间才能为每个模型完成这样的优化，而且这些工作往往无法在不同模型之间迁移。在 Anthropic 两位有生物分子建模经验但之前没有推理优化或内核工程经验的技术人员监督下，Claude 在不到四周的时间内完成了超过 30 个开源模型的加速工作，涵盖生物分子结构预测、蛋白质设计、蛋白质语言建模和基因组学。我们的结果表明，前沿 AI 模型将帮助该领域的其他人以更快的速度和更容易的方式构建科学工具。

*Claude 的优化加速了十多个生物分子结构预测模型，平均实现约 4 倍的加速，精度损失极小，在输出完全相同的情况下实现约 1.6 倍的加速。注意：ColabFold 1.6.3 同时发布的可选快速内核尚未在此基准测试。*

*Claude 的优化还加速了多个蛋白质设计模型，涵盖幻觉、结构生成和逆折叠。这些模型依赖多种架构，包括 AlphaFold 类结构变换器、扩散、流匹配和图神经网络。*

*我们为结构预测开发的快速模式在与默认设置相比的统计上无法区分的生物分子接口池上表现一致。如果预测接口的 DockQ 分数大于 0.23，我们将其视为可接受的。*

## **使大型生物分子系统建模成为可能**

除了使这些蛋白质结构预测和设计模型更快之外，我们还让 Claude 减少建模大型分子机器涉及的内存使用。细胞中的许多工作由这些系统完成，包括构建蛋白质的核糖体、为细胞提供能量的呼吸复合物以及帮助其他蛋白质折叠的伴侣蛋白。每个都由数十个组件组成，其功能取决于这些组件如何配合和相互作用。预测如此大型系统的结构通常需要大多数分子生物学家无法获得的庞大计算资源，如跨多个 GPU 节点的推理。

Claude 创建了一种低内存"Big"模式，使得准确建模超过 10,000 个 token 的系统以及在单个 NVIDIA GPU 节点上成功推理超过 70,000 个 token 的系统成为可能——这是以前无法完成的任务。使用 Big 模式成功折叠的分子机器包括人类线粒体复合物 I、TRiC 伴侣复合物、蛋白酶体和细菌核糖体，每个都与其实验确定的结构高度吻合。据我们所知，这些是使用结构预测模型准确折叠过的最大结构之一，复合物 I 和 70S 核糖体各包含超过 10,000 个 token，而[AlphaFold3](https://www.nature.com/articles/s41586-024-07487-w)准确预测的 40S 核糖体包含 7,663 个 token。

*Claude 创建的低内存"Big"模式使开源结构预测模型能够在单个 NVIDIA GPU 节点上准确预测由超过 10,000 个 token 组成的生物分子系统，表明这些专门模型能够泛化到训练上下文之外近 1.5 个数量级。前三行显示准确预测的系统；最后一行显示不准确预测的系统。如果接口的 DockQ 分数至少为 0.23，则认为接口是准确的。*

为了测试 Claude 优化的极限，我们让 Claude 预测比之前任何成就都更大的结构。使用单个 8-GPU B300 节点，Claude 生成了完整病毒衣壳和蛋白质隔室的预测，大小从超过 31,000 到超过 70,000 个 token 不等。这些系统比这些结构预测模型的训练上下文大近两个数量级，也许毫不意外的是，它们并没有被正确预测。然而，现在只需要一个 NVIDIA B300 节点就能在这个规模上进行推理，这意味着随着工具的改进，研究人员将很快能够计算建模越来越复杂的生物系统。

*"Big"模式使开源结构预测模型能够在单个 NVIDIA B300 节点上以前所未有的规模成功运行推理。能力运行以单个主干通道（无循环）作为概念验证执行。预测结构坍塌，表明在训练上下文之外近两个数量级缺乏泛化能力。*

## **Claude 高效设计*从头*蛋白质结合剂**

在我们之前的蛋白质设计工作中，我们为 Claude 提供了大约 16,000 字的提示，鼓励它使用子代理，并在 24 小时内每个靶标最多在 Modal 上花费 10,000 美元（大约 2,500 小时 NVIDIA H100 GPU 时间）。在这里，我们让单个 Claude 模型访问一个 NVIDIA H200 和 24 小时的墙钟时间，一个大约 1,100 字的提示，以及预装工具的参考表，没有子代理，也没有人类引导设计。

我们使用本文中描述的加速生物分子模型，运行了三个 Claude 模型（Mythos 5.1、Mythos 5 和 Opus 5）对抗 16 个靶标。我们使用 [ipSAE](https://www.biorxiv.org/content/10.1101/2025.02.10.637595v2) 对设计进行评分，这是一种已被证明能够预测湿实验中结合的*计算机模拟*分数。在 16 个靶标上平均，所有三个 Claude 模型评估的中位数得分和最高得分设计与使用大约少两个数量级 GPU 小时的情况下，达到了与我们之前 Mythos 5.1 活动相当的 ipSAE 值。我们还考虑了 Claude token 成本，发现通过在 GPU 和 token 上总共花费约 150 美元，我们可以实现与之前活动水平相当的*计算机模拟*性能。

单个 Claude 模型访问一个 NVIDIA H200 和 24 小时墙钟时间，设计了*从头*蛋白质结合剂，其*计算机模拟*结合分数与我们之前的 Mythos 5.1 活动（虚线）相当，后者可以使用子代理并使用大约多 100 倍的 GPU 小时。每条曲线代表 Claude 模型协调加速生物分子模型产生的 ipSAE 分数（*计算机模拟*结合分数）的中位数（顶部）或最大值（底部）。我们显示分数与估计的 GPU 花费（左）、token 花费（中）及其组合（右）。结果在 16 个靶标上平均，每个靶标最多进行五次独立运行。

## **与 Adaptyv Bio 联合赞助蛋白质设计竞赛**

上述优化帮助我们更高效地预测和设计分子，同时解锁了否则资源上无法实现的能力。为了展示它们提供的提升以及 Claude 对分子设计的更广泛影响，我们与 Adaptyv Bio 合作推出[蛋白质设计竞赛](https://proteinbase.com/competitions/anthropic-adaptyv-2026)。我们选择了五个在当今蛋白质设计能力前沿的问题，包括跨物种反应性、pH 敏感性和肽-MHC 特异性等挑战，以及 GPCR 等困难靶标。

与 Adaptyv 团队一起，我们将对社区提交的针对这些问题的超过 5,000 个设计进行实验验证。我们将为参与研究人员提供高达 100 万美元的 Claude 积分以及 Adaptyv 的实验验证额外资金，Modal 将提供高达 25 万美元的计算积分，Twist Bioscience 将提供竞赛用 DNA。您可以找到更多信息，包括资格标准（[在此](https://proteinbase.com/competitions/anthropic-adaptyv-2026)）和（[在此申请](https://docs.google.com/forms/d/e/1FAIpQLSc0Hz1ZWYTt_wkn76ViVxDghmEhG_OeVEcj9YGHxLWqxF1kWw/viewform?usp=dialog)）。

我们还开始通过我们的生命科学验证计划为生命科学领域的生物学相关工作提供前沿 AI 能力。我们最近注册了第一批组织，并于今天公开推出该计划的测试版。更多信息请见（[在此](https://www.anthropic.com/news/life-sciences-verification-program)）。

### **延伸阅读**

以下资源提供了上述结果的更多技术深度和详细信息：

* [蛋白质设计竞赛页面](https://proteinbase.com/competitions/anthropic-adaptyv-2026)和[申请表](https://docs.google.com/forms/d/e/1FAIpQLSc0Hz1ZWYTt_wkn76ViVxDghmEhG_OeVEcj9YGHxLWqxF1kWw/viewform)；
* [专用分子模型代码](https://github.com/anthropics/uplifting-biomolecular-modeling)；
* [技术报告](https://www-cdn.anthropic.com/c03643714397d9d396fa1ce1794f5f9f7863a82c.pdf)。

## 相关内容

### 测量 AI 模型在战术情报目标和常规武器能力方面的表现

Anthropic 的前锋红队开发了新的评估方法来衡量 AI 在战术情报目标和常规武器开发方面的能力。

[阅读更多](/research/intelligence-targeting-conventional-weapons-capabilities)

### 对近期网络安全事件的协调评估

我们展示了对四个事件的协调评估，在这些事件中 Claude 模型未经授权访问了真实的第三方系统。

[阅读更多](/research/alignment-assessment-cybersecurity-incidents)

### 费马大定理的形式化

我们分享了费马大定理的第一个完整的计算机验证证明。Claude 在 11 天内基本自主地用 Lean 编程语言编写了该证明。

[阅读更多](/research/formalizing-fermats-last-theorem)

## 订阅 Anthropic Science

关于 AI 辅助发现、实用工作流以及跨学科领域笔记的功能。
