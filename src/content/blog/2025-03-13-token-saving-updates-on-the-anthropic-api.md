---
title: "Anthropic API 的令牌节省更新"
description: "我们对 Anthropic API 进行了多项更新，使开发者能够显著提高吞吐量并减少与 Claude 3.7 Sonnet 的令牌使用量。这些更新包括：缓存感知速率限制、更简单的提示缓存以及令牌高效的工具使用。"
pubDate: "2025-03-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/token-saving-updates"
---
            我们对 Anthropic API 进行了多项更新，使开发者能够显著提高吞吐量并减少与 Claude 3.7 Sonnet 的令牌使用量。这些更新包括：缓存感知速率限制、更简单的提示缓存以及令牌高效的工具使用。

这些更新将共同帮助您在现有速率限制内处理更多请求，并通过最少的代码更改降低成本。

### 通过提示缓存提高吞吐量

[提示缓存](https://www.anthropic.com/news/prompt-caching)允许开发者存储和重复使用 API 调用之间频繁访问的上下文。这使得 Claude 能够维护大型文档、指令或示例的知识，而无需在每次请求时发送相同的信息——对于长提示，可降低成本高达 90%，延迟降低高达 85%。我们为 Claude 3.7 Sonnet 发布了两个提示缓存的改进，它们协同工作以帮助您更高效地扩展。

#### 缓存感知速率限制

提示缓存读取令牌不再计入您在 Anthropic API 上针对 Claude 3.7 Sonnet 的每分钟输入令牌 (ITPM) 限制。这意味着您现在可以优化提示缓存的使用，以提高吞吐量并从现有的 ITPM 速率限制中获得更多收益。您的每分钟输出令牌 (OTPM) 速率限制保持不变。

这使得 Claude 3.7 Sonnet 对于受益于广泛上下文同时需要高吞吐量的应用特别强大，例如：

* 需要在上下文中维护大型知识库的文档分析平台
* 引用广泛代码库的编码助手
* 利用详细产品文档的客户支持系统

[缓存感知 ITPM 限制](https://docs.anthropic.com/en/api/rate-limits#rate-limits)适用于 Anthropic API 上的 Claude 3.7 Sonnet。

#### 更简单的缓存管理

我们更新了提示缓存，使其更易于使用。现在，当您设置缓存断点时，Claude 会自动从您之前缓存的最长前缀中读取。

您不再需要手动跟踪和指定要使用哪些缓存段，因为我们会自动识别并使用最相关的缓存内容。这不仅减少了您的工作量，还释放了更多令牌。

此功能可在 Anthropic API 和 Google Cloud 的 Vertex AI 上使用。探索我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)以了解更多信息。

### 令牌高效的工具使用

Claude 已经能够与外部客户端工具和函数进行交互。此更新让您能够为 Claude 配备自己的自定义工具来执行任务——例如从非结构化文本中提取结构化数据或通过 API 自动化简单任务。Claude 3.7 Sonnet 现在支持[以令牌高效的方式调用工具](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/token-efficient-tool-use)，将输出令牌消耗减少高达 70%。平均而言，早期用户已看到 14% 的减少。

要使用此功能，只需将 beta 头文件 *token-efficient-tools-2025-02-19* 添加到与 Claude 3.7 Sonnet 的工具使用请求中。如果您正在使用 SDK，请确保您使用的是带有 *anthropic.beta.messages* 的 beta SDK。

令牌高效的工具使用目前可在 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 上以 beta 版本使用。

#### 文本编辑器工具

我们还引入了一个新的 *text_editor* 工具，专为用户与 Claude 在文档上协作的应用而设计。使用这个新工具，Claude 可以对源代码、文档或研究报告中的特定文本部分进行有针对性的编辑。这减少了令牌消耗和延迟，同时提高了准确性。

开发者可以通过在 API 请求中提供该工具并处理工具使用响应，轻松地在他们的应用中实现此工具。

*text_editor* 工具可在 Anthropic API、Amazon Bedrock 和 Google Cloud 的 Vertex AI 上使用。请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/tool-use/text-editor-tool)以开始使用。

### 客户聚焦：Cognition

早期用户，如 Cognition，正在利用这些更新来提高令牌效率和响应质量。Cognition 是一个应用 AI 实验室，也是 Devin 的创造者，Devin 是一个协作式 AI 队友，帮助雄心勃勃的工程团队取得更多成就。

"提示缓存使我们能够提供更多关于代码库的上下文，以获得更高质量的结果，同时降低成本和延迟。借助缓存感知的 ITPM 限制，我们正在进一步优化我们的提示缓存使用，以提高吞吐量并从现有速率限制中获得更多收益，"Cognition 联合创始人兼首席执行官 Scott Wu 表示。

### 立即开始

这些功能今天对所有 Anthropic API 客户开放。您可以立即以最少的代码更改实施它们：

1. **利用缓存感知速率限制：** 将[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)与 Claude 3.7 Sonnet 一起使用。
2. **实施令牌高效的工具使用：** 将 beta 头文件 *token-efficient-tools-2025-02-19* 添加到您的请求中，并开始节省令牌。
3. **尝试 *text_editor* 工具：** 将其集成到您的应用中，以实现更高效的文档编辑工作流程。
