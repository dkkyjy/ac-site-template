---
title: "新版 GPT-5.6 家族：Luna、Terra、Sol"
description: "新版 GPT-5.6 家族：Luna、Terra、Sol"
pubDate: "2026-07-09"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/9/gpt-5-6"
---

> *摘要：OpenAI 最新的旗舰模型于今早全面上市，并提供三种尺寸：Luna、Terra 和 Sol（从小到大）。
新模型的定价为每 100 万输入/输出 token：*

## 新版 GPT-5.6 家族：Luna、Terra、Sol

2026 年 7 月 9 日

OpenAI 最新的旗舰模型[于今早全面上市](https://openai.com/index/gpt-5-6/)，并提供三种尺寸：Luna、Terra 和 Sol（从小到大）。

新模型的定价为每 100 万输入/输出 token：Luna 1 美元/6 美元，Terra 2.50 美元/15 美元，Sol 5 美元/30 美元。作为对比，Claude Opus 系列为 5 美元/25 美元，Claude Fable 5 为 10 美元/50 美元，但每百万 token 的价格现在并不能说明太多问题，因为对于同一任务，不同模型的推理 token 数量可能相差很大。

所有三个模型的知识截止日期均为 2026 年 2 月 16 日，上下文窗口为 100 万 token，最大输出 token 为 128,000。

OpenAI 最大的基准测试宣称涉及长期运行的代理性能，一项基准测试显示所有三个模型均优于 Claude Fable 5：

> 我们训练了 GPT-5.6，让每个 token 都能产生更多有用的工作。在 [Agents' Last Exam](https://agents-last-exam.org/)（一项横跨 55 个领域的长期专业工作流评估）中，GPT-5.6 Sol 以 53.6 分创下新高，比 Claude Fable 5（自适应推理）高出 13.1 分。即使在中等推理水平下，它也比 Fable 5 高出 11.4 分，而估算成本仅为后者的约四分之一。这种效率延伸到了更小尺寸的模型，这对于让智能变得更丰富和更便宜至关重要：GPT-5.6 Terra 和 GPT-5.6 Luna 以约十六分之一的成本超越了 Fable 5。

有趣的是，Fable 5 在 SWE-Bench Pro 上以 80% 的成绩碾压了 GPT-5.6 家族（GPT-5.6 Sol 仅为 64.6%）。这或许有助于解释为什么 OpenAI 选择在[昨天发布这篇文章](https://openai.com/index/separating-signal-from-noise-coding-evaluations/)，专门指出他们在审计该基准测试时发现的问题：

> 鉴于这些结果，我们估计 SWE-bench Pro 中约有 30% 的任务存在问题，并建议模型开发者仔细审查结果。

我提前体验过 GPT-5.6 Sol——它确实非常能干，不过到目前为止，在我一直在与 Anthropic 模型配合使用的复杂编码任务上，它并没有给我留下比 Fable 更好的印象。

和往常一样，[使用 GPT-5.6 的模型指南](https://developers.openai.com/api/docs/guides/latest-model?model=gpt-5.6)包含了最有趣的细节。有一系列新的 API 功能我需要探索（可能还要在 [LLM](https://llm.datasette.io/) 中添加支持），包括：

* [编程式工具调用](https://developers.openai.com/api/docs/guides/tools-programmatic-tool-calling)允许模型“组合并运行 JavaScript 来编排工具调用”——在我看来，这可能有助于弥合 MCP 与完整终端会话之间的差距，让你能够以有用的方式组合 CLI 工具。这也让人联想到 Anthropic 为其网络搜索工具添加的[动态过滤](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool#dynamic-filtering)机制，该机制允许在单次模型轮次中对网络结果执行代码。
* [多代理](https://developers.openai.com/api/docs/guides/tools-multi-agent)让模型可以“生成子代理进行并行、专注的工作”——子代理模式现已内置于核心 API 中。
* [提示缓存断点](https://developers.openai.com/api/docs/guides/prompt-caching#prompt-cache-breakpoints)将 Claude 的提示缓存模型引入 OpenAI，让你可以明确指定缓存断点的位置，而不是依赖 API 自动检测。就个人而言，我更喜欢自动检测（OpenAI 仍然支持），但如果你愿意投入精力，大概可以在这里获得优化成本节省。
* 你现在可以在图像请求中设置 `detail: original`，以避免在处理图像前对其进行任何调整大小。

这里有[一个包含 18 只不同鹈鹕的完整页面](https://static.simonwillison.net/static/2026/gpt-5.6-pelicans.html)——对应三种不同模型在推理努力程度为 none、low、medium、high、xhigh 和 max 下的表现。它还列出了 token 数量和计算出的成本——最便宜的是 gpt-5.6-luna，努力程度为 none，0.71 美分；最贵的是 gpt-5.6-sol，最大推理水平，48.55 美分。

更多鹈鹕消息：如果你跳到他们[今早直播](https://www.youtube.com/live/Wq45rvPGNHs?t=1070s)的 17:50 处，你会看到 OpenAI 自己的演示——3D 鹈鹕骑着三轮车、自行车、小马，以及另一只鹈鹕！

发布于 [2026 年 7 月 9 日](/2026/Jul/9/) 晚上 7:46 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或者[订阅我的通讯](https://simonwillison.net/about/#subscribe)
