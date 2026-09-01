---
title: "节省 Token 的 Anthropic API 更新 | Claude 由 Anthropic 出品"
description: "---"
pubDate: "2025-03-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/token-saving-updates"
---

# 节省 Token 的 Anthropic API 更新 | Claude 由 Anthropic 出品

---

我们对 Anthropic API 进行了多项更新，使开发者能够在使用 Claude 3.7 Sonnet 时显著提高吞吐量并减少 Token 消耗。这些更新包括：缓存感知速率限制、更简洁的提示词缓存，以及 Token 高效的工具调用。

这些更新将帮助你在现有速率限制内处理更多请求，并以极少的代码改动降低成本。

### 利用提示词缓存提高吞吐量

[提示词缓存](https://www.anthropic.com/news/prompt-caching)允许开发者在 API 调用之间存储和复用频繁访问的上下文。这使得 Claude 能够在不随每次请求重复发送相同信息的情况下，保持对大型文档、指令或示例的认知——对于长提示词，成本可降低多达 90%，延迟可降低多达 85%。我们为 Claude 3.7 Sonnet 发布了两项提示词缓存改进，它们协同工作，帮助你更高效地扩展规模。

#### 缓存感知速率限制

在 Anthropic API 上，Claude 3.7 Sonnet 的提示缓存读取 Token 不再计入你的每分钟输入 Token 数（ITPM）限制。这意味着你现在可以优化提示词缓存的使用，以提高吞吐量，从现有的 ITPM 速率限制中获得更多价值。你的每分钟输出 Token 数（OTPM）速率限制保持不变。

这使得 Claude 3.7 Sonnet 在需要大量上下文同时要求高吞吐量的应用中尤为强大，例如：

* 需要在上下文中维护大型知识库的文档分析平台
* 引用大量代码库的编程助手
* 利用详细产品文档的客户支持系统

[缓存感知的 ITPM 限制](https://docs.anthropic.com/en/api/rate-limits#rate-limits)已在 Anthropic API 上的 Claude 3.7 Sonnet 中提供。

#### 更简洁的缓存管理

我们更新了提示词缓存，使其更易使用。现在，当你设置缓存断点时，Claude 会自动从你之前缓存的最长前缀中读取。

你不再需要手动跟踪和指定使用哪些缓存片段，因为我们会自动识别并使用最相关的缓存内容。这不仅减轻了你的工作量，还释放了更多 Token。

此功能已在 Anthropic API 和 Google Cloud 的 Vertex AI 上可用。请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)了解更多。

### Token 高效的工具调用

Claude 已经能够与外部客户端工具和功能进行交互。此次更新允许你为 Claude 配备自定义工具来执行任务——例如从非结构化文本中提取结构化数据，或通过 API 自动化简单任务。Claude 3.7 Sonnet 现在支持[以 Token 高效的方式调用工具](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/token-efficient-tool-use)，输出 Token 消耗可降低多达 70%。平均而言，早期用户已观察到 14% 的降幅。

要使用此功能，只需在 Claude 3.7 Sonnet 的工具调用请求中添加 Beta 请求头 *token-efficient-tools-2025-02-19* 即可。如果你正在使用 SDK，请确保使用的是带有 *anthropic.beta.messages* 的 Beta 版 SDK。

Token 高效的工具调用目前以 Beta 版本在 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 上提供。

#### Text\_editor 工具

我们还推出了一项新的 *text\_editor* 工具，专为需要用户与 Claude 协作处理文档的应用场景而设计。借助该工具，Claude 可以对源代码、文档或研究报告中的特定文本部分进行精准编辑。这在提高准确性的同时，减少了 Token 消耗和延迟。

开发者只需在 API 请求中提供该工具并处理工具调用响应，即可轻松在应用中实现此功能。

*text\_editor* 工具已在 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 上提供。请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/text-editor-tool)开始使用。

### 客户聚焦：Cognition

像 Cognition 这样的早期用户正在利用这些更新来提升 Token 效率和响应质量。Cognition 是一家应用 AI 实验室，也是 Devin 的开发者——Devin 是一位协作式 AI 团队成员，帮助雄心勃勃的工程团队实现更多成果。

"提示词缓存使我们能够向 Claude 提供更多代码库上下文，从而在降低成本和延迟的同时获得更高质量的结果。借助缓存感知的 ITPM 限制，我们进一步优化了提示词缓存的使用，以提高吞吐量，从现有速率限制中获得更多价值，"Cognition 联合创始人兼首席执行官 Scott Wu 表示。

### 立即开始

这些功能现已面向所有 Anthropic API 客户开放。你可以以极少的代码改动立即实施：

1. **利用缓存感知速率限制：** 在 Claude 3.7 Sonnet 上使用[提示词缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)。
2. **实施 Token 高效的工具调用：** 在你的请求中添加 Beta 请求头 *token-efficient-tools-2025-02-19*，开始节省 Token。
3. **尝试 *text\_editor* 工具：** 将其集成到你的应用中，实现更高效的文档编辑工作流。
