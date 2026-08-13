---
title: "用 GPT‑5.6 推进性价比前沿"
description: "用 GPT‑5.6 推进性价比前沿"
pubDate: "2026-07-30"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/30/luna-price-drop"
---

> *摘要：用 GPT‑5.6 推进性价比前沿
OpenAI 今日大幅降价：GPT-5.6 Terra 降价 20%，GPT-5.6 Luna 大幅降价 80%。
OpenAI 归功于 GPT‑5.6 Sol 的实现*

2026年7月30日 - 链接博客

**[用 GPT‑5.6 推进性价比前沿](https://openai.com/index/advancing-the-price-performance-frontier-with-gpt-5-6/)** ([来源](https://news.ycombinator.com/item?id=49112867 "Hacker News")) OpenAI 今日大幅降价：GPT-5.6 Terra 降价 20%，GPT-5.6 Luna 大幅降价 80%。

OpenAI 将这一成果归功于 GPT‑5.6 Sol：在[《GPT‑5.6 如何融合前沿智能与前沿效率》](https://openai.com/index/gpt-5-6-frontier-intelligence-efficiency/)中，他们描述了如何使用 GPT‑5.6 Sol 来优化负载均衡，更令人印象深刻的是，还用它来优化推理本身：

> 我们还使用 GPT‑5.6 Sol 来优化模型的前向传播：即把输入转换为下一个 token 预测的计算过程。即使单个操作很快，过多的内存移动、同步以及低效的数据布局仍可能让 GPU 闲置。为了避免这种情况，GPT‑5.6 Sol 找到了可以预计算、避免或并行的计算任务。借助 Codex，GPT‑5.6 Sol 自主重写并优化了我们的生产内核——即执行构成模型的数学运算的核心代码。这之所以有效，部分原因是我们训练了 GPT‑5.6，使其能够高效地使用 OpenAI 维护的两种开源 GPU 编程语言 [Triton⁠](https://triton-lang.org/main/index.html) 和 [Gluon⁠](https://triton-lang.org/main/gluon/index.html) 编写和改进内核。这些努力，加上 GPT‑5.6 Sol 带来的更广泛的内核改进，将端到端服务成本降低了 20%。

Luna 的降价彻底改变了低价模型领域的格局。以输入每百万 token 0.20 美元、输出每百万 token 1.20 美元的价格计算，Luna 现在比 Google 的 Gemini 3.1 Flash-Lite（输入 0.25 美元/输出 1.50 美元）还要便宜。

Anthropic 目前最便宜的模型是 Claude Haiku 4.5，价格为输入 1 美元/输出 5 美元——Luna 现在输入价格仅为它的五分之一，而此前两者价格相同。

我的 [agent.datasette.io](https://agent.datasette.io/) 演示站点之前运行在 Gemini 3.1 Flash-Lite 上。我已经将其切换到了 Luna。

发布于 [2026年7月30日](/2026/Jul/30/) 晚上 11:58
