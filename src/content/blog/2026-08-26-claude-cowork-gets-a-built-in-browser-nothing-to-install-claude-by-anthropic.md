---
title: "Claude Cowork 新增内置浏览器：无需安装 | Anthropic 旗下 Claude"
description: "---"
pubDate: "2026-08-26"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-built-in-browser"
---

# Claude Cowork 新增内置浏览器：无需安装 | Anthropic 旗下 Claude

---

Claude 现在在桌面应用的 Claude Cowork 中内置了浏览器。当任务需要使用网站时，侧边栏会自动打开一个浏览器，Claude 会浏览网页、阅读内容、点击和输入。现在，你可以将任务中的网页部分交给 Claude，自己继续留在当前位置：Claude 可以填写表单、从仪表板中提取数据，或操作没有连接器的门户系统。无需扩展，无需设置，也不会从你自己的浏览器共享任何内容，除非你主动选择。

迄今为止，要让 Claude 在 Cowork 中使用网络，意味着通过 [Claude in Chrome](http://claude.com/claude-in-chrome) 扩展程序让它访问你的浏览器。当工作对象是你已经打开的页面时，这仍然是最佳选择。但许多网页任务并不需要*你的*浏览器，只需要*一个*浏览器，而现在 Claude 已经有了。

本周起，该功能将面向 Claude 桌面应用的 Pro、Max 和 Team 套餐用户逐步推出。企业管理员从今天起即可为其组织开启此功能。

## 哪个浏览器，何时使用

这是 Claude 的浏览器，不是你的。内置浏览器与你自己的浏览器完全独立。Claude 永远无法访问你的标签页、书签或密码。若要保持网站登录状态，你可以逐站将登录信息从 macOS 上的 Chrome、Edge 或 Firefox，以及 Windows 和 Linux 上的 Firefox 导入。银行、邮件和单点登录网站默认不包含在内，除非你主动选择添加。

这也正是 Claude 使用网络的两种方式之间的区别。内置浏览器适用于在你继续工作的同时将网页任务交给 Claude 的场景：为报告收集研究资料，或从供应商门户中汇总本月的发票。Claude in Chrome 则适用于你已经打开的页面，使用你已登录的账户，例如更新 CRM、处理收件箱，或编辑眼前的文档。

如果你已经在用 Claude in Chrome，它将继续正常工作并保持为你的默认选项；否则 Claude 将使用内置浏览器。你可以随时在"设置 → Cowork → 首选浏览器"中切换。

## 保持掌控

内置浏览器与任何在浏览器中操作的 AI 代理一样，面临相同的[提示注入](https://www.anthropic.com/research/prompt-injection-defenses)风险——即隐藏在页面中的指令试图引导 Claude 偏离方向。它运行与 Claude in Chrome 相同的安全防护机制，包括审查 Claude 的操作是否符合你所要求的检查。我们在 [Claude in Chrome 博客文章](http://claude.com/blog/%20claude-in-chrome-generally-available)中对此做了说明。这些措施能显著降低风险，但无法完全消除，因此我们建议先从你信任的网站开始使用。更多详情请参阅我们的[安全指南](https://support.claude.com/en/articles/12902428-use-claude-in-chrome-safely)。

## 开始使用

内置浏览器将在未来一周内面向 Claude 桌面应用（macOS、Windows 和 Linux，Linux 为测试版）的 Pro、Max 和 Team 套餐用户逐步推出。一旦该功能到达你的账户，它将默认开启：给 Claude 一个涉及网站的任务，浏览器就会自动打开。对于 Enterprise 套餐，该功能现已可用，管理员可在"组织设置 → Cowork → 内置浏览器"中进行管理。

内置浏览器运行在桌面应用中。通过网页或手机，只要你的桌面应用保持打开且在线，Claude 仍然可以操控它。在没有桌面应用的网页端，Claude in Chrome 仍然是为 Claude 提供浏览器的唯一方式。
