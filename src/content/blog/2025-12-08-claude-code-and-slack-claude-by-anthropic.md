---
title: "Claude Code 和 Slack | Claude by Anthropic"
description: "---"
pubDate: "2025-12-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-and-slack"
---

# Claude Code 和 Slack | Claude by Anthropic

---

今天，我们推出了直接从 Slack 向 Claude Code 委派任务的功能。作为研究预览版（beta），Claude 让您能够轻松地将 Slack 对话中的上下文传递到编码会话中。

## **从讨论到实现**

工程工作的关键上下文通常存在于 Slack 中，包括漏洞报告、功能请求和工程讨论。当出现漏洞报告或团队成员需要代码修复时，您现在可以在 Slack 中 @提及 Claude，让其自动利用周围上下文启动一个 Claude Code 会话。可用于：

* **漏洞调查与修复**：要求 Claude 在漏洞报告后立即调查并修复。
* **快速代码审查与修改**：让 Claude 根据团队反馈实现小功能或重构代码。
* **协作调试**：当团队讨论提供了关键上下文（如错误重现或用户报告）时，Claude 可以利用这些信息来指导其调试方法。

## **自动将任务路由到 Claude Code**

此功能扩展了我们现有的 [Claude 的 Slack 应用](https://www.claude.com/blog/claude-and-slack)，允许 Claude 将任务转发回网页版的 Claude Code。当您在 Slack 中 @提及 @Claude 时，Claude 会审查您的消息以确定是否为编码任务。如果是，则会自动创建一个新的 Claude Code 会话。您也可以手动指示 Claude 将请求作为编码任务处理。

Claude 从 Slack 中最近的频道和线程消息中收集上下文，并将其输入到 Claude Code 会话中。它会根据您已在网页版 Claude Code 上认证的仓库，自动选择要在哪个仓库上运行任务。

随着 Claude Code 会话的进行，Claude 会将状态更新发布回您的 Slack 线程。完成后，您会看到一个完整会话的链接，您可以在其中查看更改，以及一个直接打开拉取请求的链接。

## **开始使用**

要开始使用，请通过 [Slack 应用市场](https://slack.com/marketplace/A08SF47R6P4) 确保 Claude 应用已安装到您的 Slack 工作区。安装后，使用您的 Claude 账户进行身份验证，然后开始 @提及 @Claude 处理编码任务。您需要能够访问 [网页版 Claude Code](https://www.claude.com/blog/claude-code-on-the-web)，以便 Claude 路由编码任务。

[浏览文档](https://code.claude.com/docs/en/slack) 了解更多信息。

‍
