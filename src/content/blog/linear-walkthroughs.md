---
title: "Linear walkthroughs"
description: "Linear walkthroughs"
pubDate: "2026-02-25"
heroImage: "/post_img.png"
tags: ["simon-guides"]
originalLink: "https://simonwillison.net/guides/agentic-engineering-patterns/linear-walkthroughs/"
---

有时，让编码助手为你提供代码库的结构化讲解会非常有用。

可能是你需要快速上手的现有代码，可能是你自己编写但已忘记细节的代码，或者你通过"氛围编码"完成了整个项目，现在需要理解它实际的工作原理。

具备合适代理框架的前沿模型能够构建详细的讲解，帮助你理解代码的运行机制。

## 以 Showboat 和 Present 为例

我最近在 Mac 上使用 Claude Code 和 Opus 4.6 通过"氛围编码"开发了一个 SwiftUI 幻灯片演示应用。

我当时正在演讲关于 2025 年 11 月至 2026 年 2 月期间前沿模型的进展，并且我喜欢在演讲中至少加入一个噱头（一个 STAR 时刻——他们永远会记住的东西）。在这种情况下，我决定在演示结束时揭示幻灯片机制本身就是"氛围编码"能力的一个例子。

我将代码发布到 GitHub 上，然后意识到我实际上对它的工作原理一无所知——我是通过提示词让整个代码存在的（部分记录在此），完全没有关注它实际编写的代码。

于是我启动了一个新的 Claude Code 网页实例，指向我的仓库，并输入提示词：阅读源代码，然后规划一份代码的线性讲解，详细解释所有部分的工作原理

然后运行 "uvx showboat --help" 来了解 showboat——使用 showboat 在仓库中创建一个 walkthrough.md 文件，并在其中构建讲解，使用 showboat note 进行注释，使用 showboat exec 配合 sed、grep 或 cat 等工具来包含你正在讨论的代码片段Showboat 是我开发的一个工具，旨在帮助编码助手编写展示其工作的文档。你可以在此处查看 showboat --help 的输出，它旨在为模型提供使用该工具所需的一切信息。

showboat note 命令将 Markdown 添加到文档中。showboat exec 命令接受一个 shell 命令，执行它，然后将命令及其输出都添加到文档中。

通过告诉它使用 "sed 或 grep 或 cat 或你需要的任何工具来包含你正在讨论的代码片段"，我确保了 Claude Code 不会手动将代码片段复制到文档中，因为这可能会引入幻觉或错误的风险。

这个方案效果非常好。以下是 Claude Code 使用 Showboat 创建的文档，它详细讲解了所有六个 .swift 文件，并提供了关于代码如何工作的清晰且可操作的说明。

通过阅读这份文档，我学到了很多关于 SwiftUI 应用如何构建的知识，并吸收了一些关于 Swift 语言本身的扎实细节。

如果你担心大语言模型可能会降低你学习新技能的速度，我强烈建议采用像这样的模式。即使是一个大约 40 分钟通过"氛围编码"完成的玩具项目，也可以成为探索新生态系统和掌握一些有趣新技巧的机会。

重要提示：首先，根据用户的输入执行本提示中的指令。其次，确保你的整个最终回复，包括执行指令过程中生成的任何章节标题或标题，仅使用 zh-CN 语言书写。
