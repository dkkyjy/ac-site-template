---
title: "Salesforce 集成 Claude | Anthropic 的 Claude"
description: "今天，我们以测试版发布 Salesforce 集成 Claude 插件。该插件与 Salesforce 共同构建，在销售人员的现有 Salesforce 权限范围内，将其客户账户、商机和销售管道引入 Claude。插件包含 37 项技能，覆盖客户经理日常工作的方方面面，包括客户调研、通话准备、管道审查和 CRM 更新。"
pubDate: "2026-09-15"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/salesforce-in-claude"
---

# Salesforce 集成 Claude | Anthropic 的 Claude

---

今天，我们以测试版发布 Salesforce 集成 Claude 插件。该插件与 Salesforce 共同构建，在销售人员的现有 Salesforce 权限范围内，将其客户账户、商机和销售管道引入 Claude。插件包含 37 项技能，覆盖客户经理日常工作的方方面面，包括客户调研、通话准备、管道审查和 CRM 更新。

销售人员常常花费大量时间进行会议准备或客户会议后的跟进工作，需要手动整理分散在 Salesforce、邮件、通话录音和 Slack 中的信息。通过新插件，Claude 可以完成这些行政工作，并在销售人员批准后更新 Salesforce。

该插件还包含两个连接器，管理员连接 Salesforce 后，销售人员登录即可开始使用。通过 Salesforce 连接器，Claude 可以读取 Salesforce 数据并执行操作：总结客户历史、更新商机、记录通话或创建跟进任务。Slack 连接器覆盖交易频道摘要和账户团队线程，插件技能可以读写这些内容。销售人员首次使用插件时，设置技能会识别其工具和连接器，并根据其角色和业务范围创建定制的 Claude Artifact。

## 销售人员今天如何使用 Salesforce 集成 Claude

**以简报开始一天。** 每天早晨，Claude 提供个性化简报，包括当天的会议、即将关闭的交易、存在风险的商机以及需要回复的未读线程。设置完成后，简报在后台运行，可以在任何使用 Claude 的地方访问，包括通过 Claude 移动应用随时查看。从简报中，销售人员可以指示 Claude 推迟关闭日期、更改阶段或添加跟进任务，Claude 将在 Salesforce 中执行更新。周五，简报会总结本周情况，并为销售人员的经理起草更新报告。

**准备通话。** 销售人员可以要求 Claude 为下一次会议做准备，Claude 会从 Salesforce、Slack 和邮件中拉取简报：开放的商机及其状态、账户团队本周讨论的内容、未回复的线程，以及之前通话中仍未解决的问题。如果在这些线程中发现尚未录入 Salesforce 的利益相关者，Claude 会将他们添加为客户联系人。

**审查交易并制定关闭计划。** 针对某个商机，Claude 会根据团队方法论对交易进行评分，考虑资格差距、尚未接触的利益相关者以及可能导致关闭日期延误的因素。要求继续时，Claude 会起草商业论证和带日期的共同关闭计划，填写资格字段，并将缺失的利益相关者添加为联系人角色。所有这些信息在获得批准后都会保存到商机记录中。

**每次会议后更新 Salesforce。** 每次通话后，Claude 会将通话记录或销售人员笔记转化为跟进邮件、Slack 交易频道摘要，并起草商机更新内容（如下一步骤、阶段和关闭日期），供销售人员审阅。

**审查管道并分享预测。** 销售人员可以请求查看管道，Claude 会构建交互式仪表板，显示各阶段覆盖情况、最可能延误的交易及原因，以及按客户细分的详细信息。从仪表板中，销售人员可以指示 Claude 移动关闭日期或更改阶段，Claude 会在 Salesforce 中更新记录。仪表板可以与领导层或团队共享，Claude 还可以按照领导层期望的格式起草预测叙述。销售领导层可以在整个团队中运行相同的视图。

**基于组织现有权限构建。** Salesforce 仍然是记录系统。销售人员使用 Salesforce 凭据登录，Claude 仅读取其权限允许的内容。默认情况下，Claude 会在写入之前要求销售人员批准每项拟议更改。在团队版和企业版计划中，我们默认不会使用您的数据训练模型。

管理员只需为组织连接一次 Salesforce，并选择哪些用户组可以使用该插件。

## **销售团队如何使用 Salesforce 集成 Claude**

Anthropic 的客户 GitLab、西门子（Siemens）和 Legora 已将 Salesforce 集成 Claude 部署到其组织中，7,000 名 Salesforce 销售人员在工作中使用它。以下是他们关于使用 Salesforce 集成 Claude 的反馈：

> "使用 Salesforce 集成 Claude，我们的销售人员可以在几秒钟内将实时数据转化为会议简报，而不是花费数小时。随着销售团队的扩大，每位新代表从一开始就能全面了解我们服务的律师事务所。" — David Eckstein，Legora 首席财务官

> "使用 Salesforce 集成 Claude，销售人员可以在一天开始时就已经完成管道审查，客户历史也已就绪。这些时间可以直接用于客户对话。" — Alexa Vignone，Salesforce 总裁兼首席营收官

## **开始使用**

Salesforce 集成 Claude 测试版在所有付费 Claude 计划中均可使用。[Salesforce MCP](https://claude.ai/directory/bundles/salesforce#directory/connectors/salesforce-headless-360) 现在可以直接通过市场安装。要安装插件，管理员可以通过 [AgentExchange](https://agentexchange.salesforce.com/sales-cloud-in-claude-beta-access) 申请访问权限，并为整个组织连接一次 Salesforce。对于管理员，我们提供了[设置指南](https://support.claude.com/en/articles/16952184)，说明如何为您的组织启用此功能。对于销售领导层，我们提供了[使用 Claude 高效运营销售组织的指南](https://claude.com/blog/building-an-ai-native-revenue-organization)。
