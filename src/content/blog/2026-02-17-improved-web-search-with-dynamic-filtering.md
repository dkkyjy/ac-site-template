---
title: "通过动态过滤改进网络搜索"
description: "与 Claude [Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 和 [Sonnet 4.6](https://www.anthropic.com/news/claude-sonnet-4-6) 一同发布，我们正在推出新版本的[网络搜索](https://platform.claude.com/docs/en/agents-an"
pubDate: "2026-02-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/improved-web-search-with-dynamic-filtering"
---
            与 Claude [Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 和 [Sonnet 4.6](https://www.anthropic.com/news/claude-sonnet-4-6) 一同发布，我们正在推出新版本的[网络搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool)和[网络抓取](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool)工具。Claude 现在可以在网络搜索期间原生编写和执行代码，在结果进入上下文窗口之前对其进行过滤，从而提高其准确性和 Token 效率。

## **具有动态过滤的网络搜索**

网络搜索是一项高度消耗 Token 的任务。使用基本网络搜索工具的代理需要发出查询，将搜索结果拉入上下文，从多个网站获取完整的 HTML 文件，并在响应之前对所有内容进行推理。但从搜索中拉入的上下文通常是不相关的，这会降低响应的质量。

为了提高 Claude 在网络搜索中的性能，我们的网络搜索和网络抓取工具现在会自动编写和执行代码来后处理查询结果。Claude 无需对完整的 HTML 文件进行推理，而是可以在将搜索结果加载到上下文之前动态过滤它们，只保留相关内容并丢弃其余部分。

我们[之前发现](https://www.anthropic.com/engineering/advanced-tool-use)这种技术在其他代理工作流中也很有效，并且我们添加了诸如[代码执行](http://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)和[程序化工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)等工具，以在我们的 API 上提供原生支持。我们现在正在将相同的技术应用于网络搜索和网络抓取。

## **评估 Claude 搜索网络的能力**

我们在 Sonnet 4.6 和 Opus 4.6 上评估了网络搜索，分别使用和不使用动态过滤，并且没有启用其他工具。在两个基准测试中，[BrowseComp](https://cdn.openai.com/pdf/5e10f4ab-d6f7-442e-9508-59515c65e35d/browsecomp.pdf) 和 [DeepsearchQA](https://storage.googleapis.com/deepmind-media/DeepSearchQA/DeepSearchQA_benchmark_paper.pdf)，动态过滤使性能平均提高了 11%，同时使用的输入 Token 减少了 24%。

**BrowseComp：搜索网络以找到一个答案**

BrowseComp 测试代理是否能够浏览众多网站以找到一条特定信息，该信息故意难以在网上找到。动态过滤显著提高了 Claude 的准确性，将 Sonnet 4.6 从 33.3% 提升到 46.6%，将 Opus 4.6 从 45.3% 提升到 61.6%。

**DeepsearchQA：搜索网络以找到多个答案**

DeepsearchQA 向代理提出具有多个正确答案的研究查询，所有这些答案都必须通过网络搜索找到。它测试代理是否能够系统地规划和执行多步骤搜索而不遗漏任何答案。它通过"F1 分数"来衡量，该分数平衡了精确率和召回率——既捕捉返回答案的准确性，也捕捉搜索的完整性。

动态过滤将 Claude 的 F1 分数从 Sonnet 4.6 的 52.6% 提高到 59.4%，从 Opus 4.6 的 69.8% 提高到 77.3%。

Token 成本将根据模型需要编写多少代码来过滤上下文而变化。在两项基准测试中，Sonnet 4.6 的加权价格 Token 有所下降，但 Opus 4.6 的有所上升。为了更好地了解您自己的成本，我们建议根据您的代理在生产环境中可能遇到的一组代表性网络搜索查询来评估此工具。

## 客户聚焦：Quora

[Quora](https://quora.com) 旗下的 [Poe](https://poe.com) 是最大的多模型 AI 平台之一，通过单一界面为数百万用户提供访问超过 200 个模型的权限。Quora 的内部团队发现，具有动态过滤的 Opus 4.6"在与其它前沿模型进行测试时，在我们的内部评估中实现了最高的准确率，"产品和研究负责人 Gareth Jones 说道。"该模型的行为就像一位真正的研究员，编写 Python 来解析、过滤和交叉引用结果，而不是在上下文中对原始 HTML 进行推理。"

## 网络搜索和抓取工具中的动态过滤

在 Claude API 上使用我们新的网络搜索和网络抓取工具与 Sonnet 4.6 和 Opus 4.6 时，动态过滤将默认开启。对于复杂的网络搜索查询，例如筛选技术文档或验证引用，您可以期待与上述类似的性能改进。

以下是如何在 API 中使用它：

```
{
  "model": "claude-opus-4-6",
  "max_tokens": 4096,
  "tools": [
    {
      "type": "web_search_20260209",
      "name": "web_search"
    },
    {
      "type": "web_fetch_20260209",
      "name": "web_fetch"
    }
  ],
  "messages": [
    {
      "role": "user",
      "content": "搜索 AAPL 和 GOOGL 的当前价格，然后计算哪一个具有更好的市盈率。"
    }
  ]
}
```

## 代码执行、记忆和更多工具现已普遍可用

我们还将多个工具升级为普遍可用，以帮助代理在 Token 密集型任务中表现更好：

* [代码执行](http://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)：提供一个沙箱，供代理在对话期间运行代码，以过滤上下文、分析数据或执行计算。
* [记忆](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)：通过持久化文件目录在对话之间存储和检索信息，使代理无需将所有内容保留在上下文窗口中即可保留上下文。
* [程序化工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)：在代码中执行复杂的多工具工作流，将中间结果保留在上下文窗口之外。
* [工具搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)：从大型库中动态发现工具，而无需将所有定义加载到上下文窗口中。
* [工具使用示例](https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use#providing-tool-use-examples)：直接在工具定义中提供示例工具调用，以演示使用模式并减少参数错误。

### **开始使用**

改进的网络搜索和网络抓取——以及代码执行、记忆、程序化工具调用、工具搜索和工具使用示例——现已在 Claude 平台上可用。阅读我们的 [API 文档](https://platform.claude.com/docs/en/build-with-claude/overview) 开始使用。
