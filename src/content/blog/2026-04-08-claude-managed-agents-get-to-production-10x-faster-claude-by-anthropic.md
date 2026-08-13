---
title: "Claude Managed Agents：将产品上线速度提升10倍 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents"
---

# Claude Managed Agents：将产品上线速度提升10倍 | Claude by Anthropic

---

## Claude Managed Agents 详解

Claude Managed Agents 是一套可组合的 API，用于大规模构建和部署云端托管的代理。它将 Anthropic 管理的 harness 与生产级基础设施相结合，提供状态、记忆、权限和计划执行能力。

在今日发布之前，构建代理意味着要将开发周期投入到安全基础设施、状态管理、权限控制以及每次模型升级时重写代理循环中。Managed Agents 将经过性能调优的代理 harness 与生产级基础设施相结合，使您能够在数天而非数月内从原型走向发布。

无论您是在构建单任务执行器还是复杂的多代理流水线，您都可以专注于用户体验，而非运营开销。

Managed Agents 现已在 Claude 平台上以公开测试版形式提供。

## **将代理的构建和部署速度提升10倍**

交付一个生产级代理需要沙盒化代码执行、检查点、凭据管理、作用域权限以及端到端追踪。在您交付任何用户可见的功能之前，这已经是数月的基础设施工作。

Managed Agents 负责处理这些复杂性。您只需定义代理的任务、工具和护栏，我们将在我们的基础设施上运行它。内置的编排 harness 决定何时调用工具、如何管理上下文以及如何从错误中恢复。

Managed Agents 包含：

*   **生产级代理**：安全沙盒、身份验证和工具执行均由我们为您处理。
*   **长时间运行的会话**：可自主运行数小时，即使断开连接，进度和输出也会持久保留。
*   **多代理协调**：代理可以启动并指挥其他代理以并行化复杂工作（处于*研究预览阶段*，[在此处](http://claude.com/form/claude-managed-agents)申请访问）。**‍**
*   **可信治理**：为代理提供对真实系统的访问，内置作用域权限、身份管理和执行追踪。

Claude Managed Agents 架构

## **专为充分发挥 Claude 的能力而设计**

Claude 模型是为代理工作而构建的。Managed Agents 是专为 Claude 量身定制的，使您能够以更少的努力获得更好的代理结果。

使用 Managed Agents，您可以定义结果和成功标准，Claude 会自我评估并迭代直至达成目标（处于*研究预览阶段*，[在此处](http://claude.com/form/claude-managed-agents)申请访问）。当您需要更严格的控制时，它也支持传统的提示-响应工作流程。

在围绕结构化文件生成的内部测试中，Managed Agents 将结果任务成功率比标准提示循环提升了多达10个百分点，在最具挑战性的问题上取得了最大收益。

会话追踪、集成分析和故障排除指导直接内置在 Claude Console 中，因此您可以检查每一次工具调用、决策和失败模式。

## **团队正在构建的内容**

团队已经在各种生产用例中使用 Managed Agents 实现了10倍的发布速度。编码代理可以读取代码库、规划修复方案并提交 PR。生产力代理可以加入项目、领取任务并与团队其他成员一起交付工作。财务和法律代理可以处理文档并提取关键信息。在每种情况下，数天内发布意味着更快地为用户提供价值。

*   [**Notion**](https://claude.com/customers/notion-qa) 让团队可以直接在其工作区内将工作委托给 Claude（现已在 Notion Custom Agents 内部以私有 alpha 形式提供）。工程师用它来交付代码，而知识工作者则用它来制作网站和演示文稿。数十个任务可以并行运行，而整个团队则协同编辑输出结果。
*   [**Rakuten**](https://claude.com/customers/rakuten-qa) 在企业产品、销售、市场营销和财务部门交付了企业代理，这些代理接入 Slack 和 Teams，让员工可以分配任务并取回可交付的成果，如电子表格、幻灯片和应用程序。每个专业代理均在一周内部署完成。
*   [**Asana**](https://claude.com/customers/asana-qa) 构建了 AI Teammates——一种协作式 AI 代理，与人类一起在 Asana 项目中工作，承接任务并起草可交付成果。该团队使用 Managed Agents 以远超其他方式的速度增加了高级功能。
*   [**Vibecode**](https://claude.com/customers/vibecode) 帮助其客户从提示到部署应用程序，使用 Managed Agents 作为默认集成，为新一代 AI 原生应用提供支持。用户现在至少可以比以前快10倍的速度启动相同的基础设施。[**‍**](https://claude.com/customers/sentry)
*   [**Sentry**](https://claude.com/customers/sentry) 将其调试代理 Seer 与一个由 Claude 驱动的代理配对，后者负责编写补丁并提交 PR，使开发者能够在一个流程中从标记错误到获得可审查的修复。该集成在 Managed Agents 上于数周内完成交付，而非数月。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终响应（包括执行指令时生成的任何章节标题或标题）仅使用 zh-CN 语言书写。
