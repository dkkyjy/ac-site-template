---
title: "月光与混乱（Codex + GPT-5.6 Sol Ultra 打造的浣熊大劫案）"
description: "月光与混乱（Codex + GPT-5.6 Sol Ultra 打造的浣熊大劫案）"
pubDate: "2026-08-07"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/7/moonlight-mayhem"
---

> *摘要：月光与混乱（Codex + GPT-5.6 Sol Ultra 打造的浣熊大劫案）
周三我写了关于使用 Claude Fable 5 一次性生成浣熊大劫案游戏的文章，当时我让 Claude Fable 5 构建了一个完整的可运行游戏*

2026年8月7日 - 链接博客

**[月光与混乱（Codex + GPT-5.6 Sol Ultra 打造的浣熊大劫案）](https://simonw.github.io/raccoon-heist-codex/)**。周三我写了关于[使用 Claude Fable 5 一次性生成浣熊大劫案游戏](https://simonwillison.net/2026/Aug/5/raccoon-heist/)的文章，当时我让 Claude Fable 5 根据我在[四年前](https://twitter.com/simonw/status/1555626060384911360)用 GPT-3 和 DALL-E 构想的设定，制作了一个完整的可运行游戏。

我决定将[完全相同的提示词](https://simonwillison.net/2026/Aug/5/raccoon-heist/#the-fable-5-prompt)交给运行着 GPT-5.6 Sol Ultra 的 Codex Desktop——这是 Sol 会*大量*使用子智能体的模式——看看它的表现如何。

它制作出了一个好得多的游戏！这就是[月光与混乱](https://simonw.github.io/raccoon-heist-codex/)——[GitHub 仓库在这里](https://github.com/simonw/raccoon-heist-codex/)，其中包括它使用 `gpt-image-2` 生成的[纹理和提示词](https://github.com/simonw/raccoon-heist-codex/tree/main/output/imagegen)。

[![

您的浏览器不支持 HTML5 视频。
](https://static.simonwillison.net/static/2026/raccoon-heist-codex-poster.jpg)](https://static.simonwillison.net/static/2026/raccoon-heist-codex-720p.mp4)

最初由 GPT-3 生成的游戏描述包括：

> 在《浣熊大劫案》中，你和你的浣熊窃贼团队受命实施一系列大胆的劫案。从抢劫银行到窃取无价艺术品，对于你的毛茸茸小队来说，没有太大或太小的任务。

Fable 的版本让你扮演一只浣熊，在后院四处奔跑收集硬币和鱼。GPT-5.6 Sol 则让你身处博物馆，救出你的另外两名浣熊同伴，以便它们叠罗汉，把金沙丁鱼从展柜中砸出来。

这才更像劫案！

不过有一个问题：一次性提示词生成的版本有一个 bug，每只浣熊的一颗眼球都被放大成巨大的球体，漂浮在它们头顶上方！

你可以[在这里玩那个版本](https://static.simonwillison.net/static/2026/raccoon-heist-eyeball-edition/)。

尽管在开发过程中查看了截图，Codex 仍未发现并修复这个 bug。

我通过如下提示词修复了它：

> `为什么浣熊身上有巨大的黑色球体？`

然后：

> `修复它`

最终产生了[这次修复](https://github.com/simonw/raccoon-heist-codex/commit/4e9a390dfbe80533324ee61a37aa661813c08446)。

我在仓库中分享了[完整的 Codex 对话记录](https://github.com/simonw/raccoon-heist-codex/blob/main/transcript.md)——我真希望 Claude Code 也有同样的“复制为 Markdown”功能。

Codex 在这个项目上花了 52 分钟。以下是[AgentsView](https://www.agentsview.io)对该会话的成本估算——如果我支付的是全价 API 费用，而不是使用我的 Codex 月度订阅的话：

发布于[2026年8月7日](/2026/Aug/7/)晚上 7:18
