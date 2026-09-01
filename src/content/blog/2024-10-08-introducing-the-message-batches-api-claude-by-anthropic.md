---
title: "介绍消息批处理 API | Anthropic 的 Claude"
description: "---"
pubDate: "2024-10-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/message-batches-api"
---

# 介绍消息批处理 API | Anthropic 的 Claude

---

***更新：*** *消息批处理 API 已在 Anthropic API 上全面可用。使用 Amazon Bedrock 上 Claude 的客户可以调用批推理功能。批预测功能也已在 Google Cloud 的 Vertex AI 上以预览版形式提供。（2024 年 12 月 17 日）*我们隆重推出一项全新的[消息批处理 API](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)——一种功能强大且经济高效的方式，用于异步处理海量查询。

开发者每次可提交最多 10,000 条查询的批次。每个批次的处理时间不超过 24 小时，费用比标准 API 调用低 50%。这使得处理非时效性任务变得更加高效、更具成本效益。

批处理 API 现已以公开测试版形式上线，在 Anthropic API 上支持 Claude 3.5 Sonnet、Claude 3 Opus 和 Claude 3 Haiku。使用 Amazon Bedrock 上 Claude 的客户可以调用[批推理](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference.html)功能。针对 [Google Cloud Vertex AI 上的 Claude](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) 的批处理支持即将推出。

## 以半价实现高吞吐量

开发者经常使用 Claude 处理海量数据——从分析客户反馈到语言翻译——在这些场景中，实时响应并非必需。

您无需管理复杂的队列系统，也不必担心速率限制，只需使用批处理 API 提交最多 10,000 条查询的批次，即可由 Anthropic 以 50% 的折扣价格完成处理。批次将在 24 小时内处理完毕，通常还会更快。此外还有其他优势：

* **增强的吞吐量：** 享受更高的速率限制，在不影响标准 API 速率限制的前提下处理更大的请求量。
* **面向大数据的可扩展性：** 处理大规模任务，如数据集分析、大规模数据集分类或广泛的模型评估，而无需顾虑基础设施问题。

批处理 API 为大规模数据处理开辟了全新的可能性，而这些任务此前往往因成本过高而难以实施。例如，分析整个企业文档库——这可能涉及数百万个文件——借助我们的批处理折扣，变得更加经济可行。

## 定价

批处理 API 让您能够享受基础设施成本方面的节省，输入和输出 token 均享有 50% 的折扣。

|   |   |   |
| --- | --- | --- |
| **Claude 3.5 Sonnet**   * 我们迄今最智能的模型 * 200K 上下文窗口 | **批处理输入**   * $1.50 / MTok | **批处理输出**   * $7.50 / MTok |
| **Claude 3 Opus**   * 适用于复杂任务的强大模型 * 200K 上下文窗口 | **批处理输入**   * $7.50 / MTok | **批处理输出**   * $37.50 / MTok |
| **Claude 3 Haiku**   * 速度最快、最具成本效益的模型 * 200K 上下文窗口 | **批处理输入**   * $0.125 / MTok | **批处理输出**   * $0.625 / MTok |

## 客户聚焦：Quora

[Quora](https://cloud.google.com/customers/quora?hl=en) 是一个基于用户的问答平台，利用 Anthropic 的批处理 API 进行摘要生成和要点提取，以打造面向终端用户的新功能。

"Anthropic 的批处理 API 在降低成本的同时，也大幅简化了运行大量无需实时处理的查询的复杂性，"Quora 产品经理 Andy Edmonds 表示。"只需提交一个批次，然后在 24 小时内下载结果，远比同时运行大量并行实时查询来得方便，后者才能达到同样的效果。这让我们工程师得以将时间投入到更有意义的问题上。"

## 开始使用

要在 Anthropic API 上以公开测试版形式开始使用批处理 API，请参阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)和[定价页面](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)。
