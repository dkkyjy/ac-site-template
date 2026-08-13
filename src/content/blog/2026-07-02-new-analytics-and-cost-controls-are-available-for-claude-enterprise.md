---
title: "Claude Enterprise 现已推出新的分析和成本控制功能"
description: "我们为 Claude Enterprise 推出了更丰富的管理员分析、模型级权限和支出提醒功能。随着 Claude 在整个组织中承担越来越复杂和具有挑战性的代理工作，其使用和成本模式与标准聊天工具有所不同。这些控制功能让管理员能够了解 Claude 的使用情况，并提供管理成本的工具。"
pubDate: "2026-07-02"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/giving-admins-more-visibility-and-control-over-claude-usage-and-spend"
---
            我们为 Claude Enterprise 推出了更丰富的管理员分析、模型级权限和支出提醒功能。随着 Claude 在整个组织中承担越来越复杂和具有挑战性的代理工作，其使用和成本模式与标准聊天工具有所不同。这些控制功能让管理员能够了解 Claude 的使用情况，并提供管理成本的工具。

今天的更新建立在 Anthropic 已提供的控制功能之上：各级别的支出上限、访问和模型路由、带有导出功能的使用分析仪表板和分析 API，以及工作量控制。更丰富的分析和更细粒度的成本控制是我们数月来一直在构建的控制面板的最新补充。

## 跟踪采用情况和成本

[管理员分析仪表板](https://support.claude.com/en/articles/12883420-view-usage-analytics-for-team-and-enterprise-plans)现在按组和用户显示使用情况和成本，输出内容包括创建的工件、编辑的文件、使用的技能和连接器，并直接显示其成本。管理员可以按 IT 团队已管理的 SCIM 组进行筛选，因此细分结果会遵循其现有的组织结构图。

Claude Code 获得了更丰富的洞察，管理控制台中新增了两个专注于价值和使用的标签页。使用情况标签页显示活跃开发者、会话次数和整个组织中的常用命令，并每日更新。价值标签页汇总使用情况和成本数据，帮助管理员一目了然地了解 Claude Code 的价值，估算生产力提升、每次提交的成本和年度价值。每个公式在标签页中都是可见的，并且输入参数可调整。

[分析聊天功能](https://support.claude.com/en/articles/14729354-use-analytics-chat-to-ask-claude-about-usage)现在可以回答更广泛的问题，并生成更丰富的工件，供您深入探索。管理员可以用自然语言提问——"哪些团队本月的 Claude 使用量翻倍了？"或"我们在哪些地方获得了最高的每席位价值？"——Claude 会返回图表，这些图表可以导出并与利益相关者分享。

使用情况和成本数据可通过[分析 API](https://platform.claude.com/docs/en/manage-claude/analytics-api) 以编程方式获取，因此财务和 IT 部门可以将 Claude 的使用情况和成本数据导入他们已在使用的工具中——如 Datadog Cloud Cost Management 和 CloudZero——并与其他云和 AI 支出一起查看。结果可按日期范围、团队、产品或模型进行筛选。技能会报告自己的使用情况和成本，新的端点可跟踪插件采用情况和工件创建情况。

管理员可以将使用情况可见性扩展到个人用户——成本、产品和模型细分，以及支出限额的进度——这样就不会有人遇到意外的中断。用户还可以查看自己的使用趋势随时间的变化，包括他们最常依赖哪些产品、模型和技能，以及这些活动在支出中的累计情况。

## 管理支出的控制功能

[模型默认设置和权限](https://support.claude.com/en/articles/15694740-manage-model-access-for-your-organization)让管理员可以设置新对话在聊天、Cowork 和 Claude Code 中默认使用哪个 Claude 模型，这样常规工作就不必默认使用最昂贵的选项。管理员可以控制哪些模型可供特定角色或整个组织使用。

支出阈值提醒会在组织级支出限额达到 75% 和 90% 时通知管理员，让他们有时间在任何人中途被阻止之前提高上限。用户会在 75% 和 95% 阈值时收到应用内通知，并可以直接从管理员处请求提高限额，无需离开 Claude。

对于需要跨多个组管理限额的组织，[管理员 API](https://platform.claude.com/docs/en/manage-claude/spend-limits-api#example-workflows) 将成本控制工作流程移至脚本中，使控制功能能够随组织扩展。自动处理增加请求的审核、识别接近支出限额的成员，并大规模标记快速变化的使用情况。

## 开始使用

对于管理整个组织内 Claude 的管理员：在管理控制台中探索使用情况和成本细分，按组设置模型默认值和支出限额，并配置支出阈值提醒以提前应对超额使用。使用数据可在管理仪表板中获取，分析 API 让财务和 IT 部门能够将相同的指标拉取到现有报告系统中，了解更多信息请点击[此处](https://support.claude.com/en/articles/13694757-get-started-with-the-claude-enterprise-analytics-api)。
