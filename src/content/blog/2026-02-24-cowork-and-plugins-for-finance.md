---
title: "Cowork 与金融插件"
description: "今天我们宣布对 Cowork 进行更新，包括新增插件，使整个企业的团队能够按照自己期望的工作方式塑造 Claude。作为其中的一部分，我们继续在[近期更新](https://claude.com/blog/opus-4-6-finance)的基础上扩展 Claude 在金融服务领域的能力。"
pubDate: "2026-02-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-plugins-finance"
---
今天我们宣布对 Cowork 进行更新，包括新增插件，使整个企业的团队能够按照自己期望的工作方式塑造 Claude。作为其中的一部分，我们继续在[近期更新](https://claude.com/blog/opus-4-6-finance)的基础上扩展 Claude 在金融服务领域的能力。

Claude 现在可以在 Excel 和 PowerPoint 中协同工作——在应用程序之间携带上下文，以端到端的方式完成多步骤金融任务。我们还推出了五个新的金融插件、两个用于 FactSet 和 MSCI 的新 MCP 连接器，以及来自 LSEG 和 S&P Global 的合作伙伴创建的插件。

对于金融专业人士而言，这意味着一个端到端的工作流程——从跨市场数据提供商进行研究，到更新模型并构建演示文稿——现在可以在单个会话中完成，无需切换工具或在过程中丢失上下文。

## 从数据到交付成果

金融相关工作分布在许多不同的地方。交易备忘录从模型中提取数据，模型从数据中提取信息，而数据又反馈到演示文稿中。Claude 现在可以在 Excel 和 PowerPoint 之间移动而不丢失上下文——检索数据、更新模型并构建交付成果，作为一项连续的单一任务。

股票分析师可以要求 Claude 分析收益、更新财务模型并构建摘要幻灯片——所有这些都无需切换工具。当输入发生变化时，Claude 会自动更新工作流程的其余部分。

Claude 在 Excel 和 PowerPoint 之间协同工作的功能现已在 Mac 和 Windows 平台上以研究预览版形式提供给所有付费计划。

## 新的金融插件

我们推出了五个由 Anthropic 构建的新插件，以及由合作伙伴构建的多个插件。每个插件都带来了与该职能实际运作方式相关的领域专业知识、预期输出质量和工作流程模式。

这 5 个由 Anthropic 开发的插件现已全部在我们的[公共代码库](https://github.com/anthropics/financial-services-plugins)中提供。

* 首先，**金融分析**插件支持所有金融分析师所需的基础工作流程，涵盖市场和竞争研究、财务建模以及 PowerPoint 模板创建和质量检查。

其他插件支持特定金融职能的工作流程：

* **投资银行**插件帮助银行家处理交易工作流程：审阅交易文件、构建可比公司分析以及准备推介材料。
* **股票研究**插件加速研究工作流程——解析财报电话会议记录、用最新指引更新财务模型以及起草研究报告。
* **私募股权**插件专为交易寻找和尽职调查而构建。Claude 可以审阅大量文件集、提取标准化的财务数据、建模情景并根据投资标准对机会进行评分。
* **财富管理**插件帮助顾问分析投资组合、识别漂移和税务风险，并大规模生成再平衡建议。

除了这五个插件之外，金融专业人士还依赖专有数据和专业平台，这些是通用插件无法复制的。我们的合作伙伴构建了自己的连接器和插件，将机构数据直接带入 Claude 的上下文中，因此用户的工作流程基于其团队已经信任的确切数据源。

## 合作伙伴主导的连接器和插件

除了我们已经支持的现有金融 MCP [连接器](https://claude.com/connectors#connectors)之外，两个新的 MCP 连接器使 Claude 能够直接访问机构数据平台。

* **FactSet** 连接器将实时市场数据、基本面分析、盈利预估和研究见解带入 Claude，以增强用户的金融分析和投资研究工作流程，提供可信赖的机构级数据。
* **MSCI** 连接器将 MSCI 值得信赖的指数数据带入 Claude，使用户能够访问 MSCI 的专有指数数据，并快速了解指数表现、风险敞口、成分股和方法论。

多个合作伙伴也在推出自己的插件，这些插件可以在上述相同的[金融服务插件代码库](https://github.com/anthropics/financial-services-plugins)中找到。

* **S&P Global** 插件将 Capital IQ Pro 带入 Claude 的上下文。功能包括公司简介、行业交易摘要和财报电话会议预览。
* **LSEG** 插件使金融专业人士能够从 Claude 内部直接访问 LSEG 的市场数据和分析。使用实时收益率曲线构建 DCF 模型，使用实时新闻起草晨间简报，使用当前跨资产定价进行投资组合再平衡，并使用实际融资经济学分析交易。适用于拥有活跃 LSEG 数据权限的用户。

## 入门指南

所有由 Anthropic 开发的插件——金融分析、投资银行、股票研究、私募股权和财富管理——现已可用，可以在我们的[公共金融插件代码库](https://github.com/anthropics/financial-services-plugins)中找到，来自 LSEG 和 S&P Global 的合作伙伴开发的插件也在此处。您可以直接安装它们，或将其作为起点，根据您公司的特定工作流程、术语和需求进行定制。

要将 Claude 连接到 FactSet、MSCI 或其他企业数据源，请访问 Claude 设置中的连接器目录。管理员可以从那里授权连接并将其捆绑为插件供团队使用。

要开始使用 Claude 在 Excel 和 PowerPoint 之间协同工作，请下载 Claude in Excel 和 Claude in PowerPoint 的加载项。有关跨所有职能的 Cowork 和插件更新的更广泛介绍，请参阅[配套博客文章](https://claude.com/blog/cowork-plugins-across-enterprise)。
