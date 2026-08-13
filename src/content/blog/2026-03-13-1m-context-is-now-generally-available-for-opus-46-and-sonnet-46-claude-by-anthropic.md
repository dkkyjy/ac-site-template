---
title: "1M 上下文窗口现已正式对 Opus 4.6 和 Sonnet 4.6 开放 | Claude by Anthropic"
description: "---"
pubDate: "2026-03-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/1m-context-ga"
---

# 1M 上下文窗口现已正式对 Opus 4.6 和 Sonnet 4.6 开放 | Claude by Anthropic

---

Claude Opus 4.6 和 Sonnet 4.6 现在在 Claude 平台上以标准定价提供完整的 1M 上下文窗口。标准定价适用于整个窗口——Opus 4.6 每百万 token 5 美元/25 美元，Sonnet 4.6 每百万 token 3 美元/15 美元。没有额外倍率：一个 900K token 的请求与一个 9K token 的请求按相同的每 token 费率计费。

**正式发布的新特性：**

* **统一价格，完整上下文窗口。** 无长上下文附加费。
* **所有上下文长度均享受完整速率限制。** 您的标准账户吞吐量适用于整个窗口。
* **每次请求的媒体数量提升 6 倍。** 最多可处理 600 张图片或 PDF 页面，此前上限为 100。今日即可在 Claude 平台原生使用，也支持 Microsoft Foundry 和 Google Cloud 的 Vertex AI。
* ​​**无需 beta 标头。** 超过 200K token 的请求自动生效。如果您已在发送 beta 标头，该标头会被忽略，因此无需修改代码。

**1M 上下文窗口现已包含在 Claude Code 的 Max、Team 和 Enterprise 用户中（搭配 Opus 4.6）。** Opus 4.6 会话可以自动使用完整的 1M 上下文窗口，这意味着更少的压缩，更多对话内容得以完整保留。此前 1M 上下文需要额外使用。

### **持久可靠的长上下文**

百万 token 的上下文只有在模型能够回忆正确细节并进行跨上下文推理时才有意义。Opus 4.6 在 MRCR v2 上得分 78.3%，是该上下文长度下前沿模型中的最高分。

Claude Opus 4.6 和 Sonnet 4.6 在整个 1M 窗口上保持准确性。长上下文检索能力随着每一代模型的更新而提升。

这意味着您可以加载整个代码库、数千页的合同、或者长时间运行的智能体的完整追踪记录——工具调用、观察结果、中间推理——并直接使用。此前长上下文工作所需的工程处理、有损摘要和上下文清理已不再必要。整个对话完整保留。
