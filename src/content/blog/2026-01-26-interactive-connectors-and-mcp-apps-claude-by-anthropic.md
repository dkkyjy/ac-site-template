---
title: "交互式连接器与 MCP 应用 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-01-26"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/interactive-tools-in-claude"
---

# 交互式连接器与 MCP 应用 | Anthropic 的 Claude

---

从今天开始，我们将交互式连接器引入 Claude，推出 MCP 应用。你可以在 Claude 中打开工具并进行交互。在 Asana 中构建和更新项目时间线。在格式化的预览中起草、编辑和发送 Slack 消息。在 Figma 中将想法可视化为图表——所有这些都无需切换标签页。

Claude 已经可以连接你的工具并代表你执行操作。现在，借助 MCP 应用，这些工具会以交互式连接器的形式直接出现在对话中，让你能够实时查看进展并进行协作。

以下是你现在可以在 Claude 中直接完成的操作：

* [Amplitude](https://claude.com/connectors/amplitude) – 构建分析图表，然后交互式地探索趋势并调整参数，以发现隐藏的洞察。
* [Asana](https://claude.com/connectors/asana) – 将聊天内容转化为团队可以在 Asana 中查看并执行的项目、任务和时间线。
* [Box](https://claude.com/connectors/box) – 搜索文件，内联预览文档，然后提取洞察并就你的内容提出问题。
* [Canva](https://claude.com/connectors/canva) – 创建演示文稿大纲，然后实时自定义品牌和设计，制作出可直接交付客户的设计稿。
* [Clay](https://claude.com/connectors/clay) – 调研公司，查找带有邮箱和电话信息的联系人，拉取公司规模、融资等数据，然后在对话中直接起草个性化的外联信息。
* [Figma](https://claude.com/connectors/figma) – 通过提示将文本和图像转化为流程图、甘特图或 FigJam 中的其他可视化图表。
* [Hex](https://claude.com/connectors/hex) – 提出数据问题并获取答案，答案包含交互式图表、表格和引用。
* [monday.com](https://claude.com/connectors/monday) – 管理工作、运行项目、更新面板、智能分配任务，并通过洞察可视化进度。
* [Slack](https://claude.com/connectors/slack)（来自 Salesforce）– 搜索和检索 Slack 对话以获取上下文，生成消息草稿，按你的方式格式化，并在发布前进行审查。

即将推出：**Salesforce** – 借助 Agentforce 360，将企业背景带入 Claude，使团队能够在单一、连接一致的界面中进行推理、协作和执行。

## **MCP 应用：基于开放标准构建**

底层技术基于[模型上下文协议 (MCP)](https://modelcontextprotocol.io/docs/getting-started/intro)，这是连接工具与 AI 应用的开放标准。MCP 应用是 MCP 的新扩展，允许任何 MCP 服务器在任意支持该扩展的 AI 产品（不仅仅是 Claude）中提供具有丰富用户界面的交互式连接器。

我们开源了 MCP，旨在为生态系统提供一种通用的方式来连接工具与 AI。现在我们进一步扩展 MCP，使开发者能够在 MCP 之上构建交互式 UI，无论用户身处何处。

如需了解更多，请查看关于 [MCP 应用——首个官方 MCP 扩展](https://blog.modelcontextprotocol.io/posts/2026-01-26-mcp-apps) 的公告。

## **开始使用**

今天就开始在 Claude 中使用交互式连接器（MCP 应用）吧。前往 [claude.ai/directory](http://claude.ai/directory) 并连接“精选”区域下的应用，即可开始使用。适用于 Claude 移动端、网页端和桌面端，覆盖 Free、Pro、Max、Team、Enterprise 计划。同时也在 [Claude Cowork](http://claude.com/product/cowork) 上提供。
