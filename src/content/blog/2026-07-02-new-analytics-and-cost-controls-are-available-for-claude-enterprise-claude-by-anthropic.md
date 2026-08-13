---
title: "新分析和成本控制功能现已面向 Claude Enterprise 推出 | Claude by Anthropic"
description: "---"
pubDate: "2026-07-02"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/giving-admins-more-visibility-and-control-over-claude-usage-and-spend"
---

# 新分析和成本控制功能现已面向 Claude Enterprise 推出 | Claude by Anthropic

---

我们正在为 Claude Enterprise 引入更丰富的管理员分析、模型级权限和支出提醒。随着 Claude 在整个组织中承担越来越复杂且更具自主性的工作，其使用和成本模式与标准聊天工具有所不同。这些控制功能让管理员能够了解 Claude 的使用情况，并提供管理成本的工具。

今天的更新建立在 Anthropic 已提供的控制功能之上：各个层级的支出上限、访问与模型路由、带有导出功能的使用分析仪表盘和分析 API，以及努力控制。更丰富的分析和更精细的成本控制是我们数月来持续构建的控制面板的最新添加功能。

## 跟踪采用情况和成本

[面向管理员的分析仪表盘](https://support.claude.com/en/articles/12883420-view-usage-analytics-for-team-and-enterprise-plans)现在按团队和用户显示使用情况和成本，输出内容包括创建的作品、编辑的文件、使用的技能和连接器，并直接显示其成本。管理员可以按 IT 团队已管理的 SCIM 组进行筛选，因此细分结果遵循现有的组织架构。

Claude Code 在管理控制台中新增了两个标签页，专注于价值和用量，提供更深入的洞察。用量标签显示活跃开发者、会话次数以及整个组织中最常用的命令，并每日更新。价值标签汇总了使用和成本数据，帮助管理员一目了然地了解 Claude Code 的价值，包括估算的生产力提升、每次提交的成本以及年度价值。标签页中的每个公式都是可见的，输入参数也可调整。

[分析聊天](https://support.claude.com/en/articles/14729354-use-analytics-chat-to-ask-claude-about-usage)现在可以回答更广泛的问题，并生成更丰富的作品，供您深入探索。管理员可以用自然语言提问——例如“哪些团队本月的 Claude 使用量翻倍了？”或“我们在哪些座位上获得了最高的每席位价值？”——Claude 会返回可导出并与利益相关者分享的图表。

使用和成本数据可通过[分析 API](https://platform.claude.com/docs/en/manage-claude/analytics-api)以编程方式获取，因此财务和 IT 部门可以将 Claude 的使用和成本数据导入他们已有的工具中——例如 Datadog Cloud Cost Management 和 CloudZero——并与其它云和 AI 支出一起查看。结果可按日期范围、团队、产品或模型进行筛选。技能会报告自己的使用和成本，新的端点可跟踪插件采用情况和作品创建。

管理员可以将使用情况可见性扩展到单个用户——包括成本、产品和模型细分，以及支出限额的使用进度——这样就不会有人遭遇意外的使用中断。用户也可以查看自己的使用趋势随时间的变化，包括他们最依赖哪些产品、模型和技能，以及这些活动累计的支出。

## 管理支出的控制功能

[模型默认设置和权限](https://support.claude.com/en/articles/15694740-manage-model-access-for-your-organization)让管理员可以设置在聊天、Cowork 和 Claude Code 中，新对话默认使用哪个 Claude 模型，这样常规工作就不必默认使用最昂贵的选项。管理员可以控制哪些模型可供特定角色或整个组织使用。

支出阈值提醒会在组织级支出限额达到 75% 和 90% 时通知管理员，让他们有时间在用户中途被阻断之前提高上限。用户会在 75% 和 95% 阈值时收到应用内通知，并可以直接在 Claude 内向管理员请求提高限额。

对于需要管理多个组之间限额的组织，[管理员 API](https://platform.claude.com/docs/en/manage-claude/spend-limits-api#example-workflows) 将成本控制工作流程转移到脚本中，使控制功能能够随组织规模扩展。可以自动处理限额增加请求的审核、识别接近支出限额的成员，并以规模化方式标记快速变化的使用情况。

重要：首先，执行此提示中提供的指令，使用用户的输入。其次，确保你最终的完整回复，包括作为执行指令一部分而生成的任何部分标题或标题，都仅以 zh-CN 语言书写。
