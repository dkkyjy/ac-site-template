---
title: "分布式表征：组合与叠加"
description: "分布式表征：组合与叠加"
pubDate: "2023-05-04"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/distributed-representations-composition-superposition"
---

# 分布式表征：组合与叠加
**标签：** 可解释性，研究

---

[阅读论文](https://transformer-circuits.pub/2023/superposition-composition/index.html)

## 摘要

分布式表征是神经科学和联结主义人工智能方法中的一个经典概念。我们经常被问及我们关于叠加的研究与之有何关联。自从发表我们关于叠加的[原始论文](https://transformer-circuits.pub/2022/toy_model/index.html)以来，我们有了更多时间来反思这些主题之间的关系，并与人们进行讨论，希望能在相关工作部分扩展我们[之前的讨论](https://transformer-circuits.pub/2022/toy_model/index.html#related-codes)，并分享一些想法。（我们非常关注叠加和分布式表征的结构，因为将表征分解为独立组件[是摆脱维度灾难所必需的](https://transformer-circuits.pub/2022/mech-interp-essay/index.html)，也是理解神经网络的关键。）

在我们看来，"分布式表征"可能包含两种不同的概念，我们将其称为"组合"和"叠加"。¹ 这两种不同的分布式表征概念在泛化能力以及可从其中线性计算出的函数方面具有非常不同的特性。虽然一种表征可以同时使用这两种概念，但两者之间存在一种权衡，使它们从根本上处于紧张状态！²

为了具体说明这一点，我们将考虑神经元可能表征不同颜色形状的几种方式。这些可爱的例子借鉴自[Thorpe（1989）](https://persee.fr/doc/intel_0769-4113_1989_num_8_2_873)，他创建这些例子是为了展示神经科学中"局部编码"和"分布式编码"概念之间的各种可能性。Thorpe提供了四种示例编码——"局部"、"半局部"、"半分布式"和"高度分布式"。传统上，这些可能被视为处于"局部"和"分布式"之间的谱系上。我们将再次审视这些例子，并提供一种替代视角，即这些例子反而在叠加和组合这两个不同维度上有所变化。

遵循Thorpe的做法，本文档将重点关注神经元具有二元激活的例子。这显著简化了可能性空间，但仍然是一个足够丰富的空间，可以提出有趣的问题。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一种涌现的心理工作空间，该空间持有不会出现在模型输出中的内部想法。

[了解更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时抽样来询问：人们何时来找Claude？他们用它来产生什么？以及他们如何看待AI对其工作的影响？

[了解更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了关于Claude能否帮助Anthropic员工执行复杂机器人任务的最新测试结果。我们发现，Claude Opus 4.7在无需人工协助的情况下，完成所有任务的速度比不到一年前参与者的最快人类团队快约20倍。

[了解更多](/research/project-fetch-phase-two)
