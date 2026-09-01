---
title: "使用 Claude Code 预览、审查与合并 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-02-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/preview-review-and-merge-with-claude-code"
---

# 使用 Claude Code 预览、审查与合并 | Anthropic 的 Claude

---

今天，我们发布了 Claude Code 的多项改进，让你能够预览运行中的应用、自动审查代码、自动修复并合并 PR，并在桌面端、移动端和命令行之间无缝切换。这些更新共同帮助你减少在代码琐碎事务上的时间，将更多精力投入到你真正享受的部分。

## **编写代码，即时预览运行效果**

桌面版 Claude Code 现在可以启动开发服务器，并直接在桌面界面中预览你正在运行的应用。Claude 会查看 Web 应用界面、读取控制台日志、捕获错误并持续迭代，因此你无需切换到浏览器再手动向 Claude 描述你看到的内容。你还可以在预览中选择视觉元素，直接将反馈传递给 Claude 进行迭代。

## **推送前审查代码**

当你的改动看起来没问题后，使用新的"审查代码"按钮让 Claude 进行审查。Claude 会检查你的本地差异，并在桌面端的差异视图中直接留下评论，标出 bug、提出建议，并内联指出潜在问题。

你可以立即获得一双"额外的眼睛"，在代码离开你的机器之前发现明显问题，还可以让 Claude 处理这些行内评论并进行修改。

## **无需离开应用即可监控 PR**

对于托管在 GitHub 上的代码，你还可以直接在桌面应用中监控 Pull Request 的状态。打开 PR 后，Claude Code 会在后台使用 GitHub CLI 追踪其状态，包括 CI 检查的通过和失败。

你还可以启用自动修复，让 Claude 自动尝试修复它检测到的任何 CI 失败。如果启用了自动合并，Claude 还会在所有检查通过后尝试合并 PR。

你可以在一个 Claude Code 会话中处理一个任务并打开 PR，然后转向新任务。在后台，Claude Code 会持续监控原始任务的 PR，并尝试修复 CI 失败，以便在你切换回该任务时，PR 已准备好合并（或已自动合并）。

## **随时续接进度**

会话现在可以随你移动。当你在命令行中使用 Claude Code 启动会话时，运行 /desktop 即可将完整的会话上下文带入桌面应用。

你还可以使用"在网页版 Claude Code 上继续"按钮将本地桌面应用会话迁移到云端。在桌面应用上开始一个任务，然后通过网页版或 Claude 移动应用从手机上继续。

## **开始使用**

这些更新现已面向所有用户开放。更新或下载[Claude Code 桌面版](https://claude.com/download)即可开始使用。查阅[文档](https://code.claude.com/docs/en/desktop)以了解更多。
