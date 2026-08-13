---
title: "Claude Code 自动模式 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-03-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/auto-mode"
---

# Claude Code 自动模式 | Anthropic 的 Claude

---

***更新***：自动模式现已面向所有 Claude Code 用户正式可用。（2026年7月10日）

今天，我们推出自动模式，这是 Claude Code 中的一种新权限模式，在该模式下 Claude 将代表您做出权限决策，并在操作运行前通过安全监控进行审查。该功能目前作为研究预览版面向 Team 计划用户开放，后续几天内将面向 Enterprise 计划用户和 API 用户推出。

## 工作原理

Claude Code 的默认权限故意设置得较为保守：每次文件写入和 bash 命令都需要请求批准。这是一种安全的默认设置，但意味着您无法启动一个大型任务后离开，因为 Claude 会沿途频繁请求人工批准。虽然有些开发者选择使用 `--dangerously-skip-permissions` 绕过权限检查，但跳过权限可能导致危险及破坏性后果，不应在隔离环境之外使用。

自动模式是一条中间路径，让您可以运行更长时间的任务，减少中断次数，同时比完全跳过权限引入更小的风险。在每个工具调用执行之前，分类器会对其进行审查，以[检查是否有潜在破坏性操作](https://code.claude.com/docs/en/permission-modes#what-the-classifier-blocks-by-default)，例如批量删除文件、敏感数据泄露或恶意代码执行。

分类器认为安全的操作会自动进行，而有风险的操作会被阻止，并引导 Claude 采用不同的方法。如果 Claude 坚持执行持续被阻止的操作，最终会向用户触发权限提示。

## 预期效果

自动模式相比 `--dangerously-skip-permissions` 降低了风险，但并未完全消除风险，我们仍建议在隔离环境中使用。分类器有时仍可能允许某些风险操作：例如，当用户意图不明确时，或者 Claude 没有足够了解您的环境背景而无法知晓某个操作可能带来额外风险时。分类器偶尔也可能阻止良性操作。我们将持续改进体验。

自动模式可能会对工具调用的 token 消耗、成本和延迟产生轻微影响。

## 开始使用

自动模式现在作为研究预览版面向 Claude Team 用户在 Claude Code 中提供，后续几天内将向 Enterprise 和 API 用户推出。它同时支持 Claude Sonnet 4.6 和 Opus 4.6。

* **对于管理员**：自动模式不久后将面向所有使用 Enterprise、Team 和 Claude API 计划的 Claude Code 用户开放。要禁用 CLI 和 VS Code 扩展中的自动模式，请在托管设置中将 `"disableAutoMode"` 设置为 `"disable"`。自动模式在 Claude 桌面应用中默认禁用，可通过「组织设置」->「Claude Code」来开启。
* **对于开发者**：运行 `claude --enable-auto-mode` 以启用自动模式，然后使用 Shift+Tab 切换到该模式。在桌面版和 VS Code 扩展中，首先在「设置」->「Claude Code」中开启自动模式，然后在会话的权限模式下拉菜单中选择该模式。

[查阅文档](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode)获取更多信息。
