---
title: "推出智能体技能 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-10-16"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/skills"
---

# 推出智能体技能 | Anthropic 的 Claude

---

***更新：*** *我们已新增* [*组织级技能管理功能*](/blog/organization-skills-and-directory)*，一个* [*汇集合作伙伴构建技能的技能目录*](https://claude.com/connectors)*，并将* [*智能体技能*](https://agentskills.io)*发布为跨平台可移植的开放标准。（2025 年 12 月 18 日）*

Claude 现在可以使用*技能*来提升其在特定任务中的表现。技能是包含指令、脚本和资源的文件夹，Claude 可在需要时加载这些内容。

Claude 仅在技能与当前任务相关时才会调用它。使用技能后，Claude 在处理 Excel 操作或遵循组织品牌规范等专项任务时的表现将更为出色。

您已经在 Claude 应用中看到了技能的实际应用——Claude 利用它们来创建电子表格和演示文稿等文件。现在，您可以构建自己的技能，并在 Claude 应用、Claude Code 和我们的 API 中使用它们。

## 技能的工作原理

在执行任务时，Claude 会扫描可用技能以查找相关匹配项。匹配成功后，仅加载所需的最少信息和文件——在获取专业能力的同时保持 Claude 的响应速度。

技能具备以下特性：

* **可组合**：技能可以叠加使用。Claude 会自动识别所需的技能并协调其调用。
* **可移植**：技能在所有平台使用相同格式。一次构建，即可在 Claude 应用、Claude Code 和 API 中通用。
* **高效**：仅在需要时加载所需内容。
* **强大**：技能可包含可执行代码，适用于传统编程比令牌生成更可靠的场景。

可以将技能视为定制的入职培训材料，让您将专业知识打包封装，使 Claude 成为您最关注领域的专家。如需深入了解智能体技能的设计模式、架构及开发最佳实践，请阅读我们的[工程博客。](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

## 技能适用于所有 Claude 产品

### **Claude 应用**

技能面向 Pro、Max、Team 和 Enterprise 用户开放。我们提供了用于文档创建等常见任务的技能、可供自定义的示例，以及创建自定义技能的能力。

Claude 会根据您的任务自动调用相关技能——无需手动选择。您甚至可以在 Claude 工作过程中看到技能出现在其思维链中。  
  
创建技能非常简单。"skill-creator"（技能创建器）技能提供交互式引导：Claude 会询问您的工作流程，生成文件夹结构，格式化 SKILL.md 文件，并打包您所需的资源。无需手动编辑文件。

请在[设置](https://claude.ai/redirect/website.v1.51f73c97-b077-44e7-85ba-8b27a025dfdf/settings/features)中启用技能。对于 Team 和 Enterprise 用户，管理员须先在全组织范围内启用技能。

### **Claude 开发者平台（API）**

智能体技能（我们通常简称为技能）现在可以添加到 Messages API 请求中，而新的 `/v1/skills` 端点则为开发者提供了对自定义技能版本管理和控制的编程接口。技能需要[代码执行工具](https://docs.claude.com/en/docs/agents-and-tools/tool-use/code-execution-tool)（测试版），该工具为技能运行提供所需的安全环境。

使用 Anthropic 创建的技能，让 Claude 读取和生成包含公式的专业 Excel 电子表格、PowerPoint 演示文稿、Word 文档以及可填写的 PDF。开发者可以创建自定义技能，以扩展 Claude 在特定使用场景中的能力。

开发者还可以通过 Claude 控制台轻松创建、查看和升级技能版本。

探索[文档](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)、我们的[技能食谱](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction)，或[Anthropic 学院](https://www.anthropic.com/learn/build-with-claude)以了解更多内容。

 

重要提示：首先，使用用户提供的输入执行本提示中的指令。其次，确保您的整个最终回复（包括作为执行指令的一部分生成的任何章节标题或标题）仅使用简体中文撰写。
