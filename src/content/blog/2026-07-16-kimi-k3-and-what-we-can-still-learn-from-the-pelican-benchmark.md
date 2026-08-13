---
title: "Kimi K3，以及我们仍可以从鹈鹕基准测试中学到什么"
description: "Kimi K3，以及我们仍可以从鹈鹕基准测试中学到什么"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/kimi-k3"
---

> *摘要：中国人工智能实验室 Moonshot AI 今早发布了 Kimi K3，将其描述为“迄今为止能力最强的模型，拥有 2.8 万亿参数”。目前可通过其网站和 API 使用，但承诺将在“2026 年 7 月 27 日”前开放权重。*

## Kimi K3，以及我们仍可以从鹈鹕基准测试中学到什么

2026 年 7 月 16 日

中国人工智能实验室 Moonshot AI 今早[发布了 Kimi K3](https://www.kimi.com/blog/kimi-k3)，将其描述为“迄今为止能力最强的模型，拥有 2.8 万亿参数”。目前可通过其网站和 API 使用，但承诺将在“2026 年 7 月 27 日”前开放权重。

Moonshot 称这是首个“开放型 3T 级模型”（我猜他们将 2.8 万亿向上取整为 3 万亿），从[DeepSeek 的 1.6T v4 Pro](https://huggingface.co/deepseek-ai/DeepSeek-V4-Pro)手中夺得了桂冠。他们[自行报告的基准测试](https://www.kimi.com/blog/kimi-k3#full-benchmark-table)显示，K3 在大多数项目中超越了 Claude Opus 4.8 max 和 GPT-5.5 high，但落后于 Claude Fable 5 和 GPT-5.6 Sol。

来自[Artificial Analysis 报告](https://twitter.com/ArtificialAnlys/status/2077832874183860404)的一些亮点：

* “在我们的私有长时程知识工作评估中，Kimi K3 整体 Elo 得分达到 1547，比 Kimi K2.6 高出 732 分，仅次于 Claude Fable 5。”
* “每任务成本（0.94 美元）与 GPT-5.6 Sol（1.04 美元）相近，约为 Opus 4.8（1.80 美元）的一半，高于开放权重同类产品”
* “Kimi K3 在 Artificial Analysis 智能指数上的令牌使用量显著下降，输出令牌比 K2.6 少 21%。”

该模型现在也成为[Arena.ai 前端代码竞技场](https://twitter.com/arena/status/2077824029126504525)的领先模型，甚至超越了 Claude Fable 5。

新模型在定价上引人注目：每百万输入令牌 3 美元，每百万输出令牌 15 美元，与 Anthropic 的 Claude Sonnet 系列处于同一水平，成为中国人工智能实验室迄今为止发布的最昂贵模型。这比他们早期的模型（如 [Kimi K2.6](https://platform.kimi.ai/docs/pricing/chat-k26)，每百万输入令牌 0.95 美元，每百万输出令牌 4 美元）有显著提升。2.8 万亿参数也是那个 1T 模型的两倍多。

#### 但它的鹈鹕表现如何？

我使用 OpenRouter（以避免注册 Moonshot API 密钥）和 [llm-openrouter 插件](https://github.com/simonw/llm-openrouter)生成了一个骑自行车的鹈鹕 SVG：

```
llm -m openrouter/moonshotai/kimi-k3 '生成一个骑自行车的鹈鹕的 SVG'
```

这是[对话记录](https://gist.github.com/simonw/66a2699eb1594258904c7b5102840dd6)。结果如下：

那只鹈鹕消耗了 95 个输入令牌和 16,658 个输出令牌（其中 13,241 个是推理令牌），总成本为[25 美分](https://www.llm-prices.com/#it=95&ot=16658&ic=3&oc=15)！

由于 K3 接受图像输入，我针对上面渲染的 SVG 运行了它（使用我的[替代文本提示](https://simonwillison.net/guides/agentic-engineering-patterns/prompts/#alt-text)），并[得到了](https://gist.github.com/simonw/665dbf840701b421745f2cb891acdfd6)（花费 [0.6 美分](https://www.llm-prices.com/#it=822&ot=243&ic=3&oc=15)）：

> 一只白色鹈鹕的卡通插画，戴着红色围巾，骑着红色自行车行驶在一条带有白色虚线的灰色道路上；鹈鹕有大橘色喙和橘色脚掌正在踩踏板，身后有白色运动线条；背景是浅蓝色天空，有白云、黄色太阳、两只飞行的小黑鸟，以及前景有绿色草地和微小白花。

#### 我们能从鹈鹕中学到什么？

我的[生成一个骑自行车的鹈鹕 SVG](https://simonwillison.net/tags/pelican-riding-a-bicycle/) 测试现在已有 21 个月的历史。它从来不是一个特别好的基准测试。它最初只是一个玩笑，关于比较这些模型有多么荒谬困难，但在第一年里，它竟然与模型的实际能力呈现出[惊人的相关性](https://simonwillison.net/2025/Jun/6/six-months-in-llms/)。

现在这种联系基本上已经断裂。[GPT-5.6](https://simonwillison.net/2026/Jul/9/gpt-5-6/) 和 [Claude Fable 5](https://simonwillison.net/2026/Jun/9/claude-fable-5/) 的鹈鹕被 [GLM-5.2](https://simonwillison.net/2026/Jun/17/glm-52/) 超越了，尽管我很喜欢 GLM，但我不认为它是 Fable 级别的模型。

（我仍然不相信实验室会[针对这个基准测试进行训练](https://simonwillison.net/2025/Nov/13/training-for-pelicans-riding-bicycles/)——如果他们真的这么做了，我期待结果会好得多。但 Gemini 有可能针对[动物与交通工具的任意组合](https://simonwillison.net/2026/Feb/19/gemini-31-pro/#jeff-dean)进行了优化！）

鹈鹕最大的局限性在于它完全未触及当今模型最重要的事情：智能体工具调用，以及在对话变长时可靠地操作工具的能力。

所以，不要用鹈鹕来比较模型！

尽管如此，我自己运行这个基准测试仍然能获得不少价值。

首先，它是推动实际尝试该模型的强制机制。如果我向你展示一只鹈鹕，那就意味着我成功地通过它运行了一个提示。如果模型有官方 API，我会使用它；如果它是开放权重（且足够小，可以装进 128GB M5 MacBook Pro），我会尝试在自己的机器上运行它，通常通过 [llama.cpp](https://github.com/ggml-org/llama.cpp)、[LM Studio](https://lmstudio.ai) 或 [Ollama](https://ollama.com)。我经常使用 [OpenRouter](https://openrouter.ai)，因为它通常提供官方 API 的代理，而无需我获取新的 API 密钥。

我的大多数鹈鹕都是通过[我的 LLM CLI 工具](https://llm.datasette.io/)生成的，这有助于确保最新模型都能通过它的某个插件得到支持。

更重要的是，即便只是执行一次“生成一个骑自行车的鹈鹕的 SVG”的简单提示，也能揭示有趣的模型特性。

看看今天 Kimi K3 的[结果](https://gist.github.com/simonw/66a2699eb1594258904c7b5102840dd6)。运行这些简单的提示有助于强调该模型的几个要点。

1. 它目前只有一个推理强度“max”——这一点很明显。模型消耗了 13,241 个推理令牌来输出 3,417 个令牌的响应。这很昂贵——那只鹈鹕花费了 25 美分！
2. 提示“生成一个骑自行车的鹈鹕的 SVG”是如何累积到 95 个输入令牌的？OpenAI 的[分词器](https://platform.openai.com/tokenizer)计数为 10，[Anthropic 的](https://tools.simonwillison.net/claude-token-counter)对 Opus 4.6 计数为 10，对 Opus 4.7 计数为 30，对 Sonnet 5/Fable 5 计数为 25。对 Kimi K3 使用提示“hi”[计数为 86 个令牌](https://news.ycombinator.com/item?id=48935342#48936461)，表明可能存在一个 85 令牌的隐藏系统提示。但它[拒绝泄露](https://news.ycombinator.com/item?id=48935342#48936515)出来。
3. 视觉能力表现良好：它生成的替代文本非常好。

K3 目前只有一个思考强度级别，但最近我通过以不同强度级别运行相同的鹈鹕提示，快速了解它们的影响，获得了不少价值。例如，这是我的 [GPT-5.6 模型系列矩阵](https://static.simonwillison.net/static/2026/gpt-5.6-pelicans.html)。

实际上，我从鹈鹕测试中获得的主要收益是：

1. 这是一个对模型进行提示的“hello world”练习
2. 对一个简单任务的粗略成本和推理估计
3. 确认模型能够输出有效的 SVG，并具备基本的几何和空间意识。这对于在我的笔记本电脑上运行的较小模型来说更为重要。
4. 在同一模型系列的不同版本之间比较鹈鹕仍然很有趣。K3 的鹈鹕比 [Kimi 2.5](https://simonwillison.net/2026/Jan/27/kimi-k25/) 有了显著改进。
5. 这是一个我可以分享的东西，证明我已经尝试过了。此外，在 Hacker News 上，评论中带一只鹈鹕已经成为一种传统；每当我迟交时，就会有人评论问它在哪里！

发布于 [2026 年 7 月 16 日](/2026/Jul/16/) 晚上 8:19 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的新闻通讯](https://simonwillison.net/about/#subscribe)
