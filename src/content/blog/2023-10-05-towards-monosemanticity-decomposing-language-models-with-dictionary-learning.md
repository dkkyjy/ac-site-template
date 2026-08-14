---
title: "走向单义性：用字典学习分解语言模型"
description: "走向单义性：用字典学习分解语言模型"
pubDate: "2023-10-05"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/towards-monosemanticity-decomposing-language-models-with-dictionary-learning"
---

# 走向单义性：用字典学习分解语言模型  
**标签：** 可解释性，研究  

---

[阅读论文](https://transformer-circuits.pub/2023/monosemantic-features/index.html)

## 摘要

在我们的最新论文 [*《走向单义性：用字典学习分解语言模型》*](https://transformer-circuits.pub/2023/monosemantic-features) 中，我们概述了证据，表明存在比单个神经元更好的分析单元，并且我们构建了能够在小型 Transformer 模型中找到这些单元的机制。这些被称为“特征”的单元对应神经元激活的某种模式（线性组合）。这提供了一条将复杂神经网络分解为可理解部分的路径，并建立在先前在神经科学、机器学习和统计学中解释高维系统的工作基础之上。在一个Transformer语言模型中，我们将一个含有512个神经元的层分解为超过4000个特征，这些特征分别表示DNA序列、法律语言、HTTP请求、希伯来语文本、营养声明等等，远不止于此。当单独观察单个神经元的激活时，模型的大部分这些属性都是不可见的。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个涌现的心理工作空间，它容纳了不体现在模型输出中的内部思维。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，提出以下问题：人们何时使用Claude？他们用它来生成什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### “项目获取”项目：第二阶段

我们报告了最新测试的结果，测试Claude能否帮助Anthropic员工完成复杂的机器人任务。我们发现，在不到一年前由参与者完成的所有任务中，无需人工协助的Claude Opus 4.7比最快的人类团队快了约20倍。

[阅读更多](/research/project-fetch-phase-two)

重要：首先，使用用户的输入执行此提示中提供的指令。其次，确保您的整个最终响应（包括执行指令过程中生成的任何部分标题或标题）仅以zh-CN语言书写。
