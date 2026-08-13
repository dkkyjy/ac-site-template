---
title: "网页版 Claude Code | Anthropic 的 Claude"
description: "---"
pubDate: "2025-10-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-on-the-web"
---

# 网页版 Claude Code | Anthropic 的 Claude

---

***更新：*** *网页版 Claude Code 现已面向 Pro 和 Max 用户以及拥有高级席位的团队和企业用户提供研究预览。对于这些用户，网页版 Claude Code 默认开启，账户管理员可在 Claude 设置中切换访问权限。2025年11月12日*

今天，我们推出网页版 Claude Code，这是一种直接从浏览器委派编码任务的全新方式。

作为研究预览版现已进入 Beta 阶段，您可以将多个编码任务分配给 Claude，这些任务将在 Anthropic 管理的云基础设施上运行，非常适合处理缺陷积压、常规修复或并行开发工作。

## 并行运行编码任务

网页版 Claude Code 让您无需打开终端即可启动编码会话。连接您的 GitHub 仓库，描述您的需求，Claude 负责实现。

每个会话都在独立的隔离环境中运行，具有实时进度跟踪功能，您可以主动引导 Claude 在任务执行过程中调整方向。

借助云端运行的 Claude Code，您现在可以从单一界面**跨不同仓库并行运行多个任务**，并通过自动创建 PR 和清晰的变更摘要**加快交付速度**。

## 适应每种工作流程的灵活性

网页界面补充了您现有的 Claude Code 工作流程。在云端运行任务尤其适用于以下场景：

* 解答关于项目运作方式和仓库映射方式的问题
* 修复缺陷以及常规的、定义明确的任务
* 后端变更——Claude Code 可使用测试驱动开发来验证变更

您也可以在移动设备上使用 Claude Code。作为本次研究预览的一部分，我们已在 iOS 应用上提供 Claude Code，以便开发者随时随地探索用 Claude 进行编码。这是一个早期预览，我们希望能根据您的反馈快速完善移动端体验。

## 以安全为先的云端执行

每个 Claude Code 任务都在具备网络和文件系统限制的独立沙箱环境中运行。Git 交互通过一个安全代理服务处理，确保 Claude 只能访问已授权的仓库——从而在整个工作流程中保护您的代码和凭据。

您还可以添加自定义网络配置，选择 Claude Code 从沙箱中可以连接哪些域名。例如，您可以允许 Claude 通过互联网下载 npm 包，以便运行测试和验证变更。

请阅读我们的[工程博客](https://www.anthropic.com/engineering/claude-code-sandboxing)和[文档](https://docs.claude.com/en/docs/claude-code/sandboxing)，深入了解 Claude Code 的沙箱方法。

## 开始使用

网页版 Claude Code 现已面向 Pro 和 Max 用户提供研究预览。访问 [claude.com/code](http://claude.com/code) 连接您的第一个仓库，开始委派任务。

基于云端的会话与所有其他 Claude Code 使用量共享速率限制。请[查阅我们的文档](https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web)了解更多信息。
