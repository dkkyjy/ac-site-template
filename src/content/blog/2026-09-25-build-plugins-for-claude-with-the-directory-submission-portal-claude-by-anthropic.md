---
title: "通过目录提交门户为 Claude 构建插件 | Claude by Anthropic"
description: "每天，数以百万计的人将 Claude 连接到他们的应用、工作工具和数据。今天，我们正在让开发者更容易触达这些用户。"
pubDate: "2026-09-25"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/build-plugins-for-claude"
---

# 通过目录提交门户为 Claude 构建插件 | Claude by Anthropic

---

每天，数以百万计的人将 Claude 连接到他们的应用、工作工具和数据。今天，我们正在让开发者更容易触达这些用户。

插件打包了 MCP 连接器、Agent 技能或两者兼有，是构建 Claude 第三方扩展的主要方式。构建一个插件，通过新的目录提交门户提交，一经批准，就会列在 [Claude 目录](https://claude.ai/directory) 中。

## **在目录提交门户中提交和跟踪插件**

[目录提交门户](https://claude.ai/directory/manage/new) 对付费 Claude 计划的开发者开放。有两种方式提交并在 Claude 目录中发布你的插件：

* **单个 MCP 连接器：** 指向你的远程 MCP 服务器。
* **插件包：** 组合 MCP 服务器和技能，将它们托管在 GitHub 上，并提交仓库。在 Claude Code 中，插件还可以包含 LSP、命令、钩子和代理。

无论你选择哪条路径，门户都会引导你从提交到发布。你可以：

* **自动验证你的插件。** 每次提交都会在提交后立即进行检查和安全扫描，以便你尽早发现问题。
* **查看状态和反馈。** 查看你的插件在审核流程中的位置、安全扫描结果以及建议的更改。
* **准备好时发布。** 一经批准，你可以决定何时在 Claude 中发布你的插件。

插件审核状态和建议更改的样式化视图。数据仅供参考。

## **插件发布后监控和改进**

你的插件上线后，使用分析会显示按产品表面和版本划分的安装量，这样你可以优先为用户修复问题和增加功能。在发现方面，你会看到你的列表被查看的频率以及哪些搜索引导人们找到它，这样你可以优化它以触达新用户。

已发布插件使用指标的样式化视图。数据仅供参考。

## **通过 MCP 2.0 及其扩展为用户提供丰富体验**

Claude 支持最新的 MCP 规范，通常被称为 [MCP 2.0](https://modelcontextprotocol.io/specification/2026-07-28)，其中包括无状态核心。你可以使用两个 [MCP 扩展](https://modelcontextprotocol.io/extensions/overview) 来改善使用你的插件的体验：[MCP Apps](https://claude.com/docs/connectors/building/mcp-apps/getting-started)，用于聊天内的交互式 UI，以及 [企业托管身份验证](https://claude.com/docs/connectors/building/enterprise-managed-auth)，用于企业用户的零接触 OAuth。对更多 MCP 功能和扩展的支持即将推出。

## **开始为 Claude 构建插件**

插件是第三方开发者为 Claude 创建扩展的主要方式。在未来几周，一个发现体验将在 Claude 和 Claude Code 中推出。

技能和 MCP 连接器仍然是构建模块，Claude 目录将继续列出它们。将来，开发者将能够将其连接器列表转换为插件。如果你已经在 Claude 目录中有现有的技能、连接器或插件，则不需要做任何更改。

要开始构建插件，请阅读我们的文档了解 [如何构建插件](https://claude.com/docs/build/overview) 并在此处 [提交你的插件](https://claude.ai/directory/manage/new)。
