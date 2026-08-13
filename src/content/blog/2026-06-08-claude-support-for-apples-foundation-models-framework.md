---
title: "Claude 对 Apple 基础模型框架的支持"
description: "今天我们发布了一个新的 Swift 包，为 Claude 提供基础模型框架支持，让 Apple 开发者能够使用 Apple 的基础模型框架调用 Claude，实现更复杂的工作流程。"
pubDate: "2026-06-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-for-foundation-models"
---
            今天我们发布了一个新的 Swift 包，为 Claude 提供基础模型框架支持，让 Apple 开发者能够使用 Apple 的基础模型框架调用 Claude，实现更复杂的工作流程。

Apple 的基础模型框架让开发者能够从 Swift 中原生访问模型。它非常易于使用，只需三行代码即可通过引导生成返回类型化的 Swift 值。开发者可以利用此功能访问 Apple 的设备端模型，用于摘要或提取等快速本地任务。

现在，开发者可以使用 Apple 的基础模型框架，在需要多步骤推理、代码生成等请求时转交给 Claude。Claude 还可以搜索网络获取最新信息，并执行代码进行数据分析。将 Claude 的响应流式传输回同一视图。

由于 Apple 的框架通过 @Generable 注解返回类型化的 Swift 值，开发者调用 Claude API 时获得的是干净输入，而非原始用户文本。

## 这解锁了什么

基础模型框架已经为一系列智能设备端功能提供支持——生成个性化提示的日记应用、总结合同的文档应用、以学生水平解释概念的学习应用。添加 Claude 扩展了这些模式的每一种可能性。

日记应用可以在设备端生成每日提示，然后让 Claude 查找数月条目中的关联线索。学习应用可以在设备端定义术语，当学生追问"为什么这对我们学过的其他内容都重要？"时，再转交给 Claude。

对用户而言，这是一个统一的体验，每一步都由合适的模型提供支持。

## 开始使用

对基础模型框架的 Claude 支持将于明天上线，通过 Apple 的基础模型框架在 iOS 27、iPadOS 27、macOS 27、visionOS 27 和 watchOS 27 上运行。将其添加到您的项目，使用 Anthropic API 密钥登录，并将 Apple 设备端处理的类型化输出传递给 Claude 请求——该包会处理流式传输、工具调用以及返回 SwiftUI 视图的结构化响应。

重要提示：首先，使用用户的输入执行本提示中提供的说明。其次，确保您的整个最终响应（包括执行说明时生成的任何部分标题或标题）仅使用 zh-CN 语言编写。
