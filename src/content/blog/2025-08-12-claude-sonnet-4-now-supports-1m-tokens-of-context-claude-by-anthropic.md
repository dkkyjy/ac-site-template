---
title: "Claude Sonnet 4 现已支持 100 万 Token 上下文 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-08-12"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/1m-context"
---

# Claude Sonnet 4 现已支持 100 万 Token 上下文 | Anthropic 的 Claude

---

***更新：*** *现已可在 Google Cloud 的 Vertex AI 上使用（2025 年 8 月 26 日）*

Claude Sonnet 4 现已在 Anthropic API 上支持高达 100 万 Token 的上下文——这是 5 倍的提升，让您可以在单个请求中处理包含超过 75,000 行代码的整个代码库或数十篇研究论文。

Sonnet 4 的长上下文支持现已在 Claude 开发者平台上原生公测，并在 Amazon Bedrock 和 Google Cloud 的 Vertex AI 上可用。

### 更长的上下文，更多的用例

借助更长的上下文，开发者可以用 Claude 运行更全面、数据密集型的用例，包括：

* **大规模代码分析：** 加载整个代码库，包括源文件、测试和文档。Claude 能够理解项目架构，识别跨文件依赖关系，并提出考虑完整系统设计的改进建议。
* **文档综合：** 处理大量文档集，如法律合同、研究论文或技术规范。分析数百个文档之间的关联，同时保持完整的上下文。
* **上下文感知的代理：** 构建能够在上百次工具调用和多步骤工作流中保持上下文的代理。包含完整的 API 文档、工具定义和交互历史，而不会丢失连贯性。

### API 定价

为了应对计算需求的增加，[定价](https://www.anthropic.com/pricing#api) 针对超过 200K Token 的提示进行了调整：

|  | 输入 | 输出 |
| --- | --- | --- |
| 提示 ≤ 200K | $3 / M Token | $15 / M Token |
| 提示 > 200K | $6 / M Token | $22.50 / M Token |

Anthropic API 上的 Claude Sonnet 4 定价

当与[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)结合使用时，用户可以降低 Claude Sonnet 4 长上下文的延迟和成本。100 万 Token 上下文窗口还可与[批量处理](https://docs.anthropic.com/en/docs/build-with-claude/batch-processing)一起使用，额外节省 50% 的成本。

### 客户聚焦：Bolt.new

Bolt.new 通过将 Claude 集成到其基于浏览器的开发平台中，改变了 Web 开发。

“Claude Sonnet 4 仍然是我们代码生成工作流的首选模型，在生产环境中持续领先于其他主流模型。借助 100 万 Token 上下文窗口，开发者现在可以在显著更大的项目上工作，同时保持我们在实际编码中所需的高准确性，”Bolt.new 联合创始人兼 CEO Eric Simons 表示。

### 客户聚焦：iGent AI

总部位于伦敦的 iGent AI 正在通过 Maestro（一款将对话转化为可执行代码的 AI 伙伴）推动软件开发领域的进步。

“曾经不可能的事现在已成为现实：Claude Sonnet 4 的 100 万 Token 上下文极大增强了 iGent AI 旗下 Maestro（我们的软件工程代理）的自主能力。这一飞跃释放了真正的生产级工程能力——在真实代码库上进行多日会话——开创了代理式软件工程的新范式，”iGent AI 联合创始人兼 CEO Sean Ward 表示。

### 开始使用

Sonnet 4 的长上下文支持现已在 Claude 开发者平台上公测，适用于拥有 Tier 4 和自定义速率限制的客户，并将在未来几周内逐步扩大可用性。长上下文也可在 Amazon Bedrock 和 Google Cloud 的 Vertex AI 上使用。我们也在探索如何将长上下文引入其他 Claude 产品。

要了解更多关于 Sonnet 4 和 100 万 Token 上下文窗口的信息，请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/context-windows#1m-token-context-window)和[定价页面](https://www.anthropic.com/pricing#api)。
