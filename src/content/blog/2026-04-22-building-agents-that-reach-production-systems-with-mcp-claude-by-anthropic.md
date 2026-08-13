---
title: "构建能够触及生产系统的智能体——通过MCP | Anthropic 的 Claude"
description: "---"
pubDate: "2026-04-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp"
---

# 构建能够触及生产系统的智能体——通过MCP | Anthropic 的 Claude

---

智能体的价值取决于它们能够触及的系统。团队通常采用三种方法将其连接到外部系统——直接 API 调用、CLI 和 MCP。本文阐述了每种方法的适用场景、为什么生产型智能体倾向于使用 MCP，以及高效构建这些集成的模式。

## 将智能体连接到外部系统

我们通常看到三种将智能体连接到外部系统的路径：直接 API 调用、CLI 和 MCP。每种方法都有其合理的应用场景，具体取决于你正在构建的内容。关键区别在于智能体与服务之间是否存在通用层，以及该层的覆盖范围。

### 直接 API 调用

智能体直接调用你的 API——要么通过在代码执行沙箱中编写发出 HTTP 请求的代码，要么通过通用的函数调用工具。这是大多数团队起步的方式，对于一个智能体与一个服务通信，或少量不需要跨智能体平台复用的集成来说，效果不错。

但在大规模场景下挑战开始显现。由于智能体与服务之间没有通用层，每个智能体-服务对都变成了一个定制集成，拥有各自的身份验证处理、工具描述和边界情况——这就是 M×N 集成问题。

### 命令行界面（CLI）

智能体在 shell 中运行你的命令行工具。这种方式快速、轻量，并且利用了已有的工具。它在本地环境和沙箱化容器中效果很好——任何有文件系统和 shell 的地方。这提供了一个通用层，但很薄。

CLI 在触及移动端、Web 端或未暴露容器的云托管平台时会遇到硬性限制，身份验证由 CLI 自身的机制处理——通常是一个磁盘上的凭据文件。它最适合本地环境中快速、权限宽松的集成。

### 模型上下文协议（MCP）

MCP 以协议的形式提供了通用层。智能体连接到暴露你系统能力的服务器，其中身份验证、发现和丰富的语义都标准化了。一个远程服务器可以触及任何兼容客户端（Claude、ChatGPT、Cursor、VS Code 等），以及任何部署环境。

它需要稍多一些的前期投入。回报是集成是可移植的，并且提供了特性丰富的智能体集成所需的语义。

## 生产型智能体在云端运行

生产型智能体越来越多地在云端运行，以便能够扩展并持续运作。它们需要触及的系统也是云托管的：你的数据所在之处、工作跟踪之地、基础设施运行之地。这些系统通常位于远程且需要身份验证，而 MCP 提供了通用层。当这些系统位于私有网络内而非公共互联网上时，[Claude 管理智能体中的 MCP 隧道](https://claude.com/blog/claude-managed-agents-updates) 通过仅出站连接将智能体连接到它们——无需暴露端口或公共端点。

我们在采用率中已经看到了这一点。[MCP SDK](https://modelcontextprotocol.io/docs/sdk) 最近月度下载量超过了 3 亿，而年初为 1 亿，在企业和流行的智能体平台中得到了广泛采用。每天有数百万人使用 MCP 与 Claude 交互，该协议支撑了我们最近推出的许多功能，包括 [Claude Cowork](https://claude.com/product/cowork)、[Claude 管理智能体](https://claude.com/blog/claude-managed-agents) 和 [Claude Code 中的频道](https://code.claude.com/docs/en/channels)。

随着 MCP 持续支持生产级智能体系统，我们分享了一些构建良好集成的方法：从构建高级服务器到上下文高效的客户端，以及技能如何补充协议。

## 构建高效的 MCP 服务器

我们在[目录](https://claude.ai/directory/connectors)中有超过 200 个 MCP 服务器，每天被数百万人使用。通过与企业和开发者在协议上紧密合作，我们发现了一些决定智能体能否可靠使用服务器的设计模式。

### 构建远程服务器以实现最大覆盖范围

远程服务器能为你带来分发能力——它是唯一一种能在 Web、移动端和云托管智能体上运行的配置，也是每个主要客户端优化所消费的类型。构建远程服务器，让你的智能体无论在哪里运行都能使用你的系统。

### 围绕意图而非端点来分组工具

数量更少、描述良好的工具始终优于详尽的 API 镜像。不要将你的 API 一对一地包装成 MCP 服务器——围绕意图分组工具，以便智能体可以通过几次调用完成一项任务，而不是拼接许多原始操作。一个 `create_issue_from_thread` 工具要比 `get_thread` + `parse_messages` + `create_issue` + `link_attachment` 更好。请参见[为智能体编写高效工具](https://www.anthropic.com/engineering/writing-tools-for-agents)以了解完整模式。

### 当你的表面区域很大时，设计为代码编排

如果你的服务需要数百种不同的操作（例如 Cloudflare、AWS 或 Kubernetes），那么按意图分组的工具集可能无法覆盖。相反，暴露一个接受代码的薄工具表面：智能体编写一个简短脚本，你的服务器在沙箱中针对你的 API 运行它，只返回结果。[Cloudflare 的 MCP 服务器](https://github.com/cloudflare/mcp) 是参考示例——两个工具（搜索和执行）以大约 1K 个令牌覆盖了约 2,500 个端点。

### 在需要的地方提供丰富的语义

[MCP Apps](https://modelcontextprotocol.io/extensions/apps/overview) 是第一个官方协议扩展，允许工具返回交互式界面，例如图表、表单或仪表板，全部在聊天界面中内联渲染。提供 MCP Apps 的服务器通常比仅返回文本的服务器获得更高的采用率和留存率。在关键时刻将你的产品 UI 呈现在智能体或最终用户面前——该扩展在 Claude.ai、Claude Cowork 和许多其他顶级 AI 工具中受支持。

[Elicitation](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation) 允许你的服务器在工具调用中途暂停以向用户请求输入。[表单模式](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation#form-mode-elicitation-requests) 发送一个简单模式，客户端渲染原生表单——用于请求缺失参数、确认破坏性操作或消除选项歧义。[URL 模式](https://modelcontextprotocol.io/specification/2025-11-25/client/elicitation#url-mode-elicitation-requests) 将用户导向浏览器——用于完成下游 OAuth、收取付款或收集任何不应经过 MCP 客户端的凭据。两者都能让用户保持在工作流中，而不是将他们送到设置页面。表单模式被广泛支持；URL 模式在 Claude Code 中受支持，更多客户端正在开发中。

### 依赖标准化的身份验证

标准化的身份验证使 MCP 对云托管智能体变得实用。如果你的服务器需要 OAuth，最新的 [MCP 规范](https://modelcontextprotocol.io/specification/2025-11-25) 支持 [CIMD](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization#client-id-metadata-documents)（客户端 ID 元数据文档）用于客户端注册——它为用户提供快速的首次身份验证流程，并且大大减少了意外的重新身份验证提示。这是我们推荐的身份验证方法，该能力在 MCP SDK、Claude.ai 和 Claude Code 中得到支持，并且正在行业范围内被广泛采用。

一旦用户授权，下一个问题是云托管智能体如何在运行时持有并重用这些令牌。[Claude 管理智能体](https://platform.claude.com/docs/en/managed-agents/overview)中的 [Vaults](https://platform.claude.com/docs/en/managed-agents/vaults#mcp-oauth-credential) 解决了这个问题：注册用户的 OAuth 令牌一次，在创建会话时通过 ID 引用 vault，平台将正确的凭据注入每个 MCP 连接并代表你刷新它们——无需构建密钥存储，也无需每次调用传递令牌。

## 使 MCP 客户端更高效地使用上下文

MCP 标准化了 AI 智能体（[*客户端*](https://modelcontextprotocol.io/docs/develop/build-client#python)如何连接并使用它们需要的工具和数据源（[*服务器*](https://modelcontextprotocol.io/docs/develop/build-server)）。服务器安全地暴露一系列能力，而客户端则编排这些能力并管理上下文。如果你正在构建 MCP 客户端，请通过逐步展示的模式使其上下文高效。

### 使用工具搜索按需加载工具定义

[工具搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool) 延迟将所有工具加载到上下文中，而不是提前加载。这允许智能体在运行时搜索目录，在需要时拉入相关工具。在我们的[测试](https://www.anthropic.com/engineering/advanced-tool-use)中，工具搜索往往能削减 85% 以上的工具定义令牌，同时保持较高的选择准确性。

使用工具搜索减少上下文使用。来源：[高级工具使用](https://www.anthropic.com/engineering/advanced-tool-use)

### 使用程序化工具调用在代码中处理工具结果

[程序化工具调用](https://www.anthropic.com/engineering/code-execution-with-mcp) 在代码执行沙箱中处理工具结果，而不是将它们原始返回给模型。这允许智能体在代码中循环、过滤和聚合多次调用，只有最终输出才进入上下文。在我们的测试中，这减少了复杂多步骤工作流中大约 [37%](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) 的令牌使用。

这些模式在多个服务器上自然组合：更精简的上下文，更少的往返，更快的响应。请参见[*高级工具使用*](https://www.anthropic.com/engineering/advanced-tool-use)以获取完整分析。

## 将 MCP 服务器与技能配对

[技能和 MCP 是互补的](https://claude.com/blog/skills-explained)。MCP 让智能体能够访问外部系统中的工具和数据，而技能则教给智能体如何使用这些工具完成实际工作的程序性知识。最有能力的智能体两者兼用，而技能使 MCP 服务器能够扩展到超过少量连接。有两种常见模式将它们结合：

### 将技能和 MCP 服务器捆绑为插件

用于 Claude 的[插件](https://code.claude.com/docs/en/plugins-reference#plugin-components-reference)是一种有用的抽象，允许开发者将技能、MCP 服务器、钩子、LSP 服务器和专业子智能体打包到一种易于消费的分发方式中。使用此方法是以最小摩擦统一多个上下文提供者的最佳方式。

将 MCP 服务器与技能结合使 Claude 更像领域专家。通过 MCP 获取工具，并赋予 Claude 端到端编排工作流的技能。请参见我们的 Cowork [数据插件](https://claude.ai/directory/plugins/data%40knowledge-work-plugins) 作为示例，它包含 10 个技能和 8 个 MCP 服务器，用于 Snowflake、Databricks、BigQuery、Hex 等应用。

结合技能与 MCP。来源：[使用技能和 MCP 服务器扩展 Claude 的能力](https://claude.com/blog/extending-claude-capabilities-with-skills-mcp-servers)

### 从 MCP 服务器分发技能

越来越多的提供者在其 MCP 服务器旁边发布技能，这样智能体既能获得原始能力，也能获得如何使用它们的经过验证的指南。[Canva](https://claude.com/connectors/atlassian)、[Notion](https://claude.com/connectors/notion)、[Sentry](https://claude.com/connectors/sentry) 等已在 Claude 中这样做，在我们的[网络目录](https://claude.com/connectors)中将其技能列在其连接器旁边。

为了使这种配对跨所有客户端可移植，MCP 社区正在积极开发一个[扩展](https://github.com/modelcontextprotocol/experimental-ext-skills)，用于直接从服务器交付技能。这样客户端会自动继承相关的专业知识，并与其所依赖的 API 版本保持一致。我们预计随着该扩展的稳定，这种模式将被广泛采用。

## 复合层

我们以三种连接智能体与外部系统的路径开篇。在实践中，成熟的集成将提供所有三种：API 作为基础，CLI 用于本地优先环境，MCP 用于云原生智能体。

随着生产型智能体迁移到云端，MCP 成为关键层，并且是不断复合的那一层。今天，一个远程服务器可以触及任何兼容客户端，跨越任何部署环境，身份验证、交互性和丰富语义由协议处理。随着更多客户端采用该规范并在其中落地更多扩展，同一个服务器在不需你发布任何新内容的情况下变得更加有能力。

在构建集成时，如果你的目标是让生产型智能体在云端触及你的系统，那么使用上述模式构建一个出色的 MCP 服务器。每一个基于 MCP 的集成都会增强生态系统：更少的边界情况需要独自解决，更少的定制集成需要维护。

### 致谢

感谢 Den Delimarsky、David Soria Parra、Henry Shi、Felix Rieseberg、Conor Kelly、Molly Vorwerck、Andy Schumeister、Kevin Garcia、Amie Rotherham、Matt Samuels、Angela Jiang、Katelyn Lesse、AJ Rebeiro 和 Jess Yan 对本博客的贡献。
