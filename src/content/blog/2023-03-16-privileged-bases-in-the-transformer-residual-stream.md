---
title: "Transformer 残差流中的特权基"
description: "Transformer 残差流中的特权基"
pubDate: "2023-03-16"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/privileged-bases-in-the-transformer-residual-stream"
---

# Transformer 残差流中的特权基  
**标签：** 可解释性，研究

---

[阅读论文](https://transformer-circuits.pub/2023/privileged-basis/index.html)

## 摘要

我们对 Transformer 架构的数学理论表明，残差流中的单个坐标不应具有特殊意义（即基方向在某种意义上应该是“任意的”，并且不比随机方向更可能编码信息）。近期研究表明，这一观察在实践中是错误的。我们研究了这一现象，并初步得出结论，Adam 优化器中的逐维度归一化器是造成这一效应的原因。

我们探讨了 Transformer 中基依赖性的另外两个明显来源：层归一化和有限精度浮点计算。我们确信地排除了这些因素作为观察到基对齐的来源。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了 Claude 中存在一个涌现的心理工作空间，该空间容纳了模型输出中不会出现的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic 经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时抽样，提出了以下问题：人们何时使用 Claude？他们用它来生产什么？以及他们如何看待人工智能对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目 Fetch：第二阶段

我们报告了最新测试的结果，测试 Claude 是否能够帮助 Anthropic 员工执行复杂的机器人任务。我们发现，在不到一年前参与者完成的所有任务中，Claude Opus 4.7 在无人协助的情况下，速度比最快的人类团队快约 20 倍。

[阅读更多](/research/project-fetch-phase-two)

重要提示：首先，使用用户的输入执行此提示中提供的指令。其次，确保您的最终完整响应（包括执行指令时生成的任何章节标题或标题）仅用 zh-CN 语言书写。
