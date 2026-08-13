---
title: "Cowork 与金融插件｜Anthropic 的 Claude"
description: "---"
pubDate: "2026-02-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-plugins-finance"
---

# Cowork 与金融插件｜Anthropic 的 Claude

---

今天我们宣布了 Cowork 的更新，包括新的插件，以便企业团队能够根据自己的工作方式定制 Claude。作为其中的一部分，我们也在继续基于[近期的更新](https://claude.com/blog/opus-4-6-finance)，扩展 Claude 能为金融服务所做的工作。

Claude 现在可以在 Excel 和 PowerPoint 之间协同工作——在不同应用间携带上下文，端到端地完成多步骤的金融任务。我们还推出了五个新的金融插件、两个新的 MCP 连接器（用于 FactSet 和 MSCI），以及来自 LSEG 和 S&P Global 的合作伙伴创建的插件。

对于金融专业人士来说，这意味着一个端到端的工作流程——从跨市场数据提供商进行研究，到更新模型和制作演示文稿——现在可以在单个会话中完成，无需切换工具或在过程中丢失上下文。

## 从数据到可交付成果

金融工作分布在许多不同的地方。一份交易备忘录从模型中提取数据，模型从数据中提取信息，而数据又反馈到演示文稿中。Claude 现在可以在 Excel 和 PowerPoint 之间移动而不丢失上下文——检索数据、更新模型、构建可交付成果，作为一项连续的单一任务。

一位股票分析师可以要求 Claude 分析财报、更新财务模型并构建总结幻灯片——所有这些都无需切换工具。当输入发生变化时，Claude 会自动更新工作流程的其余部分。

Claude 在 Excel 和 PowerPoint 之间协同工作的功能，现在已作为研究预览版，面向所有付费计划在 Mac 和 Windows 上提供。

## 新的金融插件

我们推出了五个由 Anthropic 构建的新插件，以及几个由合作伙伴构建的插件。每个插件都带来了与该职能实际运作方式相关的领域专业知识、预期的输出质量以及工作流程模式。

这 5 个由 Anthropic 开发的插件现已全部在我们的[公共仓库](https://github.com/anthropics/financial-services-plugins)中提供。

* 首先，**财务分析**插件支持所有金融分析师所需的基础工作流程，涵盖市场与竞争研究、财务建模，以及 PowerPoint 模板创建和质量检查。

其他插件支持特定关键金融职能的工作流程：

* **投资银行**插件帮助银行家处理交易工作流程：审查交易文件、构建可比公司分析以及准备推介材料。
* **股票研究**插件加速研究工作流程——解析财报电话会议记录、用新指引更新财务模型，以及起草研究报告。
* **私募股权**插件专为交易来源和尽职调查而构建。Claude 可以审查大量文档集、提取标准化的财务数据、模拟各种场景，并根据投资标准对机会进行评分。
* **财富管理**插件帮助顾问分析投资组合、识别偏离和税务暴露，并大规模生成再平衡建议。

除了这五个插件之外，金融专业人士还依赖于专有数据和专业平台，这些是任何通用插件都无法复制的。我们的合作伙伴已经构建了自己的连接器和插件，能够将机构数据直接带入 Claude 的上下文中，从而使用户的工作流程基于其团队已经信任的确切数据源。

## 合作伙伴主导的连接器和插件

除了我们已经支持的现有金融 MCP [连接器](https://claude.com/connectors#connectors)之外，两个新的 MCP 连接器为 Claude 提供了直接访问机构数据平台的途径。

* **FactSet** 连接器将实时市场数据、基本面分析、盈利预测和研究见解带入 Claude，从而增强用户的财务分析和投资研究工作流程，提供可信赖的、机构级别的数据。
* **MSCI** 连接器将 MSCI 可信赖的指数数据带入 Claude，使用户能够访问 MSCI 的专有指数数据，并快速获取关于指数表现、敞口、成分股和方法的见解。

还有几位合作伙伴也在推出自己的插件，这些插件可以在上述同一个[金融服务插件仓库](https://github.com/anthropics/financial-services-plugins)中找到。

* **S&P Global** 插件将 Capital IQ Pro 带入 Claude 的上下文中。其技能包括公司概况、行业交易摘要以及财报电话会议预览。
* **LSEG** 插件使金融专业人士能够从 Claude 内部直接访问 LSEG 的市场数据和分析。使用实时收益率曲线构建 DCF 模型，使用实时新闻起草晨间简报，使用当前跨资产定价再平衡投资组合，并使用实际融资经济学分析交易。适用于拥有活跃 LSEG 数据授权的用户。

## 入门指南

所有由 Anthropic 开发的插件——财务分析、投资银行、股票研究、私募股权和财富管理——今天即可获得，您可以在我们的[公共金融插件仓库](https://github.com/anthropics/financial-services-plugins)中找到它们，合作伙伴开发的 LSEG 和 S&P Global 插件也在同一位置。您可以直接安装它们，或者将其作为起点，根据您公司特定的工作流程、术语和需求进行定制。

要将 Claude 连接到 FactSet、MSCI 或您的其他企业数据源，请访问 Claude 设置中的连接器目录。在那里，管理员可以授权连接并将其打包成插件供团队使用。

要开始使用 Claude 在 Excel 和 PowerPoint 之间协同工作，请下载 Claude in Excel 和 Claude in PowerPoint 的加载项。要更全面地了解 Cowork 以及跨所有职能的插件更新，请参阅[配套博客文章](https://claude.com/blog/cowork-plugins-across-enterprise)。

‍

‍
