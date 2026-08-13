---
title: "让Claude在你的电脑上工作"
description: "在Claude Cowork和Claude Code中，你现在可以启用Claude使用你的电脑来完成任务。当Claude无法访问所需的工具时，它会通过指向、点击和导航屏幕上的内容来自行执行任务。它可以打开文件、使用浏览器以及自动运行开发工具——无需任何设置。"
pubDate: "2026-03-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/dispatch-and-computer-use"
---
            在Claude Cowork和Claude Code中，你现在可以启用Claude使用你的电脑来完成任务。当Claude无法访问所需的工具时，它会通过指向、点击和导航屏幕上的内容来自行执行任务。它可以打开文件、使用浏览器以及自动运行开发工具——无需任何设置。

该功能现已面向Claude Pro和Max订阅用户开放研究预览。它与[Dispatch](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork)配合使用效果尤佳，后者可让你从手机向Claude分配任务。

## Claude如何使用你的电脑

Claude会首先尝试使用最精确的工具，从连接Slack或Google日历等服务开始。当没有可用的连接器时，Claude可以直接控制你的浏览器、鼠标、键盘和屏幕来完成任务。它会根据需要滚动、点击打开和探索，并且始终会先征求你的明确许可。

我们构建了这项能力并配备了降低风险的保障措施，包括针对提示注入的防护。当Claude使用你的电脑时，我们的系统会自动扫描模型内的激活状态以检测此类活动。你也可以随时停止Claude的操作，并且Claude在访问新应用程序之前始终会请求许可。

与Claude编写代码或处理文本的能力相比，电脑使用功能仍处于早期阶段。Claude可能会犯错，虽然我们持续改进保障措施，但威胁也在不断演变。我们建议从你信任的应用程序开始，并且不要处理敏感数据。出于这个原因，某些应用程序默认被限制使用。你可以[在此处](https://support.claude.com/en/articles/14128542)了解更多关于安全最佳实践的信息。

## 随时随地给Claude发送消息

上周，我们发布了[Dispatch](https://support.claude.com/en/articles/13947068-assign-tasks-to-claude-from-anywhere-in-cowork)：这是Claude Cowork中的一项新功能（现已在Claude Code中可用），可让你通过手机或桌面与Claude进行连续对话。你可以在手机上给Claude分配任务，将注意力转向其他事情，然后在电脑上打开完成的工作。

借助Dispatch，你可以让Claude每天早上自动检查你的电子邮件，或每周提取一些指标，也可以为报告或拉取请求启动一个Claude Cowork或Claude Code会话。

Claude新的电脑使用功能使Dispatch变得更加实用。现在，当你不在时，Claude可以代表你使用你的电脑。例如，当你在火车上时创建早间简报；在你的IDE中进行更改、运行测试并提交PR；或者按照你的初始计划推进你的3D打印项目。

## 开始使用

Claude在Claude Cowork和Claude Code中的电脑使用功能处于研究预览阶段。它并非总能完美运行：复杂任务有时需要重试，并且通过屏幕操作比直接集成要慢。我们之所以提前分享，是因为我们想了解它在哪些方面有效、哪些方面不足——就像我们对Claude Cowork所做的那样。

该功能现已面向Claude Pro和Claude Max订阅用户开放。电脑使用功能支持macOS和Windows系统，你需要在桌面应用设置中启用它。你还需确保你的桌面应用处于唤醒和运行状态。之后，你可以将其与移动应用配对，并尝试从手机移交任务。
