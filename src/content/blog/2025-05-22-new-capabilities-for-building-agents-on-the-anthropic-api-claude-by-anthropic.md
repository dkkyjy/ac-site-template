---
title: "在 Anthropic API 上构建代理的新功能 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-05-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/agent-capabilities-api"
---

# 在 Anthropic API 上构建代理的新功能 | Anthropic 的 Claude

---

今天，我们在 Anthropic API 上发布了四项新功能，使开发者能够构建更强大的 AI 代理：代码执行工具、MCP 连接器、Files API，以及最长可缓存提示词一小时的能力。

### 构建更好的 AI 代理

与 [Claude Opus 4 和 Sonnet 4](https://www.anthropic.com/news/claude-4) 一起，这些测试版功能使开发者能够构建可以执行代码进行高级数据分析、通过 MCP 服务器连接到外部系统、跨会话高效存储和访问文件，以及通过经济高效的缓存将上下文保持长达 60 分钟的代理——而无需构建自定义基础设施。

例如，一个项目管理 AI 代理可以使用 MCP 连接器与 Asana 对接，以引用任务并分配工作，通过 Files API 上传相关报告，使用代码执行工具分析进度和风险，并在整个过程中保持完整的上下文——同时通过扩展提示缓存降低费用。

这些功能与现有的 [网络搜索](https://www.anthropic.com/news/web-search-api) 和 [引用](https://www.anthropic.com/news/introducing-citations-api) 等功能一起，构成了构建 AI 代理的全面工具集。请继续阅读，详细了解每一项新功能。

### 代码执行工具

我们正在 Anthropic API 上推出一个[代码执行工具](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)，使 Claude 能够在沙盒环境中运行 Python 代码，产生计算结果和数据可视化。这使 Claude 从代码编写助手转变为数据分析师，能够在 API 调用中直接迭代可视化、清洗数据集并提取洞察。

借助代码执行工具，Claude 可以加载数据集、生成探索性图表、识别模式，并根据执行结果迭代优化输出——所有这些都在一次交互中完成。这意味着 Claude 可以端到端地处理复杂的分析任务，而不仅仅是建议让你单独运行的代码。

主要用例包括：

* **金融建模**：生成财务预测、分析投资组合、计算复杂的财务指标。
* **科学计算**：执行模拟、处理实验数据、分析研究数据集。
* **商业智能**：创建自动报告、分析销售数据、生成绩效仪表板。
* **文档处理**：跨格式提取和转换数据、生成格式化报告、自动化文档工作流。
* **统计分析**：对数据集执行回归分析、假设检验和预测建模。

组织每天可获得 50 小时的代码执行工具免费使用时长，超出部分按每个容器每小时 0.05 美元计费。请查阅[文档](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)了解更多定价信息。

### MCP 连接器

Anthropic API 上的 [MCP 连接器](https://docs.anthropic.com/en/docs/agents-and-tools/mcp-connector) 使开发者无需编写客户端代码即可将 Claude 连接到任何远程模型上下文协议（MCP）服务器。

以前，连接到 MCP 服务器需要构建自己的客户端工具来处理 MCP 连接。现在，Anthropic API 自动处理所有连接管理、工具发现和错误处理。只需将远程 MCP 服务器 URL 添加到 API 请求中，即可立即访问强大的第三方工具，大大降低了构建启用工具的代理的复杂性。

当 Claude 收到配置了 MCP 服务器的请求时，它会自动：

* 连接到指定的 MCP 服务器
* 检索可用的工具
* 推理需要调用哪个工具以及传递哪些参数
* 以代理方式执行工具调用，直到获得满意的结果
* 管理身份验证和错误处理
* 返回包含集成数据的增强响应

不断扩展的远程 MCP 服务器生态系统意味着你可以轻松地为 AI 应用程序添加功能，而无需构建一次性集成。你可以与任何远程 MCP 服务器集成，包括来自 [Zapier](https://zapier.com/mcp) 和 [Asana](https://developers.asana.com/docs/using-asanas-model-control-protocol-mcp-server) 的服务器。在我们的[文档](https://docs.anthropic.com/en/docs/agents-and-tools/remote-mcp-servers)中查看更多远程 MCP 服务器。

### Files API

[Files API](https://docs.anthropic.com/en/docs/build-with-claude/files) 简化了开发者在使用 Claude 时存储和访问文档的方式。无需在每个请求中管理文件上传，你现在可以一次上传文档，并在多次对话中反复引用它。

这简化了开发工作流，特别是对于需要使用大型文档集（如知识库、技术文档或数据集）的应用程序。

Files API 将与代码执行工具集成，使 Claude 能够在代码执行期间直接访问和处理上传的文件，并生成图表等文件作为响应的一部分。这意味着开发者只需通过 Files API 上传一次数据集，然后就可以让 Claude 跨多个会话进行分析，而无需重新上传。

### 扩展提示缓存

开发者现在可以在标准的 5 分钟[提示缓存](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching)生存时间（TTL）与 [扩展的 1 小时 TTL](https://docs.anthropic.com/en/docs/build-with-claude/prompt-caching#1-hour-cache-duration-beta) 之间进行选择——后者需要[额外费用](https://docs.claude.com/en/docs/build-with-claude/prompt-caching#pricing)——这是一个 12 倍的改进，可以降低长时间运行代理工作流的费用。借助扩展缓存，客户可以向 Claude 提供大量的背景知识和示例，同时将长提示的成本降低高达 90%，延迟降低高达 85%。

这使得构建能够长时间保持上下文的代理变得实际可行，无论它们是处理多步骤工作流、分析复杂文档，还是与其他系统协调。以前因成本过高而无法运行的长时间代理应用程序现在可以高效地大规模运行。

### 开始使用

所有这些功能现在都在 Anthropic API 上以公开测试版形式提供。[访问我们的文档](https://docs.anthropic.com/en/docs/overview)了解更多信息，或[观看主题演讲](https://www.youtube.com/live/EvtPBaaykdo)来自我们的开发者大会，了解这些功能的实际应用。
