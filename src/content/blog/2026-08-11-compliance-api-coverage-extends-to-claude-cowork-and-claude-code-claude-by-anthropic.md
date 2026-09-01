---
title: "合规 API 覆盖范围扩展至 Claude Cowork 和 Claude Code | Anthropic 出品 Claude"
description: "---"
pubDate: "2026-08-11"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/compliance-api-cowork-and-claude-code"
---

# 合规 API 覆盖范围扩展至 Claude Cowork 和 Claude Code | Anthropic 出品 Claude

---

***更新：合规 API 对 Cowork（桌面端、网页端和移动端）及 Claude Code（命令行界面和桌面端）的覆盖现已全面可用；Microsoft 365 加载项（Excel、Word、PowerPoint 和 Outlook）及 Claude Science 的覆盖处于测试版阶段（2026 年 8 月 26 日）***

Claude 的合规 API 现已覆盖 Cowork 的桌面应用、网页端和移动端，以及 Claude Code 的命令行界面和桌面应用。该覆盖功能目前处于测试版阶段，面向 Claude 企业版客户。合规与安全团队可通过与现有 Claude 对话相同的合规 API 接口，从这两款产品中提取会话内容和元数据。

新增的端点为增量式扩展：您目前从合规 API 获取的数据不会发生任何变化。

安全与合规团队依赖合规 API 来了解 Claude 在其组织中的使用情况——用于审计和电子取证（eDiscovery）——而无需为每个使用场景部署独立的日志基础设施。将覆盖范围扩展至 Cowork 和 Claude Code 填补了一项空白：这些会话现在将与 Claude 对话一并呈现。

## 工作原理

新增的会话端点为每个 Cowork 和 Claude Code 会话返回一份统一的、由服务器托管的完整记录，因此提示词、回复和工具活动将作为单条会话记录一并返回。

每条会话记录包含两类数据：

* **会话内容：** 提示词和回复、工具调用内容（网页和 MCP），以及以转写文本形式捕获的技能与产物内容。
* **会话元数据：** 经验证的用户 ID 和电子邮件地址、组织 ID、会话 ID 和逐条消息 ID，以及时间戳。

本次测试版不包括网页版 Claude Code、通过 Claude 平台访问的 Claude Code，以及在 Amazon Bedrock、Google Cloud 的 Vertex AI 或 Microsoft Foundry 上运行的会话。

已在导出 OpenTelemetry 数据的组织可继续运行现有流程：合规 API 可与之协同工作，无需您进行任何基础设施配置。

## 开始使用

Cowork 和 Claude Code 的覆盖功能现已可用，并包含在您现有的合规 API 中——使用现有的合规访问密钥即可，无需构建单独的集成。如果该功能已为您的组织启用，可直接查询新的会话端点。如尚未启用，请参阅合规 API [文档](https://platform.claude.com/docs/en/manage-claude/compliance-api)以启用该功能。
