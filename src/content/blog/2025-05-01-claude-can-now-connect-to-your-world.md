---
title: "Claude 现已能够连接您的世界"
description: "今天我们宣布推出集成功能，这是一种将您的应用程序和工具连接到Claude的新方式。我们还通过一种高级模式扩展了Claude的[研究](https://www.anthropic.com/news/research)能力，该模式可搜索网络、您的Google Workspace，以及现在的集成功能。Claude可以在提交一份包含引用的全面报告之前，进行长达45分钟的研究。除了这些更新，我们还将网络搜索"
pubDate: "2025-05-01"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/integrations"
---
            今天我们宣布推出集成功能，这是一种将您的应用程序和工具连接到Claude的新方式。我们还通过一种高级模式扩展了Claude的[研究](https://www.anthropic.com/news/research)能力，该模式可搜索网络、您的Google Workspace，以及现在的集成功能。Claude可以在提交一份包含引用的全面报告之前，进行长达45分钟的研究。除了这些更新，我们还将网络搜索功能面向所有付费计划中的Claude用户全球开放。

### 集成功能

去年11月，我们推出了[模型上下文协议](https://www.anthropic.com/news/model-context-protocol)（MCP）——一种将AI应用程序连接到工具和数据的开放标准。在此之前，MCP的支持仅限于通过本地服务器运行的Claude桌面版。今天，我们推出集成功能，使Claude能够无缝地跨网络和桌面应用程序与远程MCP服务器协同工作。开发人员可以构建和托管能够增强Claude能力的服务器，而用户则可以发现并将任意数量的这些服务器连接到Claude。

当您将工具连接到Claude时，它便能深入了解您的工作——理解项目历史、任务状态和组织知识——并能在各个界面上执行操作。Claude成为一位更具洞察力的协作者，帮助您在一个地方执行复杂项目，并在每个步骤中提供专家协助。

首先，您可以从10种流行服务的集成功能中进行选择，包括[Atlassian的Jira和Confluence](https://www.atlassian.com/platform/remote-mcp-server)、[Zapier](https://zapier.com/mcp)、[Cloudflare](https://github.com/cloudflare/mcp-server-cloudflare/tree/main)、[Intercom](https://www.intercom.com/blog/introducing-model-context-protocol-fin)、[Asana](https://developers.asana.com/docs/using-asanas-model-control-protocol-mcp-server)、[Square](https://developer.squareup.com/docs/mcp)、[Sentry](https://docs.sentry.io/product/sentry-mcp/)、[PayPal](https://www.paypal.ai/)、[Linear](https://linear.app/changelog/2025-05-01-mcp)和[Plaid](https://api.dashboard.plaid.com/mcp/sse)——后续还将有来自Stripe、GitLab和Box等公司的更多集成。开发人员还可以使用我们的文档或诸如[Cloudflare](https://blog.cloudflare.com/remote-model-context-protocol-servers-mcp/)等解决方案，在短短30分钟内创建自己的集成，这些解决方案提供内置的OAuth认证、传输处理以及集成部署。

每个集成都极大地扩展了Claude的能力。例如，Zapier通过预构建的工作流连接数千个应用程序，自动化您整个软件栈中的流程。借助[Zapier集成](https://zapier.com/mcp)，Claude可以通过对话访问这些应用程序和您的自定义工作流——甚至可以自动从[HubSpot](https://developers.hubspot.com/mcp)拉取销售数据，并根据您的日历准备会议简报。

通过访问Atlassian的Jira和Confluence，Claude可以与您协作构建新产品、更有效地管理任务，并通过一次性总结和创建多个Confluence页面及Jira工作项来扩展您的工作。

连接Intercom以更快地响应用户反馈。Intercom的AI代理Fin（现已作为MCP客户端）可以在用户报告问题时执行诸如在Linear中提交错误等操作。与Claude聊天，利用Intercom的对话历史和用户属性来识别模式并进行调试——从用户反馈到错误解决，在同一个对话中管理整个工作流。

### 高级研究

我们正在推出几项新的更新，以增强我们最近发布的[研究](https://www.anthropic.com/news/research)能力。Claude现在可以跨数百个内部和外部来源进行更深入的调查，在5到45分钟内提供更全面的报告。

凭借其进行更复杂研究的新能力（在您开启研究按钮时可用），Claude会将您的请求分解成更小的部分，深入调查每个部分，然后汇编成一份全面的报告。虽然大多数报告在5到15分钟内完成，但对于更复杂的调查，Claude可能需要长达45分钟——而这些工作通常需要数小时的人工研究。

我们还扩展了Claude的数据访问权限。我们最初发布研究功能时支持网络搜索和Google Workspace，但现在通过集成功能，Claude还可以搜索您连接的任何应用程序。

当Claude整合来自各来源的信息时，它会提供清晰的引用，直接链接到原始材料。这种透明度确保您可以放心使用Claude的研究结果，确切知道每个见解的来源。

### 开始使用

集成功能和高级研究现已在Max、Team和Enterprise计划中作为测试版提供，并将很快在Pro计划中提供。网络搜索现已面向所有[Claude.ai](http://claude.ai)付费计划全球开放。有关开始使用集成功能、MCP服务器以及在连接数据源到Claude时的安全和隐私实践的更多信息，请访问我们的[帮助中心](https://support.anthropic.com/en/articles/11175166-about-integrations-using-remote-mcp)。 ***‍***

***更新：*** *适用范围已扩大。（2025年6月3日）*

集成功能和研究现已在Pro、Max、Team和Enterprise计划中提供。网络搜索在所有Claude计划中全球可用。
