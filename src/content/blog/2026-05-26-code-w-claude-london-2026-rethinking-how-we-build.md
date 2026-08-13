---
title: "与Claude一起编程 伦敦2026：重新思考我们构建的方式"
description: "本周在伦敦，我们将[与Claude一起编程](https://claude.com/code-with-claude/london)带到了欧洲。这场活动汇聚了构建者、开发者和创始人，进行了为期两天的主题演讲、分组讨论和研讨会，与构建Claude的团队一起交流。"
pubDate: "2026-05-26"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/code-w-claude-london-2026-rethinking-how-we-build"
---
            本周在伦敦，我们将[与Claude一起编程](https://claude.com/code-with-claude/london)带到了欧洲。这场活动汇聚了构建者、开发者和创始人，进行了为期两天的主题演讲、分组讨论和研讨会，与构建Claude的团队一起交流。

*产品主管Cat Wu（Claude Code）在会议间隙与一位与会者交谈。*

Claude Code负责人Boris Cherny在[主题演讲](https://www.youtube.com/watch?v=6amLO7I9xdg)中描述了他第一次感受到编程"魔力"的时刻。在中学时，他编写了TI-83程序来解决数学作业和考试，并自学了HTML来让他在eBay上出售宝可梦卡牌的列表卖得更好。他通过摆弄来学习，当程序运行时，那种感觉很令人兴奋。

他提到，不知从何时起，编程变得复杂了。编译器、类型检查器、构建系统——每一层都将"我有一个想法"到"它运行起来"之间的距离推得更远。借助智能体，这种距离正在再次缩短：你描述一个问题，程序就会出现。这是一种计算器的感觉，只不过这个计算器可以编写一个分布式系统。

从展示如何使用Claude Code[超越基础](https://claude.com/code-with-claude/session/ldn-beyond-the-basics-with-claude-code)的研讨会，到优化我们各模型中的[思考预算和努力级别](https://claude.com/code-with-claude/session/ldn-the-thinking-lever)，我们展示了Anthropic以及像[Spotify](https://claude.com/code-with-claude/session/ldn-coding-is-no-longer-the-constraint-scaling-devex-to-teams-and-agents-at-spotify)、[Base44](https://claude.com/code-with-claude/session/ldn-from-one-person-to-80-scaling-a-hypergrowth-engineering-org-with-claude-code)和[Legora](https://claude.com/code-with-claude/session/ldn-what-legal-agents-inherit-from-coding-agents-lessons-from-legora)这样的客户如何已经在重新捕捉这种体验。

## 宣布的内容

*工程主管Katelyn Lesse（Claude开发者平台）和产品主管Angela Jiang（Claude开发者平台）在伦敦与Claude一起编程活动期间演示我们新的Claude托管智能体的一些功能。*

在大会上宣布，Claude托管智能体现在可以在你控制的沙盒中运行，并连接到你的私有模型上下文协议（MCP）服务器。现在，智能体执行工具的环境以及它访问的服务都在你企业既定的边界内运行。这两项[新功能](https://claude.com/blog/claude-managed-agents-updates)已在Claude平台上可用：

* **自托管沙盒**（公开测试版）。工具执行转移到你配置的环境中——你自己的基础设施或像Cloudflare、Daytona、Modal或Vercel这样的托管提供商——而负责编排、上下文管理和错误恢复的智能体循环则保留在Anthropic的基础设施上。你的网络策略、审计日志和安全工具适用，文件和存储库不会离开你的边界，并且你可以控制计算大小和用于计算密集型工作的运行时镜像。
* **MCP隧道**（研究预览版）。你的智能体可以访问私有网络内的MCP服务器，而无需将其暴露在公共互联网上。你部署的一个轻量级网关建立单个出站连接：无需入站防火墙规则，无需公共端点，并且流量端到端加密。MCP隧道在托管智能体和Messages API中受支持，并由组织管理员通过Claude控制台进行管理。

包括Amplitude、Clay和Rogo在内的团队已经在使用自托管沙盒构建托管智能体。要开始使用，请探索[文档](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes)，遵循我们的[食谱](https://github.com/anthropics/claude-cookbooks/tree/main/managed_agents/self_hosted_sandboxes)，或[请求访问](https://claude.com/form/claude-managed-agents)MCP隧道。

## 如果你错过了

*研究产品经理Lisa Crofoot在伦敦与Claude一起编程主题演讲期间进行演示。*

如果你错过了直播，请查看我们的主题演讲和分组讨论[录像](https://claude.com/code-with-claude/london)。

与Claude一起编程下一站将前往[东京](https://claude.com/code-with-claude/tokyo)（6月5日至6日）。所有第一天的主题演讲和分组讨论都将进行直播。

*敬请期待受我们演讲启发的技术教程、指南和客户案例。*
