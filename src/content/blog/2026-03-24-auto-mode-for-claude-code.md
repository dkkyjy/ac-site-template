---
title: "Claude Code 的自动模式"
description: "今天，我们推出自动模式，这是 Claude Code 中的一种全新权限模式。在该模式下，Claude 将代表你做出权限决策，并在操作执行前通过安全监控进行审查。该功能现以研究预览形式面向团队版用户提供，即将于未来数日内面向企业版用户和 API 用户推出。"
pubDate: "2026-03-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/auto-mode"
---
            今天，我们推出自动模式，这是 Claude Code 中的一种全新权限模式。在该模式下，Claude 将代表你做出权限决策，并在操作执行前通过安全监控进行审查。该功能现以研究预览形式面向团队版用户提供，即将于未来数日内面向企业版用户和 API 用户推出。

## 工作原理

Claude Code 的默认权限设置有意保持保守：每次文件写入和 bash 命令都需要请求批准。这是一种安全的默认设置，但这也意味着你无法启动一个大型任务后离开，因为 Claude 会在此过程中频繁请求人工批准。虽然有些开发者选择使用 `--dangerously-skip-permissions` 绕过权限检查，但跳过权限可能导致危险和破坏性后果，不应在隔离环境之外使用。

自动模式是一条中间路径，让你能够运行更长时间的任务，减少中断次数，同时比完全跳过所有权限引入更小的风险。在每个工具调用运行之前，分类器会对其进行审查，[检查是否存在潜在破坏性操作](https://code.claude.com/docs/en/permission-modes#what-the-classifier-blocks-by-default)，如批量删除文件、敏感数据泄露或恶意代码执行。

分类器认为安全的操作会自动执行，而风险操作则会被阻止，并引导 Claude 采取不同方法。如果 Claude 坚持执行持续被阻止的操作，最终将触发权限提示，请求用户批准。

## 预期效果

自动模式相比 `--dangerously-skip-permissions` 降低了风险，但并未完全消除风险，我们仍建议在隔离环境中使用。分类器可能仍会允许某些风险操作：例如，当用户意图不明确时，或者当 Claude 对你的环境没有足够了解，不知道某个操作可能带来额外风险时。分类器有时也可能阻止良性操作。我们将持续改进这一体验。

自动模式可能会对令牌消耗、成本和工具调用的延迟产生轻微影响。

## 开始使用

自动模式现以研究预览形式面向 Claude 团队版用户提供，并将在未来数日内向企业版和 API 用户推出。该功能同时适用于 Claude Sonnet 4.6 和 Opus 4.6。

* **对于管理员**：自动模式即将面向所有使用企业版、团队版和 Claude API 计划的 Claude Code 用户开放。若要在 CLI 和 VS Code 扩展中禁用该功能，请在托管设置中将 `"disableAutoMode"` 设置为 `"disable"`。自动模式在 Claude 桌面应用中默认禁用，可通过"组织设置"->"Claude Code"启用。
* **对于开发者**：运行 `claude --enable-auto-mode` 以启用自动模式，然后使用 Shift+Tab 键切换到该模式。在桌面应用和 VS Code 扩展中，首先在"设置"->"Claude Code"中启用自动模式，然后在会话中从权限模式下拉菜单中选择该模式。

[查阅文档](https://code.claude.com/docs/en/permission-modes#eliminate-prompts-with-auto-mode)了解更多信息。
