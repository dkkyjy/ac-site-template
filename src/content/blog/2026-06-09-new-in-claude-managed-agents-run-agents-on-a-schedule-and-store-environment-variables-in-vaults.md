---
title: "Claude 托管代理新增功能：按计划运行代理并将环境变量存储在保管库中"
description: "从今天开始，Claude 托管代理可以按计划运行，并安全访问 CLI 工具和其他经过身份验证的服务。这两项功能现已在 Claude 平台上以公开测试版形式提供。"
pubDate: "2026-06-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/whats-new-in-claude-managed-agents"
---
            从今天开始，Claude 托管代理可以按计划运行，并安全访问 CLI 工具和其他经过身份验证的服务。这两项功能现已在 Claude 平台上以公开测试版形式提供。

## **按计划运行代理**

代理现在可以按计划运行，自动完成例行工作。一个[计划部署](https://platform.claude.com/docs/en/managed-agents/scheduled-deployments)为代理提供一个 cron 计划。每次计划触发时，代理都会启动一个新会话并完成其任务，无需您构建或托管调度器。

将其用于重复性工作，例如夜间数据同步、每周合规性扫描或每日摘要。一旦部署上线，您可以随时暂停、恢复或存档，或按需触发额外运行。

团队已经在使用计划部署来自动化重复性工作：

* [Rakuten](https://claude.com/customers/rakuten-qa) 使用计划部署来分析电子表格数据，并按周或月计划生成报告和演示文稿。团队还监控生产日志和指标，使产品经理无需创建仪表板即可查看应用程序运行状况。
* [Actively AI](https://actively.ai/) 使用托管代理为销售团队提供跨账户代理搜索功能。计划部署定期刷新答案，通过替换团队最初自行构建的调度基础设施来简化其技术栈。[‍](https://ando.so)
* [Ando](https://ando.so) 使用计划部署来保持招聘和销售团队的运转。代理自主监控渠道中提出的后续步骤，在到期时进行跟进，并发送会议提醒。

## **将环境变量存储在保管库中以验证 CLI 和其他工具**

代理通过直接 API 调用、CLI 和 MCP [连接到外部系统](https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp)。现在我们正在扩展[保管库](https://platform.claude.com/docs/en/managed-agents/vaults)以支持环境变量，从而使 CLI 和其他工具能够发出经过身份验证的请求。CLI 允许代理通过 shell 直接驱动现有的命令行工具，使其成为快速、轻量级的集成路径。使用环境变量名称及其可访问的域注册 API 密钥，安装在代理沙箱中的 CLI 即可使用它来发出经过身份验证的 API 调用。

代理永远不会看到您的密钥，因为沙箱仅持有一个占位符。真正的密钥在网络边界附加，并且仅附加到您允许的域上的请求，因此它只会到达您已批准的地方。要更改密钥，请在保管库中更新它，正在运行的会话将在下一次调用时获取新值。大多数在 HTTP 请求中发送密钥的 CLI 都以此方式工作，包括 Browserbase、KERNEL、Notion、Ramp 和 Sentry CLI。[Browserbase](https://docs.browserbase.com/integrations/anthropic/managed-agents/quickstart) 和 [KERNEL](https://www.kernel.sh/docs/integrations/claude-managed-agents) 首次为托管代理提供浏览器功能，使代理能够与其其他工具一起导航和与网页交互。

团队正在使用保管库中的环境变量为代理提供对经过身份验证的工具的安全访问：

* [Notion](https://claude.com/customers/notion-qa) 使用保管库中的环境变量将其 CLI 与 MCP 工具一起推出，为其代理添加文件上传功能，而无需将 API 令牌交给模型。
* [Browserbase](https://www.browserbase.com/) 使用通过保管库进行身份验证的 [browse CLI](https://www.npmjs.com/package/browse) 构建了其公开的浏览器技能目录。计划部署定期验证该目录以保持其准确性。
* [KERNEL](https://www.kernel.sh/docs/integrations/claude-managed-agents) 使用保管库中的环境变量安全地将代理连接到其跟踪使用情况和客户对话的数据库。代理会在使用量激增时立即标记，以便团队与客户确认该活动是否属于预期行为。[‍](https://getmilana.ai/)
* [Milana](https://getmilana.ai/) 使用保管库中的环境变量将其 AI 产品工程师安全地连接到客户的代码库。代理自动查找并修复错误，大规模数据分析速度比以前更快。

## **开始使用**

探索我们的[文档](https://platform.claude.com/docs/en/managed-agents/overview)以了解更多信息，或访问 [Claude 控制台](https://platform.claude.com/)部署您的第一个代理。
