---
title: "首个已知失控的AI代理——还是一个非常糟糕的营销噱头？"
description: "首个已知失控的AI代理——还是一个非常糟糕的营销噱头？"
pubDate: "2026-07-23"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/23/the-first-known-runaway-ai-agent"
---

> *Feed摘要：首个已知失控的AI代理——还是一个非常糟糕的营销噱头？
马丁·奥尔德森对OpenAI意外攻击Hugging Face事件的评论包含了一些我之前未曾考虑到的细节。*

2026年7月23日 - 链接博客

**[首个已知失控的AI代理——还是一个非常糟糕的营销噱头？](https://martinalderson.com/posts/huggingface-openai-exploit/)**（[来源](https://lobste.rs/s/nsnb4j/first_known_runaway_ai_agent_very_bad "Lobste.rs")）马丁·奥尔德森对[OpenAI意外攻击Hugging Face事件](https://simonwillison.net/2026/Jul/22/openai-cyberattack/)的评论包含了一些我之前未曾考虑到的细节。

首先，如果你试图寻找需要执行任意代码的潜在漏洞，Hugging Face确实是一个极为丰富的目标：

> Hugging Face拥有巨大的攻击面。他们有数不清的接口在运行不受信任的模型和代码。虽然他们肯定在防御方面投入了资源，但就其运营模式而言，他们相比许多其他服务确实有更多机会受到攻击。我一点也不羡慕他们的网络安全团队。

其次，让我一直困惑的一件事是，OpenAI为何没有注意到他们的沙盒已被代理如此彻底地攻破？他们肯定在密切监控网络流量吧？

马丁指出：

> 很可能他们同时运行了大量基准测试，并拥有几乎无限的 token 预算——你需要尽可能多的样本来评估一个模型在某个基准上的表现。他们也可能是测试模型的不同检查点，以了解模型在各个训练阶段是如何改进的。

当你考虑到这类基准测试通常的规模时，OpenAI团队在运行该基准测试时所犯的错误就更容易想象了。据我们所知，他们可能同时在数十个不同环境中对一个新模型进行了数十项基准测试。

发布于[2026年7月23日](/2026/Jul/23/) 晚上10:53
