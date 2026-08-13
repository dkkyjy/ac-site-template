---
title: "动态过滤助力网络搜索升级 | Anthropic 旗下 Claude"
description: "---"
pubDate: "2026-02-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/improved-web-search-with-dynamic-filtering"
---

# 动态过滤助力网络搜索升级 | Anthropic 旗下 Claude

---

与 Claude [Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 和 [Sonnet 4.6](https://www.anthropic.com/news/claude-sonnet-4-6) 一同发布的，还有我们 [网络搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool) 和 [网络抓取](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool) 工具的新版本。Claude 现在可以原生地在网络搜索过程中编写并执行代码，在搜索结果进入上下文窗口之前对其进行过滤，从而提升准确性和 Token 效率。

## **动态过滤式网络搜索**

网络搜索是一项 Token 消耗极高的任务。使用基础网络搜索工具的代理需要发起查询、将搜索结果拉入上下文、从多个网站抓取完整的 HTML 文件，然后对所有这些内容进行推理才能给出回答。但搜索引入的上下文往往包含大量无关信息，从而降低了回答质量。

为了提升 Claude 在网络搜索任务上的表现，我们的网络搜索和网络抓取工具现在会自动编写并执行代码，对查询结果进行后处理。Claude 不再需要对完整的 HTML 文件进行推理，而是可以在将搜索结果加载到上下文之前动态过滤，只保留相关内容，丢弃其余部分。

我们[此前发现](https://www.anthropic.com/engineering/advanced-tool-use)这一技术在其他代理工作流中同样有效，并且我们已经在 API 中添加了[代码执行](http://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)和[编程式工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)等原生支持。现在，我们将同样的技术引入网络搜索和网络抓取。

## **评估 Claude 的网络搜索能力**

我们在 Sonnet 4.6 和 Opus 4.6 上分别测试了启用与不启用动态过滤的网络搜索，且未启用其他工具。在 [BrowseComp](https://cdn.openai.com/pdf/5e10f4ab-d6f7-442e-9508-59515c65e35d/browsecomp.pdf) 和 [DeepsearchQA](https://storage.googleapis.com/deepmind-media/DeepSearchQA/DeepSearchQA_benchmark_paper.pdf) 两个基准测试中，动态过滤使性能平均提升 11%，同时输入 Token 使用量减少了 24%。

**BrowseComp：网络搜索以找到一个答案**

BrowseComp 测试代理能否在众多网站中导航，找到一条故意难以在网络上找到的特定信息。动态过滤显著提升了 Claude 的准确率：Sonnet 4.6 从 33.3% 提升至 46.6%，Opus 4.6 从 45.3% 提升至 61.6%。

**DeepsearchQA：网络搜索以找到多个答案**

DeepsearchQA 向代理提出具有多个正确答案的研究型查询，代理必须通过网络搜索找到所有答案。该测试评估代理能否系统性地规划并执行多步搜索，而不遗漏任何答案。衡量指标是“F1 分数”，它兼顾精确率与召回率——既体现返回答案的准确性，也体现搜索的完整性。

动态过滤将 Claude 的 F1 分数从 Sonnet 4.6 的 52.6% 提升至 59.4%，从 Opus 4.6 的 69.8% 提升至 77.3%。

Token 成本会根据模型为过滤上下文所需编写的代码量而有所不同。在 Sonnet 4.6 上，两个基准测试的加权 Token 成本均有所下降，但在 Opus 4.6 上则有所上升。为了更好地了解您自身的成本，我们建议使用您的代理在生产中可能遇到的一组代表性网络搜索查询来评估此工具。

## 客户案例：Quora

[Quora](https://quora.com) 旗下的 [Poe](https://poe.com) 是最大的多模型 AI 平台之一，为数百万用户提供通过单一界面访问超过 200 个模型的途径。Quora 内部团队发现，Opus 4.6 配合动态过滤“在我们的内部评估中，与其他前沿模型相比取得了最高准确率，”产品与研究负责人 Gareth Jones 表示。“该模型的行为就像一位真正的研究员，编写 Python 代码来解析、过滤和交叉引用结果，而不是在上下文中对原始 HTML 进行推理。”

## 网络搜索与抓取工具中的动态过滤

在使用 Claude API 上的新网络搜索和网络抓取工具搭配 Sonnet 4.6 和 Opus 4.6 时，动态过滤将默认开启。对于复杂的网络搜索查询，例如筛选技术文档或验证引用，您可以预期获得与上述类似的性能提升。

以下是在 API 中的使用方式：

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
      "content": "搜索 AAPL 和 GOOGL 的当前价格，然后计算哪个具有更好的市盈率。"
    }
  ]
}
```

## 代码执行、记忆及其他工具现已全面可用

我们还将多项工具升级为正式可用，以帮助代理在 Token 密集型任务中表现更佳：

* [代码执行](http://docs.anthropic.com/en/docs/agents-and-tools/tool-use/code-execution-tool)**：**为代理提供沙箱，以便在对话期间运行代码来过滤上下文、分析数据或执行计算。
* [记忆](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)：通过持久化文件目录在对话之间存储和检索信息，使代理能够保留上下文，而无需将一切保持在上下文窗口中。
* [编程式工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)**：**以代码形式执行复杂的多工具工作流，将中间结果保留在上下文窗口之外。
* [工具搜索](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)：从大型库中动态发现工具，而无需将所有定义加载到上下文窗口中。[‍](https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use#providing-tool-use-examples)
* [工具使用示例](https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use#providing-tool-use-examples)**：**直接在工具定义中提供示例工具调用，以演示使用模式并减少参数错误。

### **开始使用**

改进后的网络搜索和网络抓取——以及代码执行、记忆、编程式工具调用、工具搜索和工具使用示例——现已在 Claude 平台上提供。请阅读我们的 [API 文档](https://platform.claude.com/docs/en/build-with-claude/overview) 开始使用。
