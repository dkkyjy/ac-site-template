---
title: "让 Claude Cowork 做好企业级准备 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-for-enterprise"
---

# 让 Claude Cowork 做好企业级准备 | Claude by Anthropic

---

Claude Cowork 现已对所有付费计划全面开放。在企业内部，Claude Cowork 已成为团队运作的关键部分：处理任务、起草项目交付成果，以及让团队保持同步。

今天，我们推出组织管控功能，帮助团队在公司范围内部署 Claude Cowork：面向 Enterprise 的基于角色的访问控制、团队支出限额、扩展的 OpenTelemetry 可观测性，以及供管理员查看 Claude Cowork 采用情况的使用分析。

## 早期信号

Claude Code 帮助开发人员从向 Claude 提问转向处理完整任务，而我们在整个组织中也看到了与 Claude Cowork 相同的模式：绝大多数 Claude Cowork 使用量来自工程团队以外的部门。重要的是，运营、市场、财务、法务等职能并不是把他们的核心工作交给 Claude，而是把围绕最关键任务之外的工作——项目更新、协作演示文稿、研究冲刺等——交给它。

早期采用 Claude Cowork 的企业客户在一个团队中看到这种模式出现后，常常想要更广泛地推广，从而引发诸如谁有权访问、支出管理以及如何查看跨团队动态等问题。

## 面向组织级部署的管控措施

在组织范围内部署具备 Claude Cowork 能力的智能体，需要为管理团队提供治理和可见性。今天，我们增加了更多组织所需的管控功能：

**基于角色的访问控制。** Claude Enterprise 上的管理员现在可以将用户分组——手动或通过身份提供者的 SCIM——并为每个组分配自定义角色，定义其成员可以使用哪些 Claude 功能。为特定团队开启 Claude Cowork，并根据采用情况逐步调整。

**团队支出限额。** 通过管理控制台为每个团队设置预算。成本可预测，并根据你对每个团队需求的了解随时调整。

**使用分析。** Claude Cowork 活动现已显示在管理仪表盘和分析 API 中。在仪表盘上，管理员可以追踪不同日期范围内的 Claude Cowork 会话和活跃用户。分析 API 则更为深入：按用户查看 Claude Cowork 活动、技能和连接器调用次数，以及与现有 Chat 和 Claude Code 数据并列的 DAU/WAU/MAU（日活跃用户/周活跃用户/月活跃用户）。了解哪些团队正在采用，哪些工作流正在落地，以及下一步应该投资何处。

**扩展的 OpenTelemetry 支持。** Claude Cowork 现在会发出工具和连接器调用、读取或修改的文件、使用的技能，以及每个 AI 发起的操作是手动批准还是自动批准的事件。这些事件与标准 SIEM 管道（如 Splunk 和 Cribl）兼容，并且使用共享的用户账户标识符，可以将 OTEL 事件与 Compliance API 记录关联起来。OpenTelemetry 在 Team 和 Enterprise 计划中可用。

**Zoom MCP 连接器。** Claude Cowork 与你团队已有的工具集成。今天，Zoom 推出了一个连接器，将会议智能直接带入 Cowork 体验。Zoom 连接器提供 AI Companion 会议摘要和行动项，以及转录和智能录制，帮助团队利用在 Zoom 上的对话在 Cowork 中创建智能体工作流。从 Claude 设置中的连接器目录添加 Zoom。

**每个工具连接器的控制。** 管理员现在可以限制组织范围内每个 MCP 连接器中可用的操作——例如，允许读取访问但禁用写入操作。权限适用于整个组织，并通过管理控制台进行配置。

## 组织如何使用 Claude Cowork

[Zapier](https://claude.com/customers/zapier-cowork-qa) 将 Cowork 连接到他们的组织数据库、Slack 和 Jira，以揭示工程瓶颈——获得了一个仪表盘、按团队分析以及一份优先路线图，随后产品和设计运营团队也为自己复制了这些内容。[Jamf](https://claude.com/customers/jamf) 将七维度的绩效评估变成了 45 分钟的引导式自我评估，然后为供应商评估和事件响应构建了类似的工作流。[Airtree](https://claude.com/customers/airtree)，一家风险投资公司，构建了一个董事会准备工作流，该工作流从投资组合公司的 Drive、Slack 更新和竞争对手新闻中提取信息，并与之前的准备材料交叉对比。
