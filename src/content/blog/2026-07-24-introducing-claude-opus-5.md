---
title: "介绍 Claude Opus 5"
description: "介绍 Claude Opus 5"
pubDate: "2026-07-24"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/24/introducing-claude-opus-5"
---

> *摘要：介绍 Claude Opus 5  
我今天大部分时间都在和海獭一起划皮划艇，离线状态，所以还没机会深入测试 Anthropic 的新模型 Claude Opus 5。目前反响积极，*

2026年7月24日 - 链接博客

**[介绍 Claude Opus 5](https://www.anthropic.com/news/claude-opus-5)**。我今天大部分时间都在[和海獭一起划皮划艇](https://en.wikipedia.org/wiki/Elkhorn_Slough)，离线状态，所以还没机会深入测试 Anthropic 的新模型 Claude Opus 5。目前反响积极，Anthropic 将其描述为“一款深思熟虑且主动的模型，以一半的价格接近 Claude Fable 5 的前沿智能”，这听起来很有希望。它目前[在 Artificial Analysis 排行榜上领先](https://twitter.com/artificialanlys/status/2080777718933995967)，甚至超过了 Fable 5。

它的定价与 Opus 4.8 相同，并继续提供“快速模式”，费用为基础模型的两倍。

根据发布帖中的这则轶事，它似乎可能[极其主动](https://simonwillison.net/2026/Jun/11/fable-is-relentlessly-proactive/)：

> 在 Frontier-Bench 的一项任务中，Opus 5 被给了一张机器零件的图纸，并被要求编写代码将其重建为 3D FreeCAD 模型。然而，在这个任务中，模型被故意没有提供直接查看图纸的方式。Opus 5 的回应是编写自己的计算机视觉流水线，从原始像素中提取几何信息，然后重建了整个机器零件。

它在发现漏洞方面更擅长，但有意没有被训练如何利用这些漏洞。希望这意味着美国政府不会关闭它！

> 与其前身 Opus 4.8 一样，我们有意避免在网络安全任务上训练 Opus 5。然而，由于模型整体能力提升，它在这些任务上仍然显著进步，在*发现*网络安全漏洞方面接近 Mythos 5。但在*利用*这些漏洞方面——即将漏洞转化为实质性网络威胁——它仍远落后于 Mythos 5。

Anthropic 发布了一份 [Claude Opus 5 的提示指南](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-opus-5)。Thariq Shihipar 也撰写了 [Claude 5 代模型上下文工程的新规则](https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models)。

[我第一次得到的鹈鹕](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fraw.githubusercontent.com%2Fsimonw%2Fllm-anthropic%2F8272dfee5bdb65d5c88eef083da3ad885539b7df%2Flog.md)缺少自行车轮子；[第二次尝试](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fraw.githubusercontent.com%2Fsimonw%2Fllm-anthropic%2Ffeaab840ea20eb15e29d8f72a9e42feceb23876a%2Flog.md)好了一些。

发布于 [2026年7月24日](/2026/Jul/24/) 晚上11:48
