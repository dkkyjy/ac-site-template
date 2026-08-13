---
title: "面向企业团队的 Cowork 与插件"
description: "今天，我们推出 Cowork 与插件的更新，帮助企业根据自身工作方式定制 Claude。插件将 Claude 转变为面向每个角色和部门的专业智能体。现在，您可以构建私有市场，在整个组织内分发这些插件。"
pubDate: "2026-02-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-plugins-across-enterprise"
---
            今天，我们推出 Cowork 与插件的更新，帮助企业根据自身工作方式定制 Claude。插件将 Claude 转变为面向每个角色和部门的专业智能体。现在，您可以构建私有市场，在整个组织内分发这些插件。

            本次发布还让插件更易于构建和定制，让管理员对市场和连接器拥有更多控制权，并在更广泛的职能范围内新增了插件和连接器。Claude 现在还能跨 Excel 和 PowerPoint 进行编排，实现端到端工作，并在应用之间传递上下文。

###### ‍

###### *Silvern Capital 是一家虚构公司。演示中的大多数团队使用 Cowork 工作，而法务团队使用 Thomson Reuters CoCounsel Legal——这是一个基于 Claude Agent SDK 从头重新构建、专为特定用途打造的法律智能体。*

## 升级插件体验

今天我们推出一系列更新，让您能更轻松地在整个组织中创建、使用和管理插件。

管理员现在可以通过入门模板设置插件，或从头开始构建，Claude 会通过提问引导您完成设置，以便根据您公司的需求定制技能、命令和连接器（MCP）。所有这些都位于一个名为“自定义”的新统一菜单中，该菜单整合了插件、技能和连接器，使管理员能在同一位置查看和管理所有内容。

连接器体验也已全面革新，包括改进的目录、简化的管理控制，以及更轻松地管理哪些连接器被捆绑到插件中。管理员还能对其团队可访问的插件拥有更多控制权，包括组织特定的市场、作为插件来源的私有 GitHub 仓库（私有测试版）、按用户配置以及自动安装。

在用户端，斜杠命令现在以结构化表单形式启动，因此运行“生成报告”或“仪表盘”等工作流感觉就像填写一份简短的表格一样直观。Cowork 现在还包含公司品牌元素，包括为您的组织量身定制的重新设计的首页体验。

此外，我们还增加了 [OpenTelemetry](https://claude.com/docs/cowork/monitoring#monitoring) 支持，让管理员能够跟踪其团队的使用情况、成本和工具活动。

## 新的连接器和插件

当与您已使用的工具连接时，Cowork 最能处理复杂任务。连接器让 Claude 更贴近您的工作所在之处——帮助您跨工具更快地行动，而不是取代您所依赖的专业知识或软件。

来自流行企业软件提供商的新连接器现已可用，包括 Google Workspace（[日历](https://claude.com/connectors/google-calendar)、[云端硬盘](https://claude.com/connectors/google-drive)、[Gmail](https://claude.com/connectors/gmail)）、[Docusign](https://claude.com/connectors/docusign)、[Apollo](https://claude.com/connectors/apollo)、[Clay](https://claude.com/connectors/clay)、[Outreach](https://claude.com/connectors/outreach)、[Similarweb](https://claude.com/connectors/similarweb)、[MSCI](https://claude.com/connectors/msci)、[LegalZoom](https://claude.com/connectors/legalzoom)、[FactSet](https://claude.com/connectors/factset)、[WordPress](https://claude.com/connectors/wordpress-com) 和 [Harvey](https://claude.com/connectors/harvey)。像 [Slack by Salesforce](https://claude.com/plugins/slack)、[LSEG](https://claude.com/plugins/lseg)、[S&P Global](https://claude.com/plugins/sp-global)、[Apollo](https://claude.com/plugins/apollo)、[Common Room](https://claude.com/plugins/common-room) 和 Tribe AI（见下文）等公司也构建了供联合客户探索的插件。

我们还在扩展预构建插件模板库，以便更多知识工作者能在 Cowork 中发现价值。每个模板都与相关领域的从业者共同设计，因此工作流、术语和输出反映了该工作的实际完成方式。新增插件包括：

* [**人力资源**](https://claude.com/plugins/human-resources)：支持员工全生命周期的人员运营，从起草录用信和构建入职计划，到撰写绩效评估和执行薪酬分析。
* [**设计**](https://claude.com/plugins/design)：通过生成评审框架、起草 UX 文案、执行无障碍审计以及构建用户研究计划，加速设计工作流。
* [**工程**](https://claude.com/plugins/engineering)：简化日常工程工作流，例如编写站会摘要、协调事件响应、构建部署检查清单以及起草事后复盘报告。
* [**运营**](https://claude.com/plugins/operations)：管理核心业务运营，包括流程文档、供应商评估、变更请求跟踪以及运行手册创建。
* [**品牌调性**](https://claude.com/plugins/brand-voice) **（由 Tribe AI 提供）**：分析您现有的文档、营销材料和对话，将您的品牌调性提炼为清晰、可执行的指南。
* [**财务分析**](https://claude.com/plugins/financial-analysis)：支持每位财务分析师所需的基础工作流，从市场和竞争研究到财务建模，以及 PowerPoint 模板创建和质量检查。
* [**投资银行**](https://claude.com/plugins/investment-banking)：加速交易工作流，包括审阅交易文件、构建可比公司分析以及准备推介材料。
* [**股权研究**](https://claude.com/plugins/equity-research)：简化研究工作流，例如解析收益电话会议记录、根据新指引更新财务模型以及起草研究报告。
* [**私募股权**](https://claude.com/plugins/private-equity)：通过审阅大量文件集、提取标准化财务数据、建模情景以及根据投资标准对机会进行评分，支持交易寻找和尽职调查。
* [**财富管理**](https://claude.com/plugins/wealth-management)：帮助顾问分析投资组合、识别偏差和税务风险，并大规模生成再平衡建议。

插件是简单、可移植的文件系统，归您所有。它们可在 Cowork 和任何基于 Claude Agent SDK 构建的产品上运行，使得在团队内部以及与行业专家之间创建私有插件市场变得容易。

> “重塑专业工作的三波浪潮：生产力工具、云和搜索，以及现在的智能体 AI。普华永道正在与 Anthropic 合作，将企业级智能体引入 CFO 办公室——通过让每个团队成员拥有工具来做更雄心勃勃的工作、做出更好的决策并以以前不可能的方式发展业务，使财务团队成为更具战略性和价值的职能。” - Sanjay Subramanian，Anthropic 联盟负责人，普华永道

> “两年来，市场一直在向我们炒作 AI 智能体，仿佛它们会像数字员工一样，围绕特定、离散的工作流全天候工作。Anthropic 构建的东西要好得多。” - Mark Hines，首席运营官，Blank Metal

## 跨应用工作

Claude 现在还能跨 Excel 和 PowerPoint 端到端地处理多步骤任务。Claude 可以使用一个 Office 插件中的上下文并将其传递给另一个插件，这使得可以给 Claude 分配更大的项目，例如在 Excel 中运行分析，然后在 PowerPoint 中将其转化为演示文稿。这是一个早期研究预览，预示着 Claude 将像我们一样跨应用工作。

## 开始使用

所有插件的用户体验更新对所有 Cowork 用户开放。团队和企业管理员可以访问公司品牌、配置和 MCP 控制。

Claude 跨 Excel 和 PowerPoint 工作现已在 Mac 和 Windows 上对所有付费计划开放研究预览。下载 [Claude in Excel](https://claude.com/claude-in-excel) 和 [Claude in PowerPoint](https://claude.com/claude-in-powerpoint) 的插件即可开始使用。

要了解更多关于 Cowork 和面向金融服务的插件，请参阅我们的[配套博文](https://claude.com/blog/cowork-plugins-finance)。
