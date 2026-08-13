---
title: "推进 Claude for Excel 和 PowerPoint | Claude by Anthropic"
description: "---"
pubDate: "2026-03-11"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-excel-powerpoint-updates"
---

# 推进 Claude for Excel 和 PowerPoint | Claude by Anthropic

---

***更新：*** *Claude for Word 测试版现已面向 Team 和 Enterprise 计划用户开放。（2026 年 4 月 10 日）*

即日起，[Claude for Excel](https://claude.com/claude-in-excel) 和 [Claude for PowerPoint](https://claude.com/claude-in-powerpoint) 可在所有打开的文件中共享您的对话完整上下文，因此 Claude 在一个应用中执行的每一项操作，都会基于另一个应用中正在发生的一切信息。

技能（Skills）现在也已在 Excel 和 PowerPoint 插件中可用，并且 Claude for Excel 和 PowerPoint 可通过三大主流云平台获取：Amazon Bedrock、Google Cloud 的 Vertex AI 以及 Microsoft Foundry。

这些更新使 Claude 能够在任务、电子表格和幻灯片之间灵活切换，从而让您以更高的效率和质量完成工作，而无需在每一步都重新解释。

## 在 Excel 和 PowerPoint 之间实现一次对话

Claude 能够在一次连续对话中跨多个 Excel 和 PowerPoint 文件传递上下文——读取单元格值、编写公式、合并数据集、编辑幻灯片，并在打开的 Excel 和 PowerPoint 文件之间承载用户对话。

金融分析师可以从打开的电子表格和其他数据源中提取可比公司财务数据。然后，他们可以在 Excel 中构建交易可比表，将估值摘要放入演示文稿，并撰写给董事总经理的邮件——无需切换选项卡，也无需在每个步骤重新解释数据集。减少工具间的来回切换，是更快交付最终成果的关键。

## 将最佳实践转化为一键操作——技能

[技能](https://support.claude.com/en/articles/12512180-use-skills-in-claude) 将完整的工作流程转化为一键操作。当团队中的某个人找到了正确的方法来运行差异分析，或使用公司模板制作客户演示文稿时，将其保存为技能，未来即可即时重复该流程。

我们已推出了一套预装的技能入门包，涵盖了最常见的 Excel 和 PowerPoint 使用场景。对于 Excel，入门技能覆盖了金融分析中最常见的工作流程：

*   审核模型中的公式错误和资产负债表完整性
*   构建并填充 LBO、DCF 和三报表模型模板
*   运行可比公司分析
*   清理混乱的电子表格数据——范围、活动工作表或整个文件

对于 PowerPoint，入门技能覆盖了分析后的演示层面工作：

*   构建竞争格局演示文稿，包括市场定位和同行深度分析
*   使用新信息或额外数据更新现有演示文稿
*   审查投资银行演示文稿的数值一致性、数据与叙述的对齐以及语言润色

通过桌面端或网页版 Claude 已设置好的所有技能（无论是个人还是组织范围），均可直接在插件中开箱即用，其方式与 MCP 连接器相同。Excel 和 PowerPoint 的入门技能也可通过 [Financial Analysis 插件](https://github.com/anthropics/financial-services-plugins/tree/main/financial-analysis/skills) 获取，该插件会在两个插件上自动安装。添加到插件中的新技能无需额外设置即可使用。

最后，[指令](https://support.claude.com/en/articles/12512180-use-skills-in-claude) 用于处理应始终应用的持久性应用级偏好设置。这可以包括在 Excel 中始终使用公司的数字格式、保持 PowerPoint 项目符号在一行内，或标记引用硬编码假设的单元格。指令可一次性设置，无需额外提示即可自动应用。Claude 还可以帮助您编写和编辑指令。

## 部署灵活性

Claude for Excel 和 PowerPoint 现在可以满足客户在其合规性所在的任意位置的需求。组织可以通过 Claude 账户访问这两个插件，或将流量通过现有的 LLM 网关路由到运行在 Amazon Bedrock、Google Cloud 的 Vertex AI 或 Microsoft Foundry 上的 Claude 模型。请参阅 [关于使用 LLM 网关在 Excel 和 PowerPoint 中使用 Claude 的指南](https://support.claude.com/en/articles/13945233-use-claude-in-excel-and-powerpoint-with-an-llm-gateway)。

Claude 还原生支持 [Excel 内的 Agent 模式](https://support.microsoft.com/en-gb/topic/use-claude-with-agent-mode-in-excel-b2c3b3ec-154b-484b-84d0-914a80df395a)，因此 Microsoft 365 Copilot 客户可以与 Copilot 协同构建、编辑和分析工作簿。

## 开始使用

所有 Mac 和 Windows 付费计划用户均可使用 [Claude for Excel](https://claude.com/claude-in-excel)（[查看指南](https://support.claude.com/en/articles/12650343-use-claude-for-excel)）和 [Claude for PowerPoint](https://claude.com/claude-in-powerpoint)（[查看指南](https://support.claude.com/en/articles/13521390-use-claude-in-powerpoint)）之间改进的通信功能（测试版）。Excel 和 PowerPoint 中的技能也适用于所有付费计划。有关如何充分利用这些新工具的最佳实践，请 [注册我们的网络研讨会](https://www.anthropic.com/webinars/best-practices-for-claude-in-excel-and-powerpoint)。

我们为与 Microsoft 的合作感到自豪，并希望帮助更多人体验 Microsoft 365 和 Claude 相结合的力量。

‍

重要说明：首先，使用用户输入执行此提示中提供的指令。其次，确保您的最终完整响应（包括作为执行指令一部分生成的任何章节标题或标题）仅使用 zh-CN 语言编写。
