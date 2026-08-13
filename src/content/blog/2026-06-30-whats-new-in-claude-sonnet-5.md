---
title: "Claude Sonnet 5 新特性"
description: "Claude Sonnet 5 新特性"
pubDate: "2026-06-30"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jun/30/claude-sonnet-5"
---

> *摘要：Claude Sonnet 5 新特性
Claude Sonnet 5 今早发布了。我总是直接去看“新特性”开发者文档，因为它们往往比官方公告包含更多可操作信息。*

2026年6月30日 - 链接博客

**[Claude Sonnet 5 新特性](https://platform.claude.com/docs/en/about-claude/models/whats-new-sonnet-5)**（[来源](https://news.ycombinator.com/item?id=48736605 "Hacker News")）Claude Sonnet 5 于[今早](https://www.anthropic.com/news/claude-sonnet-5)发布。我总是直接去看“新特性”开发者文档，因为它们往往比官方公告包含更多可操作信息。

Anthropic 对 Sonnet 5 的评价是：“其性能接近 Opus 4.8，但价格更低”。[系统卡](https://www-cdn.anthropic.com/9e6a1044980d8c4ed85669faf9c2a8342e2e9f1e/Claude%20Sonnet%205%20System%20Card.pdf)帮助解释了为什么他们能在不受美国政府阻碍的情况下发布该模型：

> Sonnet 5 在网络任务方面的能力明显不如 Mythos 5：因此其防护措施与我们应用于 Opus 4.7 和 Opus 4.8 的类似（这些模型比 Sonnet 5 更强大，但远不如 Mythos 5）。

值得注意的 API 变更（来自“新特性”）：

* 采样参数 `temperature`、`top_p`、`top_k` 不再受支持。
* 拥有 100 万 token 的上下文窗口和 128,000 的最大输出 token。
* 具备“与 Claude Sonnet 4.6 相同的工具集和平台功能”。
* 自适应思考默认开启，除非你指定 `"thinking": {type: "disabled"}`。
* 定价与 Sonnet 4.6 相同：每百万输入 token 3 美元，每百万输出 token 15 美元，并在 8 月 31 日前提供入门折扣至 2 美元/10 美元。但是……
* 该模型采用了新的分词器，“相同输入文本产生的 token 数量比 Claude Sonnet 4.6 多约 30%”——实际上相当于价格上涨了 30%。

我使用我的 [Claude Token Counter](https://tools.simonwillison.net/claude-token-counter) 工具测试了新分词器。以下是针对几个较大文档的结果：

| 文档 | Sonnet 4.6 | Opus 4.7 | Sonnet 5 |
| --- | --- | --- | --- |
| [世界人权宣言（英语）](https://github.com/simonw/udhr-markdown/blob/main/declarations/eng.md) | **2,356** | **3,347** 1.42x | **3,341** 1.42x |
| [世界人权宣言（西班牙语）](https://github.com/simonw/udhr-markdown/blob/main/declarations/spa.md) | **3,572** | **4,753** 1.33x | **4,747** 1.33x |
| [世界人权宣言（简体中文）](https://github.com/simonw/udhr-markdown/blob/main/declarations/cmn_hans.md) | **3,334** | **3,366** 1.01x | **3,360** 1.01x |
| [sqlite\_utils/db.py](https://github.com/simonw/sqlite-utils/blob/79117b9d110d72f46dab5fe2cda412ff4789ab55/sqlite_utils/db.py)（4279 行 Python 代码） | **44,014** | **56,118** 1.28x | **56,113** 1.27x |

因此，新 token 对英语来说大约贵 1.4 倍，对西班牙语贵 1.33 倍，对 Python 代码贵 1.28 倍，而对简体中文的实际成本相同。

这是[那只鹈鹕](https://gist.github.com/simonw/a89e756b621a31e8ffc210e3428efa77)。没什么可大书特书的。Sonnet 5 觉得它看起来像只鹅。

发布于 [2026 年 6 月 30 日](/2026/Jun/30/) 晚上 9:23
