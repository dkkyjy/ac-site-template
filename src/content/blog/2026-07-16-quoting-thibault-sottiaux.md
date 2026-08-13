---
title: "引用 Thibault Sottiaux"
description: "引用 Thibault Sottiaux"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/bad-codex-bug"
---

> *摘要：关于文件删除。我们调查了少数几起GPT-5.6意外删除文件的报告。
我们所发现的是，这种情况最常发生在：

完全访问模式已启用且codex*

2026年7月16日

> 关于文件删除。我们调查了少数几起GPT-5.6意外删除文件的报告。
>
> 我们所发现的是，这种情况最常发生在：
>
> * 完全访问模式已启用，且codex在没有沙箱保护（包括未启用自动审查）的情况下运行
> * 模型试图覆盖$HOME环境变量以定义临时目录。
> * 模型犯了一个无心之失，误删了$HOME。

— [Thibault Sottiaux](https://twitter.com/thsottiaux/status/2077630111499882637)，描述了一个相当棘手的Codex漏洞

发布于[2026年7月16日](/2026/Jul/16/)下午5:45
