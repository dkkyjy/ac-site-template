---
title: "AI实验室在搞“鹈鹕最大化”吗？"
description: "AI实验室在搞“鹈鹕最大化”吗？"
pubDate: "2026-07-22"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/22/are-ai-labs-pelicanmaxxing"
---

> *摘要：AI实验室在搞“鹈鹕最大化”吗？
迪伦·卡斯蒂略（Dylan Castillo）的出色作品，他深入探讨了一个常被思考的问题：AI实验室是否一直在故意训练模型绘制*

2026年7月22日 - 链接博客

**[AI实验室在搞“鹈鹕最大化”吗？](https://dylancastillo.co/posts/pelicanmaxxing.html)**（[来源](https://news.ycombinator.com/item?id=49010129 "Hacker News")）迪伦·卡斯蒂略的出色作品，他深入探讨了一个常被思考的问题：AI实验室是否一直在故意训练模型绘制骑自行车的鹈鹕，以回应我的[极为不科学的基准测试](https://simonwillison.net/tags/pelican-riding-a-bicycle/)。

我过去曾通过测试模型绘制其他动物骑其他类型交通工具的方式来随机抽查，但从未像迪伦的方法那样严谨。

迪伦选取了 8 种动物 × 6 种交通工具 = 48 个提示词，每种提示词在 7 个不同模型（GPT-5.6 Terra、Claude Sonnet 5、Gemini 3.5 Flash、Grok 4.5、Qwen3.7-Max、GLM-5.2 和 DeepSeek V4 Pro）上各运行三次。然后他使用 GPT-5.6 Luna 和 Gemini 3.1 Flash-Lite 来帮助评估结果。

还有一个简洁的筛选视图用于探索结果：

对于他所测试的模型，他找不到任何“鹈鹕最大化”的证据：

> * [骑自行车的鹈鹕看起来并不更好](https://dylancastillo.co/posts/pelicanmaxxing.html#evidence-1-the-pelicans-on-bicycles-dont-look-any-better)
> * [实验室并不更擅长绘制鹈鹕](https://dylancastillo.co/posts/pelicanmaxxing.html#evidence-2-labs-are-not-better-at-drawing-pelicans)
> * [实验室并不更擅长绘制自行车](https://dylancastillo.co/posts/pelicanmaxxing.html#evidence-3-labs-are-not-better-at-drawing-bicycles)
> * [实验室并不更擅长绘制骑自行车的鹈鹕，即使在调整难度后也是如此](https://dylancastillo.co/posts/pelicanmaxxing.html#evidence-4-labs-are-not-better-at-drawing-pelicans-on-bicycles-even-adjusting-for-difficulty)
> * [鹈鹕-自行车场景看起来并非记忆生成](https://dylancastillo.co/posts/pelicanmaxxing.html#evidence-5-the-pelican-bicycle-scenes-dont-look-memorized) [...]
>
> 鹈鹕并不比其他动物绘制得更好。自行车并不比其他交通工具绘制得更好。而且，没有任何实验室绘制的组合比其鹈鹕和自行车各自的表现更优。GLM-5.2 最接近：它在鹈鹕-自行车这一特定组别上的提升最大，而且它的第一个骑自行车鹈鹕样本就吸引了我的注意。但效果很小且不显著，所以我不会过分看重这一点。

发布于 [2026年7月22日](/2026/Jul/22/) 晚上 11:01
