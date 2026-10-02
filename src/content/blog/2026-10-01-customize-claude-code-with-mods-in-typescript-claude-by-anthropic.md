---
title: "使用 TypeScript 模组自定义 Claude Code | Claude by Anthropic"
description: "今天，我们推出模组（mods）——小型 TypeScript 函数，用于改变 Claude Code 的工作方式。模组可以重写提示词、添加新界面、替换内置功能或添加全新功能。你可以自己编写模组，也可以让 Claude Code 为你编写。模组打包在插件中发布，因此你可以像安装和共享任何插件一样安装和共享模组。它们可在 Claude Code CLI 和桌面应用中运行。"
pubDate: "2026-10-01"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-mods"
---

# 使用 TypeScript 模组自定义 Claude Code | Claude by Anthropic

---

今天，我们推出模组（mods）——小型 TypeScript 函数，用于改变 Claude Code 的工作方式。模组可以重写提示词、添加新界面、替换内置功能或添加全新功能。你可以自己编写模组，也可以让 Claude Code 为你编写。模组打包在插件中发布，因此你可以像安装和共享任何插件一样安装和共享模组。它们可在 Claude Code CLI 和桌面应用中运行。

模组与 Claude Code 本身具有相同的机器访问权限。它们不受沙箱限制，因此你只应安装来自你信任来源的模组，就像在电脑上安装任何代码一样。

要了解模组的功能，[阅读我们的构建第一个模组指南](https://claude.dev/blog/getting-started-with-claude-code-mods/)。

### 我们为何构建模组

开发者希望在不等待我们发布功能的情况下，获得对 Claude Code 工作方式更多的控制权。[钩子（Hooks）](https://code.claude.com/docs/en/hooks) 帮助用户获得了一些这种控制权，但钩子无法重写事件、绘制新界面或替换功能。模组可以。

我们希望 Claude Code 感觉像是你自己的，这样你就可以根据自己的工作方式塑造它。我们在发布前[在 GitHub 上分享了模组的设计](https://github.com/anthropics/claude-code/issues/91870)以获取开发者的反馈。感谢所有参与讨论的人。

### 模组如何工作

每次 Claude Code 执行操作时，它都会发出事件。示例包括调用工具、请求权限和绘制屏幕的一部分。模组是一个挂钩到这些事件之一的函数。模组可以在事件之前、之后或代替事件运行。它还可以包装事件，在事件前后都运行代码。使用一个函数，模组可以：

* 在提示词到达模型之前重写它。
* 阻止、重写或重试工具调用。
* 批准或拒绝权限请求。
* 在 Claude 读取之前从工具输出中删除机密信息。

模组还可以改变你看到的内容。它可以编辑或替换 Claude Code 绘制的界面部分，例如工具结果或来自 Claude 的问题。它可以添加按钮和输入框，其他模组可以响应你的操作。目前，模组可以针对终端、桌面应用或两者。

当多个模组挂钩同一事件时，它们按加载顺序运行。第一个加载的模组最先看到事件，最后看到结果。这允许你堆叠来自不同作者的模组。

你还可以使用 Claude Code 来修改 Claude Code。让 Claude 创建模组，它可以编写 TypeScript、安装并在你的会话中热重载它。

### 用你自己的功能替换内置功能

Claude Code 的一些内置功能现在作为模组发布。例如，内置的 `/diff` 功能现在是一个模组，因此你可以关闭它（在 `/plugin` 中）或用你自己的版本替换它。我们计划随着时间推移将更多内置功能迁移到模组，这样你可以将 Claude Code 精简为小型核心，只添加你想要的功能。

### 面向团队和企业的模组

模组打包在插件中发布，因此你现有的插件控制措施适用。管理员可以允许或阻止插件市场。在 Team 和 Enterprise 计划中，所有者在管理控制台中设置此选项。在 Claude API 和第三方 API 计划中，管理员将托管设置推送到用户机器。

在 Team 和 Enterprise 计划中，以及任何具有托管设置的机器上，一个名为 `sec-default`（"安全默认"）的内置模组首先加载。它阻止用户安装的模组执行危险操作，例如覆盖你的权限拒绝规则。你可以[查看源代码](http://github.com/anthropics/claude-code/tree/main/mods)了解其限制内容。管理员可以改为首先加载自己的模组。如果你这样做，请将 `sec-default` 添加到你的列表中以保持其限制。

团队还可以使用模组构建自己的控制措施和功能。例如：

* **CI/CD 状态：** 模组可以在对话旁边的面板中显示你的流水线状态，并在构建通过或失败时更新它。
* **生产环境保障：** 模组可以在任何命令触及生产环境配置之前要求确认。
* **审计日志：** 首先加载的模组可以记录每个其他模组执行的每个调用。

### 开始使用

模组现已在 Claude Code CLI 和桌面应用中可用。从[Claude 目录](https://claude.ai/directory)安装包含模组的插件，或在 CLI 中运行 `/plugin`。要共享模组，将其打包为插件并[提交到目录](https://claude.ai/directory/manage)。

要构建你自己的模组，阅读我们的[入门指南](https://claude.dev/blog/getting-started-with-claude-code-mods/)或查看[文档](https://code.claude.com/docs/en/plugins/mods/overview)。
