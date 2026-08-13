---
title: "构建能够接入生产系统的 MCP 智能体"
description: "智能体的价值取决于它们能够接入的系统。团队通常采用三种方式将智能体连接到外部系统——直接 API 调用、命令行界面和 MCP。本文阐述了每种方式的适用场景、生产级智能体为何倾向于选择 MCP，以及有效构建这些集成的模式。"
pubDate: "2026-04-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp"
---
智能体的价值取决于它们能够接入的系统。团队通常采用三种方式将智能体连接到外部系统——直接 API 调用、命令行界面和 MCP。本文阐述了每种方式的适用场景、生产级智能体为何倾向于选择 MCP，以及有效构建这些集成的模式。

## 将智能体连接到外部系统

我们通常看到三种将智能体连接到外部系统的路径：直接 API 调用、命令行界面和 MCP。根据你的构建目标，每种方式都有其适用场景。关键区别在于智能体与服务之间是否存在通用层，以及该层的覆盖范围。

### 直接 API 调用

智能体直接调用你的 API——要么在代码执行沙箱内编写发起 HTTP 请求的代码，要么通过通用的函数调用工具。这是大多数团队的起点，适用于一个智能体与一个服务通信，或少量无需跨智能体平台复用的集成。

规模化后挑战开始显现。由于智能体与服务之间没有通用层，每个智能体-服务对都成为定制集成，拥有各自的认证处理、工具描述和边界情况——即 M×N 集成问题。

### 命令行界面

智能体在 shell 中运行你的命令行工具。这种方式快速、轻量，且利用现有工具。它非常适合本地环境和沙箱容器——任何存在文件系统和 shell 的地方。这提供了一个通用层，但很薄弱。

CLI 在接入移动端、网页端或云托管平台（这些平台不暴露容器）时遇到硬性限制，认证由 CLI 自身的机制处理——通常是磁盘上的凭据文件。这种方式最适合本地环境中的快速、宽松集成。

### 模型上下文协议

MCP 以协议形式提供通用层。智能体连接到一个暴露系统能力的服务器，认证、发现和丰富的语义都已标准化。一台远程服务器可以接入任何兼容客户端（Claude、ChatGPT、Cursor、VS Code 等），在任何部署环境中。

这需要稍多一些前期投入。回报是集成是可移植的，并提供功能丰富的智能体集成所需的语义。

## 生产级智能体在云端运行

生产级智能体越来越多地在云端运行，以便能够扩展和持续运行。它们需要接入的系统也托管在云端：你的数据所在之处、工作追踪之地、基础设施运行之所。这些系统通常是远程的且需要认证，而 MCP 正好提供了通用层。

我们已经在采用数据中看到了这一点。[MCP SDK](https://modelcontextprotocol.io/docs/sdk) 最近突破了每月 3 亿次下载，高于年初的 1 亿次，在企业级和主流智能体平台中得到广泛采用。每天有数百万人使用 MCP 与 Claude 交互，该协议支撑了我们近期发布的许多产品，包括 [Claude Cowork](https://claude.com/product/cowork)、[Claude Managed Agents](https://claude.com/blog/claude-managed-agents) 和 [Claude Code 中的频道](https://code.claude.com/docs/en/channels)。  

随着 MCP 持续支持生产级智能体系统，我们正在分享构建这些集成的模式：从构建高级服务器到上下文高效的客户端，以及技能如何补充协议。

## 构建有效的 MCP 服务器

我们的[目录](https://claude.ai/directory/connectors)中有超过 200 个 MCP 服务器，每天被数百万人使用。通过与企业和开发者的紧密合作，我们发现了决定智能体能否可靠使用服务器的若干设计模式。

### 构建远程服务器以实现最大覆盖范围

远程服务器是让你获得分发能力的配置——它是唯一能在网页端、移动端和云托管智能体上运行的配置，也是每个主要客户端都优化使用的配置。构建远程服务器，让智能体无论在哪里运行都能使用你的系统。

### 围绕意图而非端点分组工具

数量更少、描述更完善的工具始终优于详尽的 API 镜像。不要将你的 API 一对一地包装成 MCP 服务器——围绕意图分组工具，让智能体通过几次调用就能完成任务，而不是拼接许多原语。一个单一的 `create_issue_from_thread` 工具胜过 `get_thread` + `parse_messages` + `create_issue` + `link_attachment`。请参阅[为智能体编写有效工具](https://www.anthropic.com/engineering/writing-tools-for-agents)了解完整模式。

### 当接口庞大时设计代码编排

如果你的服务需要数百种不同的操作，例如 Cloudflare、AWS 或 Kubernetes，基于意图分组的工具集可能无法覆盖。相反，暴露一个接受代码的薄工具层：智能体编写简短脚本，你的服务器在沙箱中针对你的 API 运行它，仅返回结果。[Cloudflare 的 MCP 服务器](https://github.com/cloudflare/mcp)是参考示例——两个工具（搜索和执行）在大约 1K token 内覆盖约 2,500 个端点。

### 在需要时提供丰富的语义

[MCP Apps](https://modelcontextprotocol.io/extensions/apps/overview) 是第一个官方协议扩展，允许工具返回交互式界面，如图表、表单或仪表板，全部内联渲染在聊天界面中。提供 MCP apps 的服务器通常比仅返回文本的服务器获得显著更高的采用率和留存率。在关键时刻将你的产品 UI 呈现在智能体或最终用户面前——该扩展在 Claude.ai、Claude Cowork 和许多其他顶级 AI 工具中得到支持。

[Elicitation](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation) 允许你的服务器在工具调用中途暂停以向用户请求输入。[表单模式](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation#form-mode-elicitation-requests)发送一个简单模式，客户端渲染原生表单——用于请求缺失参数、确认破坏性操作或消除选项歧义。[URL 模式](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation#url-mode-elicitation-requests)将用户引导至浏览器——用于完成下游 OAuth、接受付款或收集不应通过 MCP 客户端传输的任何凭据。两者都让用户保持在流程中，而不是将他们发送到设置页面。表单模式得到广泛支持；URL 模式在 Claude Code 中得到支持，更多客户端正在开发中。

### 依赖标准化认证

标准化认证使 MCP 对云托管智能体变得实用。如果你的服务器需要 OAuth，最新的 [MCP 规范](https://modelcontextprotocol.io/specification/2025-11-25)支持用于客户端注册的 [CIMD](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization#client-id-metadata-documents)（客户端 ID 元数据文档）——它为用户提供快速的首次认证流程，并大幅减少意外的重新认证提示。这是我们推荐的认证方法，该能力在 MCP SDK、Claude.ai 和 Claude Code 中得到支持，并正在整个行业广泛采用。  

用户授权后，下一个问题是云托管智能体如何在运行时持有和重用这些令牌。[Claude Managed Agents](https://platform.claude.com/docs/en/managed-agents/overview) 中的 [Vaults](https://platform.claude.com/docs/en/managed-agents/vaults#mcp-oauth-credential) 解决了这个问题：注册用户的 OAuth 令牌一次，在会话创建时通过 ID 引用 vault，平台将正确的凭据注入每个 MCP 连接并代表你刷新它们——无需构建密钥存储，无需每次调用传递令牌。

## 使 MCP 客户端更高效利用上下文

MCP 标准化了 AI 智能体（[*客户端*](https://modelcontextprotocol.io/docs/develop/build-client#python)）如何连接和使用它们所需的工具与数据源（[*服务器*](https://modelcontextprotocol.io/docs/develop/build-server)）。服务器安全地暴露一系列能力，而客户端编排这些能力并管理上下文。如果你正在构建 MCP 客户端，请使用渐进式披露模式使其高效利用上下文。

### 使用工具搜索按需加载工具定义

[工具搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)延迟将所有工具加载到上下文中，而不是预先加载。这允许智能体在运行时搜索目录，拉入相关工具。在我们的[测试](https://www.anthropic.com/engineering/advanced-tool-use)中，工具搜索通常将工具定义 token 减少 85% 以上，同时保持高选择准确性。

使用工具搜索减少上下文使用。来源：[高级工具使用](https://www.anthropic.com/engineering/advanced-tool-use)

### 使用编程式工具调用在代码中处理工具结果

[编程式工具调用](https://www.anthropic.com/engineering/code-execution-with-mcp)在代码执行沙箱中处理工具结果，而不是将原始结果返回给模型。这允许智能体在代码中循环、过滤和聚合跨调用结果，只有最终输出进入上下文。在我们的测试中，这在复杂多步骤工作流上减少了大约 [37%](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) 的 token 使用。

这些模式在多个服务器上自然组合：更精简的上下文、更少的往返、更快的响应。请参阅[*高级工具使用*](https://www.anthropic.com/engineering/advanced-tool-use)了解完整分析。

## 将 MCP 服务器与技能配对

[技能和 MCP 是互补的](https://claude.com/blog/skills-explained)。MCP 让智能体能够访问外部系统的工具和数据，而技能则教给智能体如何使用这些工具完成实际工作的程序性知识。最强大的智能体同时使用两者，技能使 MCP 服务器能够扩展到少量连接之外。有两种通用模式可以结合它们：

### 将技能和 MCP 服务器捆绑为插件

Claude 的[插件](https://code.claude.com/docs/en/plugins-reference#plugin-components-reference)是一种有用的抽象，允许开发者将技能、MCP 服务器、钩子、LSP 服务器和专门的子智能体捆绑在一个易于消费的分发方式中。使用这种方法是以最小摩擦统一多个上下文提供者的最佳方式。  

将 MCP 服务器与技能相结合，使 Claude 更像领域专家。通过 MCP 获取你的工具，并赋予 Claude 端到端编排工作流的技能。以我们的 Cowork [数据插件](https://claude.ai/directory/plugins/data%40knowledge-work-plugins)为例，它包含 10 个技能和 8 个 MCP 服务器，用于 Snowflake、Databricks、BigQuery、Hex 等应用。

将技能与 MCP 相结合。来源：[使用技能和 MCP 服务器扩展 Claude 的能力](https://claude.com/blog/extending-claude-capabilities-with-skills-mcp-servers)

### 从 MCP 服务器分发技能

提供者在其 MCP 服务器旁边发布技能变得越来越常见，这样智能体既获得原始能力，又获得使用这些能力的有观点指导手册。[Canva](https://claude.com/connectors/atlassian)、[Notion](https://claude.com/connectors/notion)、[Sentry](https://claude.com/connectors/sentry) 等如今在 Claude 中就这样做，在我们的[网页目录](https://claude.com/connectors)中在其连接器旁边列出技能。

为了使这种配对在每个客户端上可移植，MCP 社区正在积极开发一个[扩展](https://github.com/modelcontextprotocol/experimental-ext-skills)，用于直接从服务器交付技能。这样客户端自动继承相关专业知识，并与所依赖的 API 版本同步。我们预计随着扩展的稳定，这种模式将得到广泛采用。

## 复合层

我们开始时提到了三种将智能体连接到外部系统的路径。在实践中，成熟的集成将提供所有三种：API 作为基础，CLI 用于本地优先环境，MCP 用于基于云的智能体。

随着生产级智能体迁移到云端，MCP 成为关键层，并且是一个不断增值的层。今天，一台远程服务器可以接入任何部署环境中的每个兼容客户端，认证、交互性和丰富语义由协议处理。随着更多客户端采用规范，更多扩展落地，同一台服务器无需你发布任何新内容就能变得更强大。

在构建集成时，如果你的目标是让云中的生产级智能体接入你的系统，请构建一个 MCP 服务器，并使用上述模式使其卓越。每个基于 MCP 构建的集成都会加强生态系统：更少需要独自解决的边界情况，更少需要维护的定制集成。

### 致谢

感谢 Den Delimarsky、David Soria Parra、Henry Shi、Felix Rieseberg、Conor Kelly、Molly Vorwerck、Andy Schumeister、Kevin Garcia、Amie Rotherham、Matt Samuels、Angela Jiang、Katelyn Lesse、AJ Rebeiro 和 Jess Yan 对本博客的贡献。
