---
title: "Claude Code 与 Slack"
description: "今天，我们推出了一项新功能：可以直接从 Slack 将任务委派给 Claude Code。该功能目前作为研究预览版进入 Beta 阶段，Claude 让您能够轻松地将 Slack 对话中的上下文迁移到编码会话中。"
pubDate: "2025-12-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-and-slack"
---
            今天，我们推出了一项新功能：可以直接从 Slack 将任务委派给 Claude Code。该功能目前作为研究预览版进入 Beta 阶段，Claude 让您能够轻松地将 Slack 对话中的上下文迁移到编码会话中。

## **从讨论到实现**

工程工作相关的关键上下文通常存在于 Slack 中，包括错误报告、功能请求以及工程讨论。当出现错误报告或团队成员需要代码修复时，您现在可以在 Slack 中标记 Claude，它会自动利用周围的上下文启动一个 Claude Code 会话。可用于：

* **错误调查与修复**：要求 Claude 在错误报告后立即进行调查和修复。
* **快速代码审查与修改**：让 Claude 根据团队反馈实现小功能或重构代码。
* **协作调试**：当团队讨论提供了关键上下文（如错误复现步骤或用户报告）时，Claude 可以利用这些信息来指导其调试方法。

## **自动将任务路由至 Claude Code**

此功能扩展了我们现有的 [Claude for Slack 应用](https://www.claude.com/blog/claude-and-slack)，允许 Claude 将任务回传给网页端的 Claude Code。当您在 Slack 中提及 @Claude 时，Claude 会审查您的消息以判断是否为编码任务。如果是，系统将自动创建一个新的 Claude Code 会话。您也可以手动指示 Claude 将请求作为编码任务处理。

Claude 会从 Slack 中的近期频道和线程消息中收集上下文，并将其输入到 Claude Code 会话中。它会根据您在网页端已向 Claude Code 进行身份验证的仓库，自动选择在哪个仓库上运行任务。

随着 Claude Code 会话的进行，Claude 会向您的 Slack 线程发布状态更新。任务完成后，您将找到指向完整会话的链接，可在其中查看更改内容，以及一个直接打开拉取请求的链接。

## **开始使用**

要开始使用，请通过 [Slack 应用市场](https://slack.com/marketplace/A08SF47R6P4) 确保 Claude 应用已安装到您的 Slack 工作区中。安装完成后，使用您的 Claude 账户进行身份验证，然后开始提及 @Claude 以处理编码任务。您需要拥有 [Claude Code 网页版](https://www.claude.com/blog/claude-code-on-the-web) 的访问权限，以便 Claude 路由编码任务。

[查阅文档](https://code.claude.com/docs/en/slack) 了解更多信息。
