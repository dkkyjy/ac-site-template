---
title: "Claude Cowork 登陆 Chrome 侧边栏 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-12"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/cowork-chrome-side-panel"
---

# Claude Cowork 登陆 Chrome 侧边栏 | Claude by Anthropic

---

[Chrome 中的 Claude](https://claude.com/blog/claude-for-chrome) 侧边栏现已升级为 [Claude Cowork](https://claude.com/product/cowork) 会话。对话将保存至历史记录，技能和连接器可在浏览器中运行，您在某个标签页中启动的任务可以在 Claude 桌面端、网页版和移动端应用上继续完成。该功能现已在 Max 和 Team 套餐上可用，并将在未来几周内逐步向 Pro 用户推出。

Chrome 中的 Claude 是一款浏览器扩展程序，它可以让 Claude 查看您当前所在的页面并在其中执行操作，包括点击链接、输入文本、在页面间导航以及使用您现有的登录凭据填写表单。

您日常使用的许多工具可以[直接连接到 Claude](http://claude.com/connectors)，但另一些则不行，例如内部仪表盘、遗留系统和供应商门户。借助 Chrome 中的 Claude，Claude 可以通过浏览器在这些应用中工作。

在此之前，侧边栏中的会话与 Claude 应用中的会话是相互独立的，因此上下文和对话无法在两者之间传递。现在，侧边栏运行的是与您在桌面端、网页版和移动端使用的同一个 Claude Cowork 会话，适用于更长的多步骤工作。由于会话与您的账户绑定而非与单台设备绑定，您可以在浏览器中开始工作，稍后在其他设备上继续。

举个例子，假设您正在制作一份预算电子表格，需要从多个供应商门户中调取发票。现在，您可以让 Chrome 中的 Claude 收集金额和日期，它会打开相关标签页、阅读每张发票并构建电子表格。然后，您可以在桌面应用中接管该会话，从电脑中导入文件，或导入上个月的预算并询问有哪些变化，从而让您在工作过程中跨平台保持上下文连贯。

## 了解风险

Chrome 中的 Claude 与任何在浏览器中执行操作的 AI 代理面临相同的风险，主要是[提示注入](https://www.anthropic.com/research/prompt-injection-defenses)。恶意行为者会在网页内容中隐藏指令，例如网页、电子邮件或文档。这些指令您可能无法看到，但它们可能将 Claude 引导执行您从未打算的操作。

[自试点以来](https://claude.com/blog/claude-for-chrome)，我们已为 Claude 自身的操作增加了检查机制。使用"自动批准"功能后，Claude 会在执行任务时不再在每一步都暂停等待许可。在执行任何重要操作之前，例如提交表单、发送消息或下载文件，一个独立的检查环节会将该操作与您最初的请求进行比对，并阻止任何不匹配的操作。这样既能减少中断，又能保持监督。

对于某些不可逆或代价高昂的操作，例如进行购买或共享个人数据，Claude 仍会事先征求您的确认。虽然这些措施能显著降低风险，但无法将其完全消除。提示注入是一个不断变化的目标，因此我们持续寻找新的攻击方式，并将所学经验融入我们发布的每一个模型中。我们建议从您信任的网站开始使用，我们的[安全指南](https://support.claude.com/en/articles/12902428-use-claude-in-chrome-safely)中还有更多最佳实践。

## 开始使用

要开始使用 Chrome 中的 Claude，请从[Chrome 网上应用店](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn)安装该扩展程序，登录账户，然后打开侧边栏。新的侧边栏现已在 Max 和 Team 套餐上可用，并将在未来几周内逐步向 Pro 用户推出。在企业版套餐中，Chrome 中的 Claude 默认处于关闭状态。管理员可以将其开启并将其限制在已批准的域名范围内。请参阅[管理员设置指南](https://support.claude.com/en/articles/13065128-claude-in-chrome-admin-controls#h_bdb63199e1)。

您仍需使用 Claude 桌面应用来处理电脑上的文件或其他应用程序。Chrome 中的 Claude 目前尚不支持其他 Chromium 浏览器或移动端。
