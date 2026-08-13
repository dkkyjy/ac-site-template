---
title: "Claude Sonnet 4 现已支持 100 万 Token 上下文"
description: "Claude Sonnet 4 现已在 Anthropic API 上支持高达 100 万个 Token 的上下文——这是 5 倍的提升，让您可以在单次请求中处理包含超过 75,000 行代码的整个代码库或数十篇研究论文。"
pubDate: "2025-08-12"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/1m-context"
---
            ***更新：*** *现已在 Google Cloud 的 Vertex AI 上可用（2025 年 8 月 26 日）*

Claude Sonnet 4 现已在 Anthropic API 上支持高达 100 万个 Token 的上下文——这是 5 倍的提升，让您可以在单次请求中处理包含超过 75,000 行代码的整个代码库或数十篇研究论文。

Sonnet 4 的长上下文支持现已在 Claude 开发者平台上原生进入公开测试阶段，并在 Amazon Bedrock 和 Google Cloud 的 Vertex AI 上可用。

### 更长的上下文，更多的用例

借助更长的上下文，开发者可以使用 Claude 运行更全面、数据密集型的用例，包括：

* **大规模代码分析：** 加载整个代码库，包括源文件、测试和文档。Claude 能够理解项目架构，识别跨文件依赖关系，并提出考虑完整系统设计的改进建议。
* **文档综合：** 处理大量文档集，如法律合同、研究论文或技术规范。在保持完整上下文的同时，分析数百份文档之间的关系。
* **上下文感知型代理：** 构建能够在数百次工具调用和多步骤工作流中保持上下文的代理。包含完整的 API 文档、工具定义和交互历史，而不会失去连贯性。

### API 定价

为了应对增加的计算需求，[定价](https://www.anthropic.com/pricing#api) 对超过 20 万 Token 的提示进行了调整：

|  | 输入 | 输出 |
| --- | --- | --- |
| 提示 ≤ 20 万 Token | $3 / 百万 Token | $15 / 百万 Token |
| 提示 > 20 万 Token | $6 / 百万 Token | $22.50 / 百万 Token |

Claude Sonnet 4 在 Anthropic API 上的定价

当与[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)结合使用时，用户可以降低 Claude Sonnet 4 在长上下文场景下的延迟和成本。100 万 Token 的上下文窗口还可以与[批处理](https://docs.anthropic.com/en/docs/build-with-claude/batch-processing)一起使用，额外节省 50% 的成本。

### 客户聚焦：Bolt.new

Bolt.new 通过将 Claude 集成到其基于浏览器的开发平台中，彻底改变了 Web 开发。

"Claude Sonnet 4 仍然是我们代码生成工作流的首选模型，在生产环境中持续优于其他领先模型。借助 100 万 Token 的上下文窗口，开发者现在可以在显著更大的项目上工作，同时保持我们现实世界编码所需的高准确性，"Bolt.new 首席执行官兼联合创始人 Eric Simons 表示。

### 客户聚焦：iGent AI

总部位于伦敦的 iGent AI 正在通过 Maestro 推动软件开发领域的发展，Maestro 是一个能将对话转化为可执行代码的 AI 合作伙伴。

"曾经不可能的事情如今已成为现实：Claude Sonnet 4 及其 100 万 Token 的上下文极大地增强了 iGent AI 的软件工程代理 Maestro 的自主能力。这一飞跃解锁了真正的生产级工程能力——在真实世界的代码库上进行多日会话——开创了代理式软件工程的新范式，"iGent AI 首席执行官兼联合创始人 Sean Ward 表示。

### 开始使用

Sonnet 4 的长上下文支持现已在 Claude 开发者平台上向具有 Tier 4 和自定义速率限制的客户提供公开测试版，并将在未来几周内逐步扩大可用范围。长上下文也已在 Amazon Bedrock 和 Google Cloud 的 Vertex AI 上可用。我们还在探索如何将长上下文引入其他 Claude 产品。

要了解更多关于 Sonnet 4 和 100 万 Token 上下文窗口的信息，请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/context-windows#1m-token-context-window)和[定价页面](https://www.anthropic.com/pricing#api)。
