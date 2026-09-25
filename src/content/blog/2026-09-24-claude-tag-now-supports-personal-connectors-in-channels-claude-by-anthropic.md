---
title: "Claude Tag 现在支持在频道中使用个人连接器 | Claude by Anthropic"
description: "[Claude Tag（测试版）](https://claude.com/product/tag) 允许您将 Claude 添加到 Slack 频道，在那里它可以与您的团队协同工作。到目前为止，Claude 只能使用[管理员附加到频道的连接器](https://claude.com/blog/agent-identity-access-model)，而大多数组织故意将这一列表保持较短：他们希望访问"
pubDate: "2026-09-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-tag-now-supports-personal-connectors-in-channels"
---

# Claude Tag 现在支持在频道中使用个人连接器 | Claude by Anthropic

---

[Claude Tag（测试版）](https://claude.com/product/tag) 允许您将 Claude 添加到 Slack 频道，在那里它可以与您的团队协同工作。到目前为止，Claude 只能使用[管理员附加到频道的连接器](https://claude.com/blog/agent-identity-access-model)，而大多数组织故意将这一列表保持较短：他们希望访问权限跟随个人，而不是频道。

现在，Claude 可以在您于频道中提出的请求中使用您自己的[连接器](https://claude.com/docs/claude-tag/concepts/settings-map)。例如，您的日历、您的云盘、您在 CRM 上分配的账户，或者您的预生产部署。如果您[已将其连接到您的 Claude 账户，](http://claude.ai/customize)您可以在频道中访问它。

您可以决定在要求 Claude 访问您的连接器时信息如何呈现。您可以在发布前审查每个回复。或者，您可以使用自动模式自动发布，除非 Claude 判断有需要您审查的敏感内容。在企业版计划中，管理员将能够要求所有人进行审查。

个人连接器为管理员提供了更多的治理选项。他们可以在代理身份下提供对共享工具集的访问权限，让频道成员仅使用个人连接器以依赖现有的基于角色的访问权限，或者逐个工具进行决策。

Claude Tag 中的个人连接器正在团队版计划中推出，企业版随后跟进。

## **使用个人连接器**

人们在频道中需要的大部分信息都在他们自己的登录背后：您的日历上合适的时间、您正在进行的交易、只有您能打开的计划。现在您可以在频道中要求这些了。

例如，这里是 Priya 在 #checkout-migration 频道中，该频道连接到 GitHub。她问道："@Claude 检查我的 Google Drive 文档'结账迁移，Q3'与我们已发布的内容对比。还有什么未完成？"

Priya 要求 Claude 在 #checkout-migration 频道中检查她的计划与已发布内容的对比。频道连接到 GitHub。只有 Priya 能打开该文档。

Claude 通过频道的 GitHub 连接器读取已合并的拉取请求。该文档是只有 Priya 能打开的。以前，Claude 会在此处停止。

现在 Claude 通过 Priya 的 Google Drive 连接器读取文档，与频道的工作分开，并发布已发布的内容和剩余内容。Priya 的计划不敏感，所以她使用自动模式，Claude 在发布前筛选对比结果。对于她想先检查的文档，她可以切换到审查模式，在频道看到之前查看回复。

同一线程。Claude 的带有对比的回复，以及审查提示显示将在发布前发布的内容。

Claude 通过您的连接器所做的一切都会出现在该工具在您账户下的日志中，就像您今天的直接消息工作一样。频道自己的工作保留在其服务账户下，即您的安全团队已经关注的那个账户。

您决定 Claude 能访问什么以及频道能看到什么，就像您在直接消息中使用连接器时一样，并且您可以随时断开连接器的连接。

## **频道自己的连接器仍然重要的地方**

个人连接器不会无人值守运行。计划任务，以及 Claude 自行启动的任何操作，都使用管理员附加到频道的连接器。需要无人值守操作或整个频道依赖的操作的工具应使用共享连接器。

例如，您可能希望将 Claude 添加到您的[#on-call 频道以协助 CI 分类和响应](https://claude.com/blog/ai-ci-cd-on-call)。如果您设置了对运行手册、监控工具和部署历史的共享连接器，Claude 可以识别并帮助修复问题（即使在下班后）。

仅依赖个人连接器的频道适合密切监督的工作。例如，协作起草 RFP 回复可能需要从价格或其他未提供给整个频道的敏感来源拉取数据。Claude 发布的任何内容对频道中的所有人都可见。

## **接下来是什么**

无需安装任何内容。当您的请求需要您的连接器之一时，Claude 第一次会询问您，然后在线程中使用它。

在频道中向 @Claude 请求只有您能访问的内容。了解有关[Claude Tag](https://claude.com/product/tag)的更多信息。
