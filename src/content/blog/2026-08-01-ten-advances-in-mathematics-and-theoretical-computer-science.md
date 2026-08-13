---
title: "数学与理论计算机科学的十项进展"
description: "数学与理论计算机科学的十项进展"
pubDate: "2026-08-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/1/ten-advances-in-mathematics"
---

> *摘要：数学与理论计算机科学的十项进展
几天前，Anthropic 使用 Mythos Preview 版的 Claude 发现加密弱点，花费 10 万美元的 token，并带有*

2026年8月1日 - 链接博客

**[数学与理论计算机科学的十项进展](https://openai.com/index/ten-advances-in-mathematics/)**（[来源](https://news.ycombinator.com/item?id=49132058 "Hacker News")）几天前，Anthropic [使用 Mythos Preview 版的 Claude 发现加密弱点](https://simonwillison.net/2026/Jul/28/discovering-cryptographic-weaknesses-with-claude/)，花费了 10 万美元的 token，并且在提示词中包含了“再说一次，我们不是在寻找唾手可得的成果，我们想要的是真正的研究，以找到真正困难的发现”。

现在轮到 OpenAI 展示实力了。他们让“Astra 的内部版本，也就是我们的下一个主要模型”去解决十个数学问题，这些问题“至少十年来主要结果都没有任何进展”。他们声称，按照 GPT-5.6 Sol 的 token 价格，每个问题花费不到 2000 美元。

（不过，没有消息说他们有多少问题花了 2000 美元*却*没有得出解决方案。）

[openai/ten-proofs](https://github.com/openai/ten-proofs) 存储库包含他们结果的 Lean 4 形式化，还有一篇[论文](https://cdn.openai.com/pdf/ten-proofs-oai.pdf)描述了这些解决方案，以及另外一份[LLM 生成的 PDF](https://cdn.openai.com/pdf/reasoning-walkthroughs.pdf)，其中模型基于未公开的推理轨迹“重构了证明是如何形成的”。

这种透明程度已经不错了，但我想看看他们使用的提示词！

网上许多数学家正经历着一场集体性的[深蓝](https://simonwillison.net/2026/Feb/15/deep-blue/)冲击。数学家柯温·汉普希尔上周发表了一篇慷慨激昂的文章[数学的暗夜](https://kirwinhampshire.substack.com/p/the-dark-night-of-mathematics)，描述了先前（且较不重要的）结果所引发的“深刻的精神危机”。

OpenAI 的结果让我想起了陶哲轩在六月刊的 [IEEE Spectrum](https://spectrum.ieee.org/ai-in-mathematics) 中所描述的“大数学”：

> 与他的一些同行不同，陶哲轩既不轻视 AI，也不害怕它。相反，他认为 AI 是这门学科发生根本性转变的催化剂——一种向他所称的“大数学”的过渡。他设想了一个大规模、去中心化的人机协作未来，复杂的数学任务可以被切分和细分，人类负责创造性的部分，而 AI 承担绝大部分技术性苦活。

发布于 [2026年8月1日](/2026/Aug/1/) 晚上 8:34
