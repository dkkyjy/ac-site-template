---
title: "1M 上下文窗口现已面向 Opus 4.6 和 Sonnet 4.6 全面开放"
description: "Claude Opus 4.6 和 Sonnet 4.6 现已在 Claude 平台上以标准定价提供完整的 1M 上下文窗口。标准定价适用于整个窗口——Opus 4.6 每百万 tokens 收费 $5/$25，Sonnet 4.6 每百万 tokens 收费 $3/$15。没有乘数：一个 900K tokens 的请求与一个 9K tokens 的请求按相同的每 token 费率计费。"
pubDate: "2026-03-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/1m-context-ga"
---
            Claude Opus 4.6 和 Sonnet 4.6 现已在 Claude 平台上以标准定价提供完整的 1M 上下文窗口。标准定价适用于整个窗口——Opus 4.6 每百万 tokens 收费 $5/$25，Sonnet 4.6 每百万 tokens 收费 $3/$15。没有乘数：一个 900K tokens 的请求与一个 9K tokens 的请求按相同的每 token 费率计费。

**全面开放的新特性：**

* **统一价格，完整上下文窗口。** 无长上下文附加费。
* **所有上下文长度均享有完整速率限制。** 您的标准账户吞吐量适用于整个窗口。
* **每个请求可处理 6 倍媒体内容。** 最多可处理 600 张图片或 PDF 页面，较之前的 100 张有所提升。即日起在 Claude 平台原生、Microsoft Foundry 和 Google Cloud 的 Vertex AI 上可用。
* ​​**无需 beta 标头。** 超过 200K tokens 的请求会自动生效。如果您已在发送 beta 标头，它将被忽略，因此无需更改代码。

**1M 上下文窗口现已纳入 Claude Code 中面向 Max、Team 和 Enterprise 用户的 Opus 4.6 版本。** Opus 4.6 会话可自动使用完整的 1M 上下文窗口，这意味着更少的压缩和更多对话内容的完整保留。此前 1M 上下文需要额外使用。

### **可靠的长上下文**

一百万 tokens 的上下文只有在模型能够回忆正确的细节并跨上下文进行推理时才有意义。Opus 4.6 在 MRCR v2 上得分为 78.3%，是该上下文长度下前沿模型中的最高分。

Claude Opus 4.6 和 Sonnet 4.6 在整个 1M 窗口内保持准确性。长上下文检索能力随着每一代模型的更新而提升。

这意味着您可以加载整个代码库、数千页的合同或长时间运行代理的完整追踪记录——工具调用、观察结果、中间推理——并直接使用。此前长上下文工作所需的工程处理、有损摘要和上下文清理已不再必要。整个对话保持完整。

### **开始使用**

1M 上下文即日起在 Claude 平台原生以及通过 Amazon Bedrock、Google Cloud 的 Vertex AI 和 Microsoft Foundry 上可用。使用 Opus 4.6 的 Claude Code Max、Team 和 Enterprise 用户将自动默认使用 1M 上下文。

详情请参阅我们的[文档](https://platform.claude.com/docs/en/build-with-claude/context-windows)和[定价](https://platform.claude.com/docs/en/about-claude/pricing)。

‍
