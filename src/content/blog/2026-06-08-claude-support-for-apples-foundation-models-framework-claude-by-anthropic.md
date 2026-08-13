---
title: "Claude 对 Apple Foundation Models 框架的支持 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-for-foundation-models"
---

# Claude 对 Apple Foundation Models 框架的支持 | Claude by Anthropic

---

今天，我们发布了针对 Claude 的 Foundation Models 框架支持，通过一个新的 Swift 包来实现，让 Apple 开发者能够使用 Apple 的 Foundation Models 框架调用 Claude，以完成更复杂的工作流程。

Apple 的 Foundation Models 框架让开发者能够从 Swift 原生调用模型。它非常易于使用，可以通过引导生成在短短三行代码内返回类型化的 Swift 值。开发者可以利用它在设备端快速执行本地任务，例如摘要或提取。

现在，开发者可以使用 Apple 的 Foundation Models 框架，在请求需要多步骤推理、代码生成等场景下，将任务移交给 Claude。Claude 还可以搜索网页以获取最新信息，并执行代码进行数据分析。将 Claude 的响应流式传回同一视图。

由于 Apple 的框架会从 `@Generable` 注解返回类型化的 Swift 值，开发者调用 Claude API 时获得的是干净的输入，而非原始的用户文本。

## 这带来的可能性

Foundation Models 框架已经为一系列智能设备端功能提供支持——例如生成个性化提示的日记应用、总结合同文档的应用、以学生水平解释概念的学习应用。添加 Claude 则扩展了这些模式。

一款日记应用可以在设备端生成每日提示，然后要求 Claude 在数月的记录中寻找关联线索。一款学习应用可以在设备端定义术语，当学生追问“这对我们学过的其他内容有什么影响？”时，再移交给 Claude。

对用户来说，这是一体化的体验，每一步都由合适的模型支撑。

## 入门指南

Claude 对 Foundation Models 框架的支持将于明天可用，可在 Apple 的 Foundation Models 框架上运行，支持 iOS 27、iPadOS 27、macOS 27、visionOS 27 和 watchOS 27。将其添加到您的项目中，使用 Anthropic API 密钥登录，然后将 Apple 设备端处理的类型化输出传递给 Claude 请求——该包会自动处理流式传输、工具调用以及向您的 SwiftUI 视图返回结构化响应。

重要提示：首先，请根据用户输入执行本提示中的指示。其次，确保您的整个最终回答（包括在执行指示过程中生成的任何章节标题或标题）仅使用简体中文（zh-CN）书写。
