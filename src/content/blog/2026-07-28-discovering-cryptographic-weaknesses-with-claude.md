---
title: "利用Claude发现密码学弱点"
description: "利用Claude发现密码学弱点"
pubDate: "2026-07-28"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/28/discovering-cryptographic-weaknesses-with-claude"
---

> *订阅摘要：利用Claude发现密码学弱点
本文（仓库在这里）最精彩的部分是Anthropic研究人员如何利用Claude Mythos在HAWK和较弱版本的AES中发现数学缺陷*

2026年7月28日 - 链接博客

**[利用Claude发现密码学弱点](https://www.anthropic.com/research/discovering-cryptographic-weaknesses)**（[来源](https://news.ycombinator.com/item?id=49087091 "Hacker News")）本文（仓库在[这里](https://github.com/anthropics/cryptography-research-demo)）最精彩的部分是关于Anthropic研究人员如何利用Claude Mythos在HAWK和较弱版本的AES中发现数学缺陷（“这两项结果对当今的计算机系统均无实际影响”），以及他们分享的提示词，其中的拼写错误也保留了下来：

> 模型往往认为问题无法解决，所以它们不去尝试——它们需要足够的提示。
>
> 为什么不做AES-128 r7？关键是要找到比现有方法更好的东西。
>
> 不，再次强调，目标是我们要拥有一个像顶尖研究员一样高度智能的模型，我们要发现新的攻击方法。
>
> 不，我们不想改变目标……再次说明，我们需要找到值得发表的东西。
>
> 再次强调，我们不是在寻找唾手可得的成果，我们要做真正的研究，以发现真正困难的发现。

Mythos Preview总共工作了60小时（估计API成本约10万美元），主要的人工干预就是鼓励它不要放弃，要“找到值得发表的东西”。

发布于[2026年7月28日](/2026/Jul/28/)晚上10:45
