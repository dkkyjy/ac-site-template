---
title: "捐赠我们的开源对齐工具"
description: "捐赠我们的开源对齐工具"
pubDate: "2026-05-07"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/donating-open-source-petri"
---

# 捐赠我们的开源对齐工具
**标签：** 对齐

---

2025年10月，我们推出了[Petri](https://www.anthropic.com/research/petri-open-source-auditing)，这是一套开源的对齐测试工具箱，可应用于任何大型语言模型。Petri作为我们Anthropic研究员计划的一部分开发而成，可用于快速简便地测试AI模型是否存在令人担忧的倾向，如欺骗、谄媚以及配合有害请求。这是我们为整个AI开发社区开发开放且有用的对齐工具所做的努力之一。

自Claude Sonnet 4.5以来，Petri已成为我们每个Claude模型对齐评估的一部分。它通过一个独立的"审计员"模型模拟一系列与对齐相关的场景，比较新模型在这些场景中的行为表现。然后，另一个"裁判"模型对生成的对话记录进行评分，以评估是否存在失调行为。

我们很高兴看到Petri被外部组织使用：例如，英国AI安全研究所（AISI）将其作为评估模型是否倾向于破坏AI研究能力的[主要组成部分](https://arxiv.org/abs/2604.00788)。

我们现在正在将Petri更新到第三个版本。以下是一些最大的变化：

* *适应性。* Petri 3.0涉及重大的架构变化，使用户能够将其适应更多用途，特别是通过将审计员模型和目标模型拆分为可以单独调整的独立组件；
* *真实性。* 尽管对齐研究人员试图使测试看起来逼真，但模型通常可以从设置中的各种人为痕迹推断出它实际上正在接受测试。如果模型意识到自己正在被评估，研究人员就无法再了解模型*在一般情况下*的行为方式。Petri的一个附加组件——我们称之为"Dish"——使设置更加逼真，例如通过使用模型的真实系统提示和真实"脚手架"（包裹在模型周围以帮助其实现目标的软件）来运行测试，这些软件将在真实的模型部署中使用；
* *深度。* 我们现在已将Petri与我们的另一个开源对齐工具[Bloom](https://www.anthropic.com/research/bloom)集成，后者可以对特定的选定行为进行更深入的评估（与Petri更广泛的方法相比）。

我们也在为Petri寻找新的归宿。我们已将其开发工作移交给[Meridian Labs](https://meridianlabs.ai/)，一个AI评估非营利组织。这一举措——类似于我们向Linux基金会[捐赠](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation)模型上下文协议（MCP）——将有助于确保Petri保持独立于任何AI实验室，使其结果能够被行业内外的各方视为中立和可信的。

作为Meridian Labs的一部分，Petri加入了[Inspect](https://inspect.aisi.org.uk/)和[Scout](https://meridianlabs-ai.github.io/inspect_scout/)等其他工具，共同构建一个向实验室、独立研究人员和政府开放的技术栈——在AI模型行为的可靠测试比以往任何时候都更加重要的时刻。

您可以在[Meridian Labs博客](https://meridianlabs.ai/blog/posts/introducing-petri-3/)上阅读更多关于Petri 3.0的信息。

安装和使用Petri的说明可在[Petri网站](https://meridianlabs-ai.github.io/inspect_petri/)上找到。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中存在一个涌现的心理工作空间，其中包含不出现在模型输出中的内部思维。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，提出以下问题：人们何时使用Claude？他们用它产生了什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了关于Claude是否能够帮助Anthropic员工执行复杂机器人任务的最新测试结果。我们发现，Claude Opus 4.7在无人协助的情况下，完成所有任务的速度比不到一年前最快的人类团队快了约20倍。

[阅读更多](/research/project-fetch-phase-two)
