---
title: "Claude 托管代理新增功能：按计划运行代理并将环境变量存储在保险库中 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/whats-new-in-claude-managed-agents"
---

# Claude 托管代理新增功能：按计划运行代理并将环境变量存储在保险库中 | Claude by Anthropic

---

从今天开始，[Claude 托管代理](https://claude.com/blog/claude-managed-agents)可以按计划运行，并安全访问 CLI 工具及其他经过身份验证的服务。这两项功能现已在 Claude 平台上以公开测试版形式提供。

## **计划部署：按计划运行代理**

代理现在可以按计划运行，自动完成例行工作。[计划部署](https://platform.claude.com/docs/en/managed-agents/scheduled-deployments)可让 Claude 托管代理按 cron 计划运行。每次计划触发时，代理都会启动一个新会话并完成其任务，无需您自行构建或托管调度器。

可用于重复性工作，例如夜间数据同步、每周合规性扫描或每日摘要。部署上线后，您可以随时暂停、恢复或归档，也可以按需触发额外运行。

团队已经在使用计划部署来自动化重复性工作：

* [Rakuten](https://claude.com/customers/rakuten-qa) 使用计划部署来分析电子表格数据，并按周或按月生成报告和演示文稿。团队还监控生产日志和指标，使产品经理无需创建仪表板即可了解应用运行状况。
* [Actively AI](https://actively.ai/) 使用托管代理为销售团队提供跨账户的代理搜索能力。计划部署定期刷新答案，通过替换团队最初自行构建的调度基础设施来简化其技术栈。[‍](https://ando.so)
* [Ando](https://ando.so) 使用计划部署来保持招聘和销售团队的顺畅运转。代理会自动监控渠道中的建议后续步骤，在到期时跟进，并发送会议提醒。

## **保险库：存储环境变量以对 CLI 和其他工具进行身份验证**

[保险库](https://platform.claude.com/docs/en/managed-agents/vaults)可安全存储 Claude 托管代理的环境变量和凭据。在运行时，代理会将 API 密钥等机密作为环境变量访问，从而无需将凭据硬编码到提示词或代码中即可对 CLI 工具和其他服务进行身份验证。

代理通过直接 API 调用、CLI 和 MCP [连接到外部系统](https://claude.com/blog/building-agents-that-reach-production-systems-with-mcp)。CLI 让代理可以直接通过 shell 驱动现有的命令行工具，使其成为快速、轻量级的集成路径。使用环境变量名称及其可访问的域注册 API 密钥后，安装在代理沙箱中的 CLI 即可使用该密钥进行经过身份验证的 API 调用。

代理永远不会看到您的密钥，因为沙箱中只保存占位符。真实密钥在网络边界处附加，且仅附加到您允许的域请求上，因此它只会发送到您已批准的位置。如需更改密钥，请在保险库中更新，正在运行的会话将在下次调用时获取新值。大多数通过 HTTP 请求发送密钥的 CLI 都支持此方式，包括 Browserbase、KERNEL、Notion、Ramp 和 Sentry CLI。[Browserbase](https://docs.browserbase.com/integrations/anthropic/managed-agents/quickstart) 和 [KERNEL](https://www.kernel.sh/docs/integrations/claude-managed-agents) 首次为托管代理提供浏览器能力，使代理能够与其他工具一起浏览网页并与之交互。

团队正在使用保险库中的环境变量，让代理安全访问经过身份验证的工具：

* [Notion](https://claude.com/customers/notion-qa) 使用保险库中的环境变量，将其 CLI 与 MCP 工具一起推出，为代理添加文件上传能力，而 API 令牌从未交给模型。
* [Browserbase](https://www.browserbase.com/) 使用通过保险库进行身份验证的 [browse CLI](https://www.npmjs.com/package/browse) 构建了其公开的浏览器技能目录。计划部署会定期验证该目录以保持其准确性。
* [KERNEL](https://www.kernel.sh/docs/integrations/claude-managed-agents) 使用保险库中的环境变量，安全地将代理连接到其跟踪使用情况和客户对话的数据库。代理会在使用量激增发生时立即标记，以便团队与客户确认该活动是否为预期行为。[‍](https://getmilana.ai/)
* [Milana](https://getmilana.ai/) 使用保险库中的环境变量，安全地将其 AI 产品工程师连接到客户的代码库。代理会自动发现并修复错误，大规模数据分析的运行速度也比以前更快。
