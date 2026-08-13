---
title: "引入智能体技能"
description: "Claude 现在可以使用*技能*来改进其执行特定任务的方式。技能是包含指令、脚本和资源的文件夹，Claude 可在需要时加载这些内容。"
pubDate: "2025-10-16"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/skills"
---
            ***更新：*** *我们已添加* [*面向组织的技能管理功能*](/blog/organization-skills-and-directory)*，推出* [*技能目录*](https://claude.com/connectors) *展示合作伙伴构建的技能，并将* [*智能体技能*](https://agentskills.io) *发布为跨平台可移植性的开放标准。（2025年12月18日）*

Claude 现在可以使用*技能*来改进其执行特定任务的方式。技能是包含指令、脚本和资源的文件夹，Claude 可在需要时加载这些内容。

Claude 仅在技能与当前任务相关时才会访问该技能。使用时，技能能让 Claude 在诸如处理 Excel 或遵循您组织的品牌指南等专业任务上表现更出色。

您已经在 Claude 应用中见识过技能的作用，Claude 使用技能来创建电子表格和演示文稿等文件。现在，您可以构建自己的技能，并在 Claude 应用、Claude Code 以及我们的 API 中使用它们。

## 技能的工作原理

在执行任务时，Claude 会扫描可用技能以找到相关匹配项。当匹配成功时，它仅加载所需的最少信息和文件——既保持 Claude 的快速响应，又能访问专业能力。

技能具有以下特性：

* **可组合性**：技能可以叠加使用。Claude 会自动识别需要哪些技能并协调它们的使用。
* **可移植性**：技能在任何地方都使用相同的格式。一次构建，即可在 Claude 应用、Claude Code 和 API 中使用。
* **高效性**：仅在需要时加载所需内容。
* **强大性**：技能可包含可执行代码，用于处理传统编程比令牌生成更可靠的任务。

将技能视为自定义的入职培训材料，让您能够打包专业知识，使 Claude 成为您最关心领域的专家。如需深入了解智能体技能的设计模式、架构和开发最佳实践，请阅读我们的[工程博客](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)。

## 技能适用于所有 Claude 产品

### **Claude 应用**

技能适用于 Pro、Max、Team 和 Enterprise 用户。我们为常见任务（如文档创建）提供技能、可供自定义的示例，以及创建您自己自定义技能的能力。

Claude 会根据您的任务自动调用相关技能——无需手动选择。您甚至可以在 Claude 的思维链中看到技能的工作过程。

创建技能非常简单。"技能创建器"技能提供交互式指导：Claude 会询问您的工作流程，生成文件夹结构，格式化 SKILL.md 文件，并打包您所需的资源。无需手动编辑文件。

在[设置](https://claude.ai/redirect/website.v1.51f73c97-b077-44e7-85ba-8b27a025dfdf/settings/features)中启用技能。对于 Team 和 Enterprise 用户，管理员必须首先在组织范围内启用技能。

### **Claude 开发者平台（API）**

智能体技能（我们通常简称为技能）现在可以添加到 Messages API 请求中，新的 `/v1/skills` 端点为开发者提供了对自定义技能版本控制和管理的编程控制能力。技能需要[代码执行工具](https://docs.claude.com/en/docs/agents-and-tools/tool-use/code-execution-tool)测试版，该工具提供了技能运行所需的安全环境。

使用 Anthropic 创建的技能，让 Claude 读取并生成包含公式的专业 Excel 电子表格、PowerPoint 演示文稿、Word 文档和可填写的 PDF 文件。开发者可以创建自定义技能，以扩展 Claude 针对特定用例的能力。

开发者还可以通过 Claude Console 轻松创建、查看和升级技能版本。

探索[文档](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)、我们的[技能食谱](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction)或 [Anthropic Academy](https://www.anthropic.com/learn/build-with-claude) 以了解更多信息。

### **Claude Code**

技能通过您团队的专业知识和工作流程扩展 Claude Code。通过 anthropics/skills 市场中的插件安装技能。Claude 会在相关时自动加载它们。通过版本控制与您的团队共享技能。您也可以通过将技能添加到 `~/.claude/skills` 来手动安装。Claude 智能体 SDK 为构建自定义智能体提供相同的智能体技能支持。

## 入门指南

* **Claude 应用：** [用户指南](https://support.claude.com/en/articles/12580051-teach-claude-your-way-of-working-using-skills)和[帮助中心](https://support.claude.com/en/articles/12512176-what-are-skills)
* **API 开发者：** [文档](https://docs.claude.com/en/api/skills-guide)
* **Claude Code：** [文档](https://docs.claude.com/en/docs/claude-code/skills)
* **可供自定义的示例技能：** [GitHub 仓库](https://github.com/anthropics/skills)

## 后续计划

我们正在努力简化技能创建工作流程并实现企业级部署能力，使组织能够更轻松地在团队之间分发技能。

请记住，此功能允许 Claude 访问并执行代码。虽然功能强大，但也意味着需要注意您使用的技能——坚持使用可信来源以保护您的数据安全。[了解更多](https://support.claude.com/en/articles/12512180-using-skills-in-claude#h_2746475e70)。
