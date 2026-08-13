---
title: "消息批处理 API 介绍"
description: "开发者每次最多可提交 10,000 个查询。每个批处理在 24 小时内处理完成，成本比标准 API 调用低 50%。这使得处理非时间敏感型任务更加高效且经济。"
pubDate: "2024-10-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/message-batches-api"
---
            ***更新：*** *消息批处理 API 已在 Anthropic API 上全面可用。在 Amazon Bedrock 上使用 Claude 的客户可以使用批处理推理。批处理预测也已在 Google Cloud 的 Vertex AI 上提供预览版。（2024年12月17日）*我们推出了一项全新的[消息批处理 API](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)——这是一种强大且经济高效的方式，用于异步处理大量查询。

开发者每次最多可提交 10,000 个查询。每个批处理在 24 小时内处理完成，成本比标准 API 调用低 50%。这使得处理非时间敏感型任务更加高效且经济。

批处理 API 目前以公开测试版形式提供，支持 Anthropic API 上的 Claude 3.5 Sonnet、Claude 3 Opus 和 Claude 3 Haiku。在 Amazon Bedrock 上使用 Claude 的客户可以使用[批处理推理](https://docs.aws.amazon.com/bedrock/latest/userguide/batch-inference.html)。对[Google Cloud Vertex AI 上的 Claude](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) 的批处理支持即将推出。

## 半价享受高吞吐量

开发者经常使用 Claude 处理海量数据——从分析客户反馈到翻译语言——这些场景不需要实时响应。

无需管理复杂的队列系统或担心速率限制，您可以使用批处理 API 提交最多 10,000 个查询的组，让 Anthropic 以 50% 的折扣处理这些请求。批处理将在 24 小时内处理完成，但通常速度更快。其他优势包括：

* **增强的吞吐量：** 享受更高的速率限制，处理更大规模的请求量，同时不影响您的标准 API 速率限制。
* **大数据可扩展性：** 处理大规模任务，如数据集分析、大型数据集分类或广泛的模型评估，无需担心基础设施问题。

批处理 API 为以前不太实用或成本过高的大规模数据处理开启了新的可能性。例如，分析整个企业文档库——可能涉及数百万个文件——通过利用我们的批处理折扣变得更加经济可行。

## 定价

批处理 API 让您能够利用基础设施成本节省，并为输入和输出令牌均提供 50% 的折扣。

|  |  |  |
| --- | --- | --- |
| **Claude 3.5 Sonnet**  * 我们目前最智能的模型 * 200K 上下文窗口 | **批处理输入**  * $1.50 / 百万令牌 | **批处理输出**  * $7.50 / 百万令牌 |
| **Claude 3 Opus**  * 适用于复杂任务的强大模型 * 200K 上下文窗口 | **批处理输入**  * $7.50 / 百万令牌 | **批处理输出**  * $37.50 / 百万令牌 |
| **Claude 3 Haiku**  * 最快、最具成本效益的模型 * 200K 上下文窗口 | **批处理输入**  * $0.125 / 百万令牌 | **批处理输出**  * $0.625 / 百万令牌 |

## 客户聚焦：Quora

[Quora](https://cloud.google.com/customers/quora?hl=en)，一个基于用户问答的平台，利用 Anthropic 的批处理 API 进行摘要和重点提取，以创建新的终端用户功能。

"Anthropic 的批处理 API 不仅节省了成本，还降低了运行大量无需实时处理的查询的复杂性，"Quora 产品经理 Andy Edmonds 表示。"提交一个批处理并在 24 小时内下载结果非常方便，而无需处理运行大量并行实时查询以获得相同结果的复杂性。这为我们的工程师腾出了时间，让他们可以专注于更有趣的问题。"

## 开始使用

要开始在 Anthropic API 上使用公开测试版的批处理 API，请查阅我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)和[定价页面](https://docs.anthropic.com/en/docs/build-with-claude/message-batches)。
