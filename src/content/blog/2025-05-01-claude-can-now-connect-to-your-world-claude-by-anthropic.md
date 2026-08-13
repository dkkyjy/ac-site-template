---
title: "Claude 现已连接你的世界 | Claude by Anthropic"
description: "---"
pubDate: "2025-05-01"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/integrations"
---

# Claude 现已连接你的世界 | Claude by Anthropic

---

今天，我们正式推出功能集成（Integrations），这是一种将你的应用和工具连接到 Claude 的全新方式。同时，我们也在扩展 Claude 的[研究](https://www.anthropic.com/news/research)能力，新增了高级模式，可搜索网络、你的 Google Workspace，以及现在新增的集成平台。Claude 可以在最长 45 分钟内完成研究，并交付一份包含引用的全面报告。除此之外，我们还面向所有付费计划中的 Claude 用户，在全球范围内开放了网络搜索功能。

### 功能集成

去年 11 月，我们发布了[模型上下文协议](https://www.anthropic.com/news/model-context-protocol)（MCP）——一种将 AI 应用连接到工具和数据的开放标准。在此之前，MCP 的支持仅限于通过本地服务器在 Claude Desktop 中使用。今天，我们引入功能集成，使 Claude 能够无缝地与远程 MCP 服务器协作，覆盖网页端和桌面应用。开发者可以构建和托管服务器来增强 Claude 的能力，而用户则可以发现并将任意数量的此类服务器连接到 Claude。

当你将工具连接到 Claude 时，它便能深入了解你的工作——掌握项目历史、任务状态和组织知识——并在各个交互面上执行操作。Claude 成为更具洞察力的协作者，帮助你在一个平台上执行复杂项目，每一步都获得专家级的协助。

首先，你可以从 10 种流行服务的功能集成中进行选择，包括 [Atlassian 的 Jira 和 Confluence](https://www.atlassian.com/platform/remote-mcp-server)、[Zapier](https://zapier.com/mcp)、[Cloudflare](https://github.com/cloudflare/mcp-server-cloudflare/tree/main)、[Intercom](https://www.intercom.com/blog/introducing-model-context-protocol-fin)、[Asana](https://developers.asana.com/docs/using-asanas-model-control-protocol-mcp-server)、[Square](https://developer.squareup.com/docs/mcp)、[Sentry](https://docs.sentry.io/product/sentry-mcp/)、[PayPal](https://www.paypal.ai/)、[Linear](https://linear.app/changelog/2025-05-01-mcp) 和 [Plaid](https://api.dashboard.plaid.com/mcp/sse)，后续还将有来自 Stripe、GitLab 和 Box 等公司的更多集成。开发者也可以利用我们的文档或类似 [Cloudflare](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/) 的解决方案（提供内置 OAuth 认证、传输处理和集成部署），在短短 30 分钟内创建自己的功能集成。

每项集成都极大地扩展了 Claude 的能力。例如，Zapier 通过预构建的工作流连接数千款应用，实现整个软件栈的流程自动化。借助 [Zapier 集成](https://zapier.com/mcp)，Claude 可以通过对话访问这些应用和你自定义的工作流——甚至能自动从 [HubSpot](https://developers.hubspot.com/mcp) 拉取销售数据，并根据你的日历准备会议简报。

通过访问 Atlassian 的 Jira 和 Confluence，Claude 可以与你协作构建新产品、更高效地管理任务，并通过一次性汇总和创建多个 Confluence 页面及 Jira 工作项来扩展你的工作。

连接 Intercom 以更快响应用户反馈。Intercom 的 AI 代理 Fin 现在作为 MCP 客户端，可以在用户报告问题时执行诸如在 Linear 中提交 Bug 等操作。与 Claude 对话，利用 Intercom 的对话记录和用户属性来识别模式并进行调试——从用户反馈到 Bug 解决，整个工作流尽在一条对话中完成。

### 高级研究

我们推出了多项新更新，以增强此前发布的[研究](https://www.anthropic.com/news/research)能力。Claude 现在可以针对数百个内部和外部来源进行更深入的研究，在 5 到 45 分钟内交付更全面的报告。

借助新的更复杂的研究能力（当你打开研究按钮时可用），Claude 会将你的请求拆分为更小的部分，深入探究每一部分，然后整合成一份全面报告。大多数报告在 5 到 15 分钟内完成，但对于更复杂的研究，Claude 可能需要长达 45 分钟——这类工作通常需要数小时的人工研究。

我们还扩展了 Claude 的数据访问范围。我们最初发布研究功能时支持网络搜索和 Google Workspace，但现在通过功能集成，Claude 还可以搜索你连接的任何应用。

当 Claude 整合来自各来源的信息时，它会提供清晰的引用，直接链接到原始材料。这种透明性确保你可以放心使用 Claude 的研究成果，清楚了解每项见解的来源。

### 开始使用

功能集成和高级研究现已在 Max、Team 和 Enterprise 计划中开放 Beta 测试，并即将在 Pro 计划中推出。网络搜索现已面向所有 [Claude.ai](http://claude.ai) 付费计划全球开放。有关开始使用功能集成、MCP 服务器以及连接数据源到 Claude 时的安全与隐私实践的更多信息，请访问我们的[帮助中心](https://support.anthropic.com/en/articles/11175166-about-integrations-using-remote-mcp)。 ***‍***

***更新：*** *适用范围已扩大。（2025 年 6 月 3 日）*

功能集成和研究现已在 Pro、Max、Team 和 Enterprise 计划中可用。网络搜索在所有 Claude 计划中全球可用。

‍
