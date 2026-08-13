---
title: "让 Claude Cowork 做好企业级部署准备"
description: "Claude Cowork 现已面向所有付费计划全面开放。在企业内部，Claude Cowork 已成为团队运作方式的关键组成部分：处理任务、起草项目交付成果，以及让团队保持信息同步。"
pubDate: "2026-04-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-for-enterprise"
---
            Claude Cowork 现已面向所有付费计划全面开放。在企业内部，Claude Cowork 已成为团队运作方式的关键组成部分：处理任务、起草项目交付成果，以及让团队保持信息同步。

今天，我们推出组织管控功能，帮助团队在公司范围内部署 Claude Cowork：面向企业版的基于角色的访问控制、团队支出限额、扩展的 OpenTelemetry 可观测性，以及供管理员查看 Claude Cowork 采用情况的使用分析。

## 早期信号

Claude Code 帮助开发者从向 Claude 提问过渡到处理完整任务，而我们在整个组织中通过 Claude Cowork 看到了相同的模式：Claude Cowork 的绝大部分使用量来自工程团队之外。重要的是，运营、市场、财务、法务等职能部门并没有将核心工作交给 Claude，而是将围绕其最关键任务的工作——项目更新、协作演示文稿、研究冲刺等——交给 Claude。

随着 Claude Cowork 的早期企业采用者在一个团队中看到这种模式的出现，他们往往希望更广泛地推广，从而引出了诸如谁有权访问、支出管理以及如何查看跨团队情况等问题。

## 面向全组织部署的管控功能

在组织范围内部署具备 Claude Cowork 能力的智能代理，需要为管理团队提供治理和可见性。今天，我们增加了组织所需的更多管控功能：

**基于角色的访问控制。** Claude Enterprise 版的管理员现在可以将用户组织成组——手动操作或通过身份提供商的 SCIM 进行——并为每个组分配自定义角色，定义其成员可以使用哪些 Claude 功能。为特定团队开启 Claude Cowork，并根据采用情况的发展进行调整。

**团队支出限额。** 从管理控制台设置每个团队的预算。成本可预测，可根据您了解每个团队的需求进行调整。

**使用分析。** Claude Cowork 活动现在会显示在管理仪表板和分析 API 中。从仪表板，管理员可以跟踪不同日期范围内的 Claude Cowork 会话和活跃用户。分析 API 则更进一步：提供按用户划分的 Claude Cowork 活动、技能和连接器调用，以及 DAU/WAU/MAU，与现有的 Chat 和 Claude Code 数据并列。查看哪些团队正在采用、哪些工作流程正在落地，以及下一步应在何处投入。

**扩展的 OpenTelemetry 支持。** Claude Cowork 现在会为工具和连接器调用、读取或修改的文件、使用的技能，以及每个 AI 发起的操作是手动批准还是自动批准，发出事件。事件与 Splunk 和 Cribl 等标准 SIEM 管道兼容，共享的用户账户标识符让您可以将 OTEL 事件与合规 API 记录关联起来。OpenTelemetry 在 Team 版和 Enterprise 版计划中可用。

**Zoom MCP 连接器。** Claude Cowork 与您的团队已在使用的工具集成。今天，Zoom 推出了一款连接器，将会议智能直接带入 Cowork 体验。Zoom 连接器提供 AI Companion 会议摘要和行动项，以及转录文本和智能录音——帮助团队利用 Zoom 上的对话在 Cowork 中创建代理工作流。从 Claude 设置中的连接器目录添加 Zoom。

**每个工具连接器控制。** 管理员现在可以限制整个组织中每个 MCP 连接器内可用的操作——例如，允许读取访问但禁用写入操作。权限适用于整个组织，并从管理控制台进行配置。

## 组织如何使用 Claude Cowork

[Zapier](https://claude.com/customers/zapier-cowork-qa) 将 Cowork 连接到其组织数据库、Slack 和 Jira，以发现工程瓶颈——从而获得一个仪表板、按团队的分析以及一份优先级排序的路线图，产品和设计运营团队随后将其复制供自己使用。[Jamf](https://claude.com/customers/jamf) 将七方面的绩效评估转变为 45 分钟的引导式自我评估，随后为供应商评估和事件响应构建了类似的工作流程。[Airtree](https://claude.com/customers/airtree) 是一家风险投资公司，构建了一个董事会准备工作流程，该流程从投资组合公司的 Drive、Slack 更新和竞争对手新闻中提取信息，并与之前的准备工作进行交叉引用。

## 开始使用

Claude Cowork 和桌面版 Claude Code 现已面向所有付费计划在 macOS 和 Windows 上全面开放。在 [claude.com/download](http://claude.com/download) 下载 Claude 桌面应用。

对于在组织中部署 Claude 的管理员：从[管理控制台](https://claude.com/settings/admin)配置[基于角色的访问控制](https://support.claude.com/en/articles/13930458-set-up-role-based-permissions-on-enterprise-plans)、团队支出限额和 [OpenTelemetry](https://claude.com/docs/cowork/monitoring)。Claude Cowork 使用数据可在管理仪表板中获取，Analytics API 的文档在[此处](https://support.claude.com/en/articles/13694757-access-engagement-and-adoption-data-with-the-analytics-api)。

如需部署演练，请参加我们与 PayPal 合作的 4 月 16 日[网络研讨会](https://www.anthropic.com/webinars/deploying-cowork-across-the-enterprise-with-paypal)。
