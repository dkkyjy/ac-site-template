---
title: "理解 ChatGPT Work"
description: "理解 ChatGPT Work"
pubDate: "2026-08-30"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/30/understanding-chatgpt-work"
---

> *摘要：OpenAI 于 7 月 9 日发布了 ChatGPT Work，此后一直在疯狂迭代。这是一款极其令人困惑却又极其强大的产品。以下是我目前了解到的情况。*

## 理解 ChatGPT Work

2026 年 8 月 30 日

OpenAI 于 7 月 9 日[发布了 ChatGPT Work](https://openai.com/index/chatgpt-for-your-most-ambitious-work/)，此后一直在疯狂迭代。这是一款极其令人困惑却又极其强大的产品。以下是我目前了解到的情况。

#### ChatGPT Work 实际上是两款产品

更有趣的版本是在云端运行的那个。它可以通过 [chatgpt.com](https://www.chatgpt.com/) 或 ChatGPT 移动应用访问。我们叫它 **Work Cloud（云端版）**。

如果你安装了 ChatGPT 桌面应用——就是那个以前叫 Codex 的应用——你就能使用一个叫 ChatGPT Work 的功能，它可以访问你电脑上的文件并直接运行程序。我们叫它 **Work Local（本地版）**。这个版本感觉更像是换了层皮的普通 Codex，目的是让非软件开发者不那么望而生畏。

在本文的其余部分，我只讨论 Work Cloud。

#### Work 仅限付费用户

目前，ChatGPT Work（两个版本）仅对每月 20 美元及以上的订阅用户开放。免费用户和每月 8 美元的 Go 用户无法使用。

#### Work 拥有 Chat 中没有的功能

访问 Work 的界面是一个选项卡选择器，它将自己呈现为 Chat 的一个替代方案：

一个显而易见的问题是：*什么时候该用 Chat，什么时候该用 Work？*

OpenAI 给出的[官方回答](https://learn.chatgpt.com/docs/get-started-with-work)是：

> 当你需要答案、解释、头脑风暴或简短草稿时，使用 Chat。当你希望 ChatGPT 完成一项有明确产出的任务时，使用 ChatGPT Work，例如简报、演示文稿、分析、定期更新、工作流，或者你可以审查和使用的文件。

我觉得这个回答几乎毫无用处，因为我多年来一直在用普通的 ChatGPT Chat 完成所有这些类型的任务！

那么更好的问题应该是：*Work 有哪些 Chat 中缺失的功能？*

经过大量实验，我认为我基本搞清楚了：

* [可以用 Luna 和 Terra 替代 Sol 的选项](#model-selection)
* [一个可以访问互联网的代码执行环境](#code-execution-with-internet-access-)
* [一个无头 Chrome 浏览器](#a-full-headless-chrome-browser)
* [一个在会话之间共享的持久化文件系统](#a-persistent-shared-filesystem)
* [发布 ChatGPT 站点的能力](#chatgpt-sites)
* [使用 Sol、Luna 和 Terra 运行子代理会话的能力](#sub-agents-with-sol-luna-and-terra)
* [定时提示词自动化](#scheduled-prompt-automations)（可能 ChatGPT Chat 也有）

#### 模型选择

在 Work 中，你可以选择 GPT-5.6 Sol、Luna 或 Terra，每个都有 Light（轻量）、Medium（中档）、High（高档）、Extra High（特高档）、Max（最高档）或 Ultra（超档）推理级别。你还可以选择 GPT-5.5 的 Light、Medium、High 或 Extra High 级别。

这些看起来就是通过 OpenAI API 可用的同一批模型。

Chat 提供的选择则不同：5.6 Instant、Medium、High、Extra High 和 Pro（实际上 Extra High 和 Pro 仅对每月 100 美元及以上的订阅用户开放——每月 20 美元的订阅用户最高只能用到 High）。它没有说明这些是 Luna、Terra 还是 Sol（我猜是 Sol？）。5.6 Pro 似乎是 Chat 独有的，Work 中没有对应版本。

根据我使用 Codex 的目前的理解，Ultra 是一种特殊模式，它会更积极地委托任务给子代理。

我认为 ChatGPT Work 会话的计费是从你的 Codex 配额中扣除的，而 ChatGPT Chat 会话则有自己独立的配额。这可能有助于解释两者在模型可用性上的差异。

#### 可以访问互联网的代码执行！

作为一个[代码解释器模式](https://simonwillison.net/tags/code-interpreter/)的长期爱好者——这一模式由 OpenAI 于 2023 年开创——这是 ChatGPT Work（云端版）中让我最兴奋的功能，没有之一。

代码执行环境现在可以访问整个互联网了！

ChatGPT Chat 做不到这一点——如果你让它安装额外的软件包，或者与需要访问的网站或 API 交互，容器代理会阻止这些访问。

（奇怪的是，今年一月份它[曾经获得了安装软件包的能力](https://simonwillison.net/2026/Jan/26/chatgpt-containers/)，但似乎现在又不行了。真希望他们能提供更好的更新日志！）

Claude 的等效容器自去年九月[上线以来](https://simonwillison.net/2025/Sep/9/claude-code-interpreter/)就允许受限的互联网访问。Claude 可以从 PYPI 和 NPM 安装软件包，也可以从 GitHub 克隆仓库。但仅此而已：允许访问的域名白名单非常短。

ChatGPT Work 允许的范围要大得多。它可以配置一个特定的允许域名列表，但默认设置似乎是对所有域名开放的。

这使得 Work 成为一个极其有用的工具。你可以让它克隆 GitHub 仓库，安装其依赖项，然后用它们与整个互联网进行交互！

#### 完整的无头 Chrome 浏览器

ChatGPT Work 的另一个杀手级功能是[浏览器工具](https://learn.chatgpt.com/docs/browser?surface=web)。ChatGPT Work 可以启动一个完整的 Chrome 实例，加载网站、填写表单并截图。

如果网站需要登录，浏览器可以提示你接管操作，输入密码和双因素认证验证码，而无需将这些凭据来回传递给模型本身。

它甚至可以在已加载页面的 DOM 上运行 JavaScript。我这样提示：

> `在你的浏览器中加载 simonwillison.net，然后用 JavaScript 提取所有标题`

ChatGPT Work 启动了一个浏览器实例并运行了以下代码：

```
await tab.playwright.evaluate(() => {
  return Array.from(document.querySelectorAll("h1,h2,h3,h4,h5,h6"), heading => ({
    level: heading.tagName.toLowerCase(),
    text: heading.innerText.trim().replace(/\s+/g, " "),
    id: heading.id || null
   }));
});
```

这感觉非常像我的 [shot-scraper javascript](https://shot-scraper.datasette.io/en/stable/javascript.html) 工具，只不过现在我在手机上就能用了！

#### 持久化、共享的文件系统

ChatGPT Chat 的每个聊天会话都会获得一个全新的文件系统，这些文件无法从其他会话中访问。

在 ChatGPT Work 中，每个会话都有自己的临时文件夹——名字类似 `/workspace/scratch/e00a0a017944`——但每个文件夹都会跨会话持久保存，因此你可以访问之前聊天中的文件。我光 `/workspace/scratch` 目录下就有 171 个文件夹了！

据我所知，`/workspace` 卷挂载到了所有当前正在运行的 Work 会话上——一个会话中的文件编辑可以立即被其他会话看到。但它们似乎不共享同一个进程空间，在一个会话中运行的本地服务器无法从另一个会话中访问。

#### ChatGPT 站点

ChatGPT Work 能够构建*并部署*完整的网站，使用 Cloudflare Workers。这些站点可以包含 HTML 和 JavaScript，还能运行服务端功能，包括基于 Cloudflare D1 和 R2 的有状态功能。

这是我用这个功能搭建的一个简单站点：

[london-pelicans-in-her-piety.simonw.chatgpt.site](https://london-pelicans-in-her-piety.simonw.chatgpt.site/)

我的提示词是：

> `找出伦敦所有与"鹈鹕护雏"相关的地点，然后将其整理成一个 JSON 文件，并围绕这些地点搭建一个 ChatGPT 站点`

（"鹈鹕护雏"是一种引人入胜的[中世纪基督教意象](https://devonchurchland.co.uk/blog/pelican-in-her-piety/#What-is-a-Pelican-In-Her-Piety)——一旦你知道了它，你会发现它无处不在。）

这些站点默认仅创建者本人可见，但你可以将其设为公开，并且（在团队版计划中）可以与特定的其他用户共享。

#### 使用 Sol、Luna 和 Terra 的子代理

关于这一点没什么好说的。ChatGPT Chat 无法运行子代理，而 ChatGPT Work 可以。这显然是一个高级用户功能：如果你正在运行一个复杂项目，并且可以从多个并行代理协作中获益，Work 可以做到这一点。

#### 定时提示词自动化

另一个似乎从普通 ChatGPT 迁移到了 ChatGPT Work 的功能。你可以这样提示 ChatGPT Work：

> `每天上午 8 点搜索一次，看看 Waymo 是否已经公布了半月湾（Half Moon Bay）的上线日期`

这会将一个提示词按该频率安排定时执行。这些提示词可以判断没有值得关注的新动态，也可以决定通知你一些新信息。

**更新**：实际上这个功能在 ChatGPT Chat 中似乎也有效。

不过在这里提一下仍然值得，因为它可以与其他 ChatGPT Work 专属功能配合使用。例如，你可以设置一个定时任务，每小时更新一次一个 ChatGPT 站点。

#### 这安全吗？

目前对我来说一个悬而未决的问题是，这一切到底有多*安全*。

我的[致命三要素模型](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)警示了任何代理系统固有的风险——当该系统同时具备访问私有数据的能力、暴露于不可信内容的风险，以及一种将窃取信息回传给攻击者的途径时。

ChatGPT Work 三者兼备！

我非常希望 OpenAI 能更多地谈谈他们如何保护 ChatGPT Work 会话免受提示注入攻击。我预计他们的答案和 Codex 的[自动审查机制](https://learn.chatgpt.com/docs/sandboxing/auto-review)一样。

#### OpenAI 本可以让这一切没那么令人困惑

搞清楚这一切花了我多得多的精力，本不该如此。

我认为这里有两个关键问题：

1. OpenAI 用"用途"来解释 Work，而不是用"它实际能做什么"来解释
2. OpenAI 仍然坚持隐藏他们的系统提示词和工具描述

如果 ChatGPT Work 的文档中包含了代理所使用的确切系统提示词和工具描述，我就不需要写这篇文章了。

发布于 [2026 年 8 月 30 日](/2026/Aug/30/) 晚上 11:59 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
</think>

# 理解 ChatGPT Work

         **日期：** 2026-08-30 23:59 UTC
         **链接：** https://simonwillison.net/2026/Aug/30/understanding-chatgpt-work/
         **标签：** ai, openai, generative-ai, chatgpt, llms, code-interpreter, lethal-trifecta, general-agents

         ---

         > *摘要：OpenAI 于 7 月 9 日发布了 ChatGPT Work，此后一直在疯狂迭代。这是一款极其令人困惑却又极其强大的产品。以下是我目前了解到的情况。*

## 理解 ChatGPT Work

2026 年 8 月 30 日

OpenAI 于 7 月 9 日[发布了 ChatGPT Work](https://openai.com/index/chatgpt-for-your-most-ambitious-work/)，此后一直在疯狂迭代。这是一款极其令人困惑却又极其强大的产品。以下是我目前了解到的情况。

#### ChatGPT Work 实际上是两款产品

更有趣的版本是在云端运行的那个。它可以通过 [chatgpt.com](https://www.chatgpt.com/) 或 ChatGPT 移动应用访问。我们叫它 **Work Cloud（云端版）**。

如果你安装了 ChatGPT 桌面应用——就是那个以前叫 Codex 的应用——你就能使用一个叫 ChatGPT Work 的功能，它可以访问你电脑上的文件并直接运行程序。我们叫它 **Work Local（本地版）**。这个版本感觉更像是换了层皮的普通 Codex，目的是让非软件开发者不那么望而生畏。

在本文的其余部分，我只讨论 Work Cloud。

#### Work 仅限付费用户

目前，ChatGPT Work（两个版本）仅对每月 20 美元及以上的订阅用户开放。免费用户和每月 8 美元的 Go 用户无法使用。

#### Work 拥有 Chat 中没有的功能

访问 Work 的界面是一个选项卡选择器，它将自己呈现为 Chat 的一个替代方案：

一个显而易见的问题是：*什么时候该用 Chat，什么时候该用 Work？*

OpenAI 给出的[官方回答](https://learn.chatgpt.com/docs/get-started-with-work)是：

> 当你需要答案、解释、头脑风暴或简短草稿时，使用 Chat。当你希望 ChatGPT 完成一项有明确产出的任务时，使用 ChatGPT Work，例如简报、演示文稿、分析、定期更新、工作流，或者你可以审查和使用的文件。

我觉得这个回答几乎毫无用处，因为我多年来一直在用普通的 ChatGPT Chat 完成所有这些类型的任务！

那么更好的问题应该是：*Work 有哪些 Chat 中缺失的功能？*

经过大量实验，我认为我基本搞清楚了：

* [可以用 Luna 和 Terra 替代 Sol 的选项](#model-selection)
* [一个可以访问互联网的代码执行环境](#code-execution-with-internet-access-)
* [一个无头 Chrome 浏览器](#a-full-headless-chrome-browser)
* [一个在会话之间共享的持久化文件系统](#a-persistent-shared-filesystem)
* [发布 ChatGPT 站点的能力](#chatgpt-sites)
* [使用 Sol、Luna 和 Terra 运行子代理会话的能力](#sub-agents-with-sol-luna-and-terra)
* [定时提示词自动化](#scheduled-prompt-automations)（可能 ChatGPT Chat 也有）

#### 模型选择

在 Work 中，你可以选择 GPT-5.6 Sol、Luna 或 Terra，每个都有 Light（轻量）、Medium（中档）、High（高档）、Extra High（特高档）、Max（最高档）或 Ultra（超档）推理级别。你还可以选择 GPT-5.5 的 Light、Medium、High 或 Extra High 级别。

这些看起来就是通过 OpenAI API 可用的同一批模型。

Chat 提供的选择则不同：5.6 Instant、Medium、High、Extra High 和 Pro（实际上 Extra High 和 Pro 仅对每月 100 美元及以上的订阅用户开放——每月 20 美元的订阅用户最高只能用到 High）。它没有说明这些是 Luna、Terra 还是 Sol（我猜是 Sol？）。5.6 Pro 似乎是 Chat 独有的，Work 中没有对应版本。

根据我使用 Codex 的目前的理解，Ultra 是一种特殊模式，它会更积极地委托任务给子代理。

我认为 ChatGPT Work 会话的计费是从你的 Codex 配额中扣除的，而 ChatGPT Chat 会话则有自己独立的配额。这可能有助于解释两者在模型可用性上的差异。

#### 可以访问互联网的代码执行！

作为一个[代码解释器模式](https://simonwillison.net/tags/code-interpreter/)的长期爱好者——这一模式由 OpenAI 于 2023 年开创——这是 ChatGPT Work（云端版）中让我最兴奋的功能，没有之一。

代码执行环境现在可以访问整个互联网了！

ChatGPT Chat 做不到这一点——如果你让它安装额外的软件包，或者与需要访问的网站或 API 交互，容器代理会阻止这些访问。

（奇怪的是，今年一月份它[曾经获得了安装软件包的能力](https://simonwillison.net/2026/Jan/26/chatgpt-containers/)，但似乎现在又不行了。真希望他们能提供更好的更新日志！）

Claude 的等效容器自去年九月[上线以来](https://simonwillison.net/2025/Sep/9/claude-code-interpreter/)就允许受限的互联网访问。Claude 可以从 PYPI 和 NPM 安装软件包，也可以从 GitHub 克隆仓库。但仅此而已：允许访问的域名白名单非常短。

ChatGPT Work 允许的范围要大得多。它可以配置一个特定的允许域名列表，但默认设置似乎是对所有域名开放的。

这使得 Work 成为一个极其有用的工具。你可以让它克隆 GitHub 仓库，安装其依赖项，然后用它们与整个互联网进行交互！

#### 完整的无头 Chrome 浏览器

ChatGPT Work 的另一个杀手级功能是[浏览器工具](https://learn.chatgpt.com/docs/browser?surface=web)。ChatGPT Work 可以启动一个完整的 Chrome 实例，加载网站、填写表单并截图。

如果网站需要登录，浏览器可以提示你接管操作，输入密码和双因素认证验证码，而无需将这些凭据来回传递给模型本身。

它甚至可以在已加载页面的 DOM 上运行 JavaScript。我这样提示：

> `在你的浏览器中加载 simonwillison.net，然后用 JavaScript 提取所有标题`

ChatGPT Work 启动了一个浏览器实例并运行了以下代码：

```
await tab.playwright.evaluate(() => {
  return Array.from(document.querySelectorAll("h1,h2,h3,h4,h5,h6"), heading => ({
    level: heading.tagName.toLowerCase(),
    text: heading.innerText.trim().replace(/\s+/g, " "),
    id: heading.id || null
   }));
});
```

这感觉非常像我的 [shot-scraper javascript](https://shot-scraper.datasette.io/en/stable/javascript.html) 工具，只不过现在我在手机上就能用了！

#### 持久化、共享的文件系统

ChatGPT Chat 的每个聊天会话都会获得一个全新的文件系统，这些文件无法从其他会话中访问。

在 ChatGPT Work 中，每个会话都有自己的临时文件夹——名字类似 `/workspace/scratch/e00a0a017944`——但每个文件夹都会跨会话持久保存，因此你可以访问之前聊天中的文件。我光 `/workspace/scratch` 目录下就有 171 个文件夹了！

据我所知，`/workspace` 卷挂载到了所有当前正在运行的 Work 会话上——一个会话中的文件编辑可以立即被其他会话看到。但它们似乎不共享同一个进程空间，在一个会话中运行的本地服务器无法从另一个会话中访问。

#### ChatGPT 站点

ChatGPT Work 能够构建*并部署*完整的网站，使用 Cloudflare Workers。这些站点可以包含 HTML 和 JavaScript，还能运行服务端功能，包括基于 Cloudflare D1 和 R2 的有状态功能。

这是我用这个功能搭建的一个简单站点：

[london-pelicans-in-her-piety.simonw.chatgpt.site](https://london-pelicans-in-her-piety.simonw.chatgpt.site/)

我的提示词是：

> `找出伦敦所有与"鹈鹕护雏"相关的地点，然后将其整理成一个 JSON 文件，并围绕这些地点搭建一个 ChatGPT 站点`

（"鹈鹕护雏"是一种引人入胜的[中世纪基督教意象](https://devonchurchland.co.uk/blog/pelican-in-her-piety/#What-is-a-Pelican-In-Her-Piety)——一旦你知道了它，你会发现它无处不在。）

这些站点默认仅创建者本人可见，但你可以将其设为公开，并且（在团队版计划中）可以与特定的其他用户共享。

#### 使用 Sol、Luna 和 Terra 的子代理

关于这一点没什么好说的。ChatGPT Chat 无法运行子代理，而 ChatGPT Work 可以。这显然是一个高级用户功能：如果你正在运行一个复杂项目，并且可以从多个并行代理协作中获益，Work 可以做到这一点。

#### 定时提示词自动化

另一个似乎从普通 ChatGPT 迁移到了 ChatGPT Work 的功能。你可以这样提示 ChatGPT Work：

> `每天上午 8 点搜索一次，看看 Waymo 是否已经公布了半月湾（Half Moon Bay）的上线日期`

这会将一个提示词按该频率安排定时执行。这些提示词可以判断没有值得关注的新动态，也可以决定通知你一些新信息。

**更新**：实际上这个功能在 ChatGPT Chat 中似乎也有效。

不过在这里提一下仍然值得，因为它可以与其他 ChatGPT Work 专属功能配合使用。例如，你可以设置一个定时任务，每小时更新一次一个 ChatGPT 站点。

#### 这安全吗？

目前对我来说一个悬而未决的问题是，这一切到底有多*安全*。

我的[致命三要素模型](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)警示了任何代理系统固有的风险——当该系统同时具备访问私有数据的能力、暴露于不可信内容的风险，以及一种将窃取信息回传给攻击者的途径时。

ChatGPT Work 三者兼备！

我非常希望 OpenAI 能更多地谈谈他们如何保护 ChatGPT Work 会话免受提示注入攻击。我预计他们的答案和 Codex 的[自动审查机制](https://learn.chatgpt.com/docs/sandboxing/auto-review)一样。

#### OpenAI 本可以让这一切没那么令人困惑

搞清楚这一切花了我多得多的精力，本不该如此。

我认为这里有两个关键问题：

1. OpenAI 用"用途"来解释 Work，而不是用"它实际能做什么"来解释
2. OpenAI 仍然坚持隐藏他们的系统提示词和工具描述

如果 ChatGPT Work 的文档中包含了代理所使用的确切系统提示词和工具描述，我就不需要写这篇文章了。

发布于 [2026 年 8 月 30 日](/2026/Aug/30/) 晚上 11:59 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
