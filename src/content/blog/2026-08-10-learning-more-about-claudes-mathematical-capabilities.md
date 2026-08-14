---
title: "深入了解 Claude 的数学能力"
description: "---"
pubDate: "2026-08-10"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/riemann-zeta"
---

# 深入了解 Claude 的数学能力

---

科学

# 深入了解 Claude 的数学能力

2026 年 8 月 10 日


最近，Anthropic 的一名员工给 Claude 出了一个强人所难的挑战。这关乎数学中最著名的未解问题之一：*认真尝试攻克黎曼猜想*。

Claude 确实认真尝试了，但如果你了解这项任务的难度，你大概不会感到意外（黎曼猜想可追溯至 1859 年，并且有[百万美元悬赏](https://www.claymath.org/millennium-problems/)），它没有成功。尽管如此，在尝试过程中，它出人意料地在某个相关问题上取得了进展。

一个未发布的 Claude 研究版本改进了黎曼ζ函数中满足黎曼猜想的零点比例的长期下界。借助数学家们过去几十年的广泛前期研究，它将该下界从 41.6% 提高到了 67.2%。

Anthropic 的两位数学家研究并验证了 Claude 的[论文](https://www-cdn.anthropic.com/564f962e60643842f5fcb4a17c9dbc8f608f1c37.pdf)，并撰写了一份面向专家的[非正式说明](https://www-cdn.anthropic.com/23455459f8832d06bb175cc0f88d019aed962ef8.pdf)，简洁地陈述了 Claude 的证明。Claude 还为其结果生成了一个[可形式化验证的证明](https://github.com/anthropics/zeta-23-lean)。我们感谢该领域的两位专家 Brian Conrey 和 Dan Goldston，他们在时间紧迫的情况下慷慨地审阅了这篇论文。

我们并不期望 Claude 使用的技术会导向黎曼猜想的证明。但它的工作是 AI 模型数学能力进步速度的最新例证。在本文中，我们讨论 Claude 是如何处理这个问题的，以及它发现了什么。

## 黎曼ζ函数

黎曼ζ函数描述素数的分布：函数取值为零的每个位置都会为素数序列贡献越来越精细的信息。黎曼猜想认为，决定素数的那些零点都位于某条垂直直线上。这已成为数学中最具影响力的猜想之一：许多结果都假设它为真，以便为素数提供某种形式的随机性。

至今没有人能够证明或否定黎曼猜想，但数学家们在研究与黎曼ζ函数及其零点相关的许多方向上取得了进展。其中一个方向如上所述，是量化位于线上的零点的最小比例：随着时间推移，他们已逐渐将这个已知的常数比例提高到 41.6%。

另一个方向关注的是线上零点的*分布*。具体来说，1973 年，Montgomery 在该领域[引入](https://en.wikipedia.org/wiki/Montgomery's_pair_correlation_conjecture)了许多新技术，尽管这些技术假设猜想成立。最近，几位数学家（Baluyot、Goldston、Suriajaya 和 Turnage-Butterbaugh）发表了一个[系列](https://arxiv.org/abs/2306.04799)的[工作](https://arxiv.org/abs/2501.14545)，使 Montgomery 的技术在*不依赖*该假设的情况下也能成立，这意味着它们可以支持提高线上零点下界常数的工作。Claude 的结果在很大程度上借鉴了这一研究方向，以及 Bombieri 在 2000 年发表的[论文](https://eudml.org/doc/252338)。

## Claude 的发现

Claude 发现，将 Baluyot、Goldston、Suriajaya 和 Turnage-Butterbaugh 的结果与 Bombieri 的工作相结合，可以超越此前最优的 41.6% 下界比例，并将其提高到 67.2%。

Claude 发现的简短技术说明如下：Claude 构造了一个合适的函数空间，其二次型由 Weil 诱导，并由线上（相应地位于线外）的零点产生正定（相应地为负定）子空间。随后，Claude 直接根据一阶和二阶矩信息写下了一个关于二次型秩的不等式。（后者能通过素数上的对偶图景，或通过控制希尔伯特变换而成功计算出来，这在解析数论中并不令人意外。）敢于将整个空间一并处理，同时考虑正定性和负定性，并允许二次型是非对角的，在某种意义上正是这一步让 Claude 得以在重要的前期工作基础上得出结论。

完整的技术说明见[论文](https://www-cdn.anthropic.com/564f962e60643842f5fcb4a17c9dbc8f608f1c37.pdf)。Claude 关于其如何得出该结果的解释见单独的[附录](https://www-cdn.anthropic.com/d7f3ecf1d01392d887f8bc974ca187e2a121b1ed.pdf)。

## Claude 的方法

一个未发布的 Claude 研究版本在 Claude Code 中通过两次会话发现了新的下界，总共使用了 3100 万个输出 token。

Anthropic 员工（并非数学家）Jarred Sumner 提示 Claude 对猜想本身“认真尝试一番”，之后的数学选择完全交给模型。最初，Claude 生成并尝试了 650 个想法，但都没有成功。Jarred 让 Claude 再试一次，它花了一天半时间协调约 60 个 Claude 子代理，这次深入得多：它们总共运行了 2,400 条 shell 命令，并编写了数百个 Python 脚本。1 这些子代理针对已知的 ζ 零点进行了数千次数值检验，并相互评审彼此的工作。在整个过程中，Jarred 的输入主要限于向 Claude 发送鼓励消息（大多是“继续”或“相信自己”的变体）。2 这似乎帮助 Claude 克服了最初的一些怀疑，即它是否能取得有意义的进展。

在尝试这项任务的过程中发现这一新结果后，Claude 对自己的工作进行了检验：它让多个子代理审阅证明、寻找反例、从 arXiv 下载 54 篇论文以确认其发现尚未被提出，并从头独立地重新证明该发现。Claude 主动提出把其发现写成论文，并建议由一位人类数论专家验证其发现。

Anthropic 自己的两位数学家 Levent Alpöge 和 Ralph Furman 研究了 Claude 的工作，以理解这些新结果以及它们与上述前期工作的关系。与此同时，Claude 与另一位员工 Eric Easley 合作，为该结果生成了一个 [Lean 形式化版本](https://github.com/anthropics/zeta-23-lean)，该形式化版本通过了标准验证工具 [comparator](https://github.com/leanprover/comparator)。

## AI 模型在数学上的进展

这一结果表明，像 Claude 这样的 AI 模型能够以新的、有时出人意料的方式扩展数学家思想的影响力和覆盖面。尽管它未能解决黎曼猜想本身，但这一结果却作为最初请求的意外副产品而出现。

连 Claude 也对自己的发现感到惊讶——它起初持怀疑态度，这也许是因为它从训练中了解到数学开放问题的难度以及 AI 模型的局限性。但在一些鼓励性提示之后，它得出了我们描述的结果。也许 Claude 和我们许多人一样，低估了 AI 进步的速度。

## 延伸阅读

以下文档列表提供了有关 Claude 结果的更多信息：

* [Claude](https://www-cdn.anthropic.com/564f962e60643842f5fcb4a17c9dbc8f608f1c37.pdf)[的](https://www-cdn.anthropic.com/d7f3ecf1d01392d887f8bc974ca187e2a121b1ed.pdf)[论文](https://www-cdn.anthropic.com/564f962e60643842f5fcb4a17c9dbc8f608f1c37.pdf)；
* [Claude](https://github.com/anthropics/zeta-23-lean)[的](https://www-cdn.anthropic.com/d7f3ecf1d01392d887f8bc974ca187e2a121b1ed.pdf)[形式化](https://github.com/anthropics/zeta-23-lean)；
* [Anthropic](https://www-cdn.anthropic.com/23455459f8832d06bb175cc0f88d019aed962ef8.pdf)[的](https://www-cdn.anthropic.com/d7f3ecf1d01392d887f8bc974ca187e2a121b1ed.pdf)[更简洁地陈述该证明的非正式说明](https://www-cdn.anthropic.com/23455459f8832d06bb175cc0f88d019aed962ef8.pdf)；
* [Claude 对其如何得出结果的解释](https://www-cdn.anthropic.com/d7f3ecf1d01392d887f8bc974ca187e2a121b1ed.pdf)；
* [Claude 过程的详细记录](https://www-cdn.anthropic.com/8a0d1add3c637b858a9a181e98c40e9548c3f44f.pdf)。

#### 脚注

1. 在 60 个子代理中，2 个负责提出关键数学思想，13 个向这些代理贡献了想法，30 个尝试提出新想法（但未能成功），13 个担任验证者以检查论证的正确性，最后 2 个帮助撰写了最初的论文。
2. 一个包含类似鼓励的提示曾被用来帮助 Claude 证伪[雅可比猜想](https://x.com/__alpoge__/status/2079028340955197566?s=20)。

## 相关内容

### 用 Claude 发现密码学弱点

密码算法。第一次攻击显著削弱了 HAWK——一种为未来量子计算机能够破解现有标准的世界而设计的数字签名方案。第二次攻击找到了一种攻击降轮 AES 的新方法，AES 是使用最广泛的对称密码。

[阅读更多](/research/discovering-cryptographic-weaknesses)

### Project Pilot：AI 能控制无人机吗？

我们与 Andon Labs 合作开发了一系列新的评估，用以评估 AI 模型使用飞行无人机的能力，最终形成了一个新基准：Drone-Bench。

[阅读更多](/research/project-pilot)

### 加拿大如何使用 Claude：Anthropic 经济指数的发现

[阅读更多](/research/how-canada-uses-claude)

## 订阅 Anthropic Science

内容包括 AI 辅助发现、实用工作流程，以及横跨各科学领域的实地记录。
