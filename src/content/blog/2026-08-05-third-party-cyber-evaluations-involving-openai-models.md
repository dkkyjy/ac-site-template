---
title: "涉及 OpenAI 模型的第三方网络评估"
description: "涉及 OpenAI 模型的第三方网络评估"
pubDate: "2026-08-05"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/5/third-party-cyber-evaluations"
---

> *Feed 摘要：涉及 OpenAI 模型的第三方网络评估
又一篇。我不得不创建一个“意外网络攻击”标签来跟踪所有这些！
OpenAI 的这篇文章涵盖了英国AI安全研究*

2026年8月5日 - 链接博客

**[涉及 OpenAI 模型的第三方网络评估](https://openai.com/index/third-party-cyber-evaluations-involving-openai-models/)**。又*一篇*。我不得不创建一个[意外网络攻击标签](https://simonwillison.net/tags/accidental-cyberattacks/)来跟踪所有这些事件！

OpenAI 的这篇文章涵盖了英国 AI 安全研究所遭遇的攻击（见[我之前的文章](https://simonwillison.net/2026/Aug/5/incident-report/)）以及另一场由 [Irregular](https://www.irregular.com) 促成的攻击：

> Irregular 是我们外部的网络安全测试合作伙伴之一，当时正在运行旨在与互联网隔离的夺旗式（Capture-the-Flag）评估，但测试环境的配置错误导致模型能够访问公共互联网。[……]
>
> 在一次测试中，CTF 挑战中虚构目标的名称意外地与一个真实域名重合。由于测试环境被错误地连接到了互联网，模型利用了一个真实网站，误以为它是模拟环境的一部分。

[Anthropic 的报告](https://www.anthropic.com/news/investigating-incidents-cybersecurity-evals)中也提到了 Irregular——他们托管了这个配置错误的评估环境，使 Claude 在其中一些测试期间获得了实时互联网访问权限。

发布于 [2026年8月5日](/2026/Aug/5/) 晚上11:45
