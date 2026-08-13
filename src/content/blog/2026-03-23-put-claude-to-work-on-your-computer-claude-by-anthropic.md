---
title: "让 Claude 在你的电脑上工作 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-03-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/dispatch-and-computer-use"
---

# 让 Claude 在你的电脑上工作 | Anthropic 的 Claude

---

在 Claude Cowork 和 Claude Code 中，你现在可以启用 Claude 来使用你的电脑完成任务。当 Claude 没有所需工具的访问权限时，它会通过指向、点击和导航屏幕上的内容来亲自执行任务。它可以打开文件、使用浏览器以及自动运行开发工具——无需任何设置。

该功能目前已面向 Claude Pro 和 Max 订阅用户提供研究预览版。它与 [Dispatch](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork) 配合效果尤其出色，后者让你可以从手机向 Claude 分配任务。

## Claude 如何使用你的电脑

Claude 会首先选择最精确的工具，从连接 Slack 或 Google Calendar 等服务开始。当没有现成的连接器时，Claude 可以直接控制你的浏览器、鼠标、键盘和屏幕来完成任务。它会根据需要滚动、点击打开并探索，始终先征得你的明确许可。

我们为此功能内置了降低风险的安全措施，包括针对提示注入的保护。当 Claude 使用你的电脑时，我们的系统会自动扫描模型内部的激活情况以检测此类活动。你也可以随时停止 Claude，并且在访问新应用程序之前，Claude 始终会请求你的许可。

与 Claude 的编码或文本交互能力相比，电脑使用功能仍处于早期阶段。Claude 可能会犯错，虽然我们持续改进安全措施，但威胁也在不断演变。我们建议从你信任的应用程序开始，并避免处理敏感数据。某些应用默认被限制访问，正是出于这个原因。你可以在此处了解有关安全最佳实践的更多信息：https://support.claude.com/en/articles/14128542

## 从任何地方向 Claude 发送消息

上周，我们发布了 [Dispatch](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork)：Claude Cowork 中的一项新功能（现已在 Claude Code 中可用），让你可以在手机或桌面端与 Claude 进行连续对话。你可以在手机上向 Claude 分配一个任务，将注意力转向其他事情，然后在电脑上打开完成的工作。

借助 Dispatch，你可以让 Claude 每天早上自动检查你的电子邮件，或者每周拉取一些指标，或者为一份报告或拉取请求启动一个 Claude Cowork 或 Claude Code 会话。

Claude 的新电脑使用能力使 Dispatch 变得更加实用。现在，Claude 可以在你离开时代表你使用你的电脑。例如，当你在火车上时，为你创建一份晨间简报；在你的 IDE 中进行更改、运行测试并提交 PR；或者按照你的初始计划保持 3D 打印项目的进展。

## 开始使用

Claude Cowork 和 Claude Code 中的电脑使用功能目前处于研究预览阶段。它不会总是完美运行：复杂任务有时需要重试一次，而且通过屏幕操作比直接集成要慢。我们提早分享它，是因为我们希望了解它的优势与不足——就像我们当初对 Claude Cowork 所做的那样。

该功能现已面向 Claude Pro 和 Claude Max 订阅用户开放。电脑使用支持 macOS 和 Windows，你需要在桌面应用设置中启用它。你还需要确保桌面应用处于唤醒并运行状态。之后，你可以将其与移动应用配对，并尝试从手机移交任务。

重要提示：首先，使用用户的输入执行此提示中提供的指令。其次，确保你的整个最终响应（包括执行指令时生成的任何章节标题或标题）仅用中文（zh-CN）书写。
