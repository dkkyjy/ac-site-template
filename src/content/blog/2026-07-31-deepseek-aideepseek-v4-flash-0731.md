---
title: "deepseek-ai/DeepSeek-V4-Flash-0731"
description: "deepseek-ai/DeepSeek-V4-Flash-0731"
pubDate: "2026-07-31"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/31/deepseek-v4-flash-0731"
---

> *订阅源摘要：deepseek-ai/DeepSeek-V4-Flash-0731
        DeepSeek V4 系列的最新版本，“具有大幅增强的智能体能力”。它有 3040 亿参数——在 Hugging Face 上为 167GB——但它似乎*

2026年7月31日 - 链接博客

**[deepseek-ai/DeepSeek-V4-Flash-0731](https://huggingface.co/deepseek-ai/DeepSeek-V4-Flash-0731)**（[来源](https://news.ycombinator.com/item?id=49120299 "Hacker News")）DeepSeek V4 系列的最新版本，“具有大幅增强的智能体能力”。它有 3040 亿参数——在 Hugging Face 上为 167GB——但它的表现似乎*远超*其体量。

Artificial Analysis [将其排名](https://artificialanalysis.ai/models/deepseek-v4-flash)在 MiniMax M3——一个 428B 模型——之前。其 $0.14/百万输入和 $0.27/百万输出的定价意味着这可能是目前市场上性价比最高的模型。它在[智能指数 vs. 每智能指数任务成本](https://artificialanalysis.ai/models/deepseek-v4-flash#intelligence-comparison-tabs)图表上表现非常好：

我通过 OpenRouter 使用默认推理级别，得到了[一只令人失望的鹈鹕](https://gist.github.com/simonw/83bfb1171792f1e7a4d8935b5e82317e#prompt)：

但当我把推理级别调高到高时，我得到了[好得多的东西](https://gist.github.com/simonw/83bfb1171792f1e7a4d8935b5e82317e#options)：

`llm -m openrouter/deepseek/deepseek-v4-flash-0731 -t pelican -o reasoning_effort high`

发布于 [2026年7月31日](/2026/Jul/31/) 晚上 11:59
