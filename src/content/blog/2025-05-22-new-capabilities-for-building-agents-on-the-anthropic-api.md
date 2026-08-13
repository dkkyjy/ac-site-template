---
title: "Anthropic API 新增智能体构建功能"
description: "今天，我们宣布在 Anthropic API 上推出四项新功能，使开发者能够构建更强大的 AI 智能体：代码执行工具、MCP 连接器、Files API，以及将提示缓存延长至一小时的能力。"
pubDate: "2025-05-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/agent-capabilities-api"
---
            今天，我们宣布在 Anthropic API 上推出四项新功能，使开发者能够构建更强大的 AI 智能体：代码执行工具、MCP 连接器、Files API，以及将提示缓存延长至一小时的能力。

### 构建更好的 AI 智能体

与 [Claude Opus 4 和 Sonnet 4](https://www.anthropic.com/news/claude-4) 一起，这些测试版功能使开发者能够构建可执行代码进行高级数据分析、通过 MCP 服务器连接外部系统、跨会话高效存储和访问文件，以及通过经济高效的缓存将上下文维持长达 60 分钟的智能体——而无需构建自定义基础设施。

例如，一个项目管理 AI 智能体可以使用 MCP 连接器与 Asana 关联任务和分配工作，通过 Files API 上传相关报告，使用代码执行工具分析进度和风险，并在整个过程中保持完整上下文——同时通过扩展的提示缓存来降低成本。

这些功能与现有功能（如[网络搜索](https://www.anthropic.com/news/web-search-api)和[引用](https://www.anthropic.com/news/introducing-citations-api)）一起，构成了构建 AI 智能体的综合工具包。请继续阅读，详细了解每项新功能。

### 代码执行工具

我们在 Anthropic API 上推出了[代码执行工具](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)，使 Claude 能够在沙盒环境中运行 Python 代码，以生成计算结果和数据可视化。这将 Claude 从代码编写助手转变为数据分析师，能够在 API 调用中直接迭代可视化、清理数据集并获取洞察。

借助代码执行工具，Claude 可以加载数据集、生成探索性图表、识别模式，并根据执行结果迭代优化输出——所有这些都在一次交互中完成。这意味着 Claude 可以端到端地处理复杂的分析任务，而不仅仅是建议代码让您单独运行。

主要用例包括：

* **财务建模**：生成财务预测、分析投资组合、计算复杂的财务指标。
* **科学计算**：执行模拟、处理实验数据、分析研究数据集。
* **商业智能**：创建自动化报告、分析销售数据、生成绩效仪表板。
* **文档处理**：跨格式提取和转换数据、生成格式化报告、自动化文档工作流。
* **统计分析**：对数据集执行回归分析、假设检验和预测建模。

组织每天可获得 50 小时的代码执行工具免费使用时间，之后按每容器每小时 0.05 美元支付额外使用费用。请查阅[文档](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)了解有关定价的更多信息。

### MCP 连接器

Anthropic API 上的 [MCP 连接器](https://docs.anthropic.com/en/docs/agents-and-tools/mcp-connector)使开发者能够将 Claude 连接到任何远程模型上下文协议（MCP）服务器，而无需编写客户端代码。

以前，连接到 MCP 服务器需要构建自己的客户端框架来处理 MCP 连接。现在，Anthropic API 自动处理所有连接管理、工具发现和错误处理。只需将远程 MCP 服务器 URL 添加到您的 API 请求中，即可立即访问强大的第三方工具，大大降低了构建支持工具的智能体的复杂性。

当 Claude 收到配置了 MCP 服务器的请求时，它会自动：

* 连接到指定的 MCP 服务器
* 检索可用工具
* 推理要调用哪个工具以及传递哪些参数
* 以智能体方式执行工具调用，直到获得足够的结果
* 管理身份验证和错误处理
* 返回带有集成数据的增强响应

不断增长的远程 MCP 服务器生态系统意味着您可以轻松地为 AI 应用程序添加功能，而无需构建一次性集成。您可以集成任何远程 MCP 服务器，包括来自 [Zapier](https://zapier.com/mcp) 和 [Asana](https://developers.asana.com/docs/using-asanas-model-control-protocol-mcp-server) 的服务器。在我们的[文档](https://docs.anthropic.com/en/docs/agents-and-tools/remote-mcp-servers)中查看更多远程 MCP 服务器。

### Files API

[Files API](https://docs.anthropic.com/en/docs/build-with-claude/files) 简化了开发者在构建 Claude 应用时存储和访问文档的方式。您无需在每个请求中管理文件上传，现在可以一次上传文档并在对话中重复引用。

这简化了开发工作流，特别是对于需要处理大型文档集（如知识库、技术文档或数据集）的应用程序。

Files API 将与代码执行工具集成，使 Claude 能够在代码执行期间直接访问和处理上传的文件，并生成图表等文件作为响应的一部分。这意味着开发者可以一次通过 Files API 上传数据集，然后让 Claude 在多个会话中进行分析，而无需重新上传。

### 扩展提示缓存

开发者现在可以在标准的 5 分钟生存时间（TTL）[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)和[延长至 1 小时的 TTL](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching#1-hour-cache-duration-beta)（需[额外付费](https://docs.claude.com/en/docs/build-with-claude/prompt-caching#pricing)）之间进行选择——这是 12 倍的改进，可以降低长时间运行的智能体工作流的费用。借助扩展缓存，客户可以为 Claude 提供广泛的背景知识和示例，同时将长提示的成本降低高达 90%，延迟降低高达 85%。

这使得构建能够在较长时间内保持上下文的智能体变得切实可行，无论它们是处理多步骤工作流、分析复杂文档，还是与其他系统协调。以前因成本过高而无法实现的长时间运行的智能体应用程序，现在可以高效地大规模运行。

### 入门指南

所有这些功能现在都在 Anthropic API 上以公开测试版形式提供。[访问我们的文档](https://docs.anthropic.com/en/docs/overview)了解更多信息，或[观看主题演讲](https://www.youtube.com/live/EvtPBaaykdo)了解这些功能的实际应用。
