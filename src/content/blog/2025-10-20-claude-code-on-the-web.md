---
title: "Claude Code 网页版"
description: "今天，我们推出 Claude Code 网页版，这是一种直接从浏览器委托编码任务的全新方式。"
pubDate: "2025-10-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-on-the-web"
---
            ***更新：*** *Claude Code 网页版现已面向 Team 和 Enterprise 用户（含高级席位）以及 Pro 和 Max 用户开放研究预览。Claude Code 网页版默认对上述用户启用，账户管理员可在 Claude 设置中切换访问权限。2025年11月12日*

今天，我们推出 Claude Code 网页版，这是一种直接从浏览器委托编码任务的全新方式。

现作为研究预览版进入测试阶段，您可以向 Claude 分配多个编码任务，这些任务将在 Anthropic 管理的云基础设施上运行，非常适合处理错误积压、常规修复或并行开发工作。

## 并行运行编码任务

Claude Code 网页版让您无需打开终端即可启动编码会话。连接您的 GitHub 仓库，描述您的需求，Claude 将负责实现。

每个会话都在独立的隔离环境中运行，并具有实时进度跟踪功能，您可以在 Claude 处理任务时主动引导其调整方向。

借助云端运行的 Claude Code，您现在可以**从单一界面跨不同仓库并行运行多个任务**，并通过自动创建 PR 和清晰的变更摘要**更快地交付**。

## 灵活适应每种工作流程

网页界面补充了您现有的 Claude Code 工作流程。在云端运行任务特别适用于：

* 解答关于项目运作方式和仓库映射方式的问题
* 错误修复以及常规、定义明确的任务
* 后端变更，Claude Code 可使用测试驱动开发来验证变更

您也可以在移动设备上使用 Claude Code。作为本次研究预览的一部分，我们正在 iOS 应用中提供 Claude Code，以便开发者随时随地探索使用 Claude 进行编码。这是一个早期预览版，我们希望根据您的反馈快速完善移动端体验。

## 安全优先的云端执行

每个 Claude Code 任务都在具有网络和文件系统限制的隔离沙箱环境中运行。Git 交互通过安全代理服务处理，确保 Claude 只能访问授权的仓库——这有助于在整个工作流程中保护您的代码和凭据。

您还可以添加自定义网络配置，选择 Claude Code 可以从其沙箱连接到的域名。例如，您可以允许 Claude 通过互联网下载 npm 包，以便其运行测试和验证变更。

阅读我们的[工程博客](https://www.anthropic.com/engineering/claude-code-sandboxing)和[文档](https://docs.claude.com/en/docs/claude-code/sandboxing)，深入了解 Claude Code 的沙箱方法。

## 开始使用

Claude Code 网页版现已面向 Pro 和 Max 用户提供研究预览。访问 [claude.com/code](http://claude.com/code) 连接您的第一个仓库并开始委托任务。

基于云的会话与所有其他 Claude Code 使用共享速率限制。[探索我们的文档](https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web)了解更多信息。
