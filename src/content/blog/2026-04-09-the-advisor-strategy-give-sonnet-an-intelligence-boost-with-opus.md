---
title: "顾问策略：借助 Opus 提升 Sonnet 的智能水平"
description: "希望更好地平衡智能与成本的开发者们，已经汇聚到我们称之为顾问策略的方法上：将 Opus 作为顾问，与作为执行者的 Sonnet 或 Haiku 配对。这能为你的智能体带来接近 Opus 级别的智能，同时将成本保持在接近 Sonnet 的水平。"
pubDate: "2026-04-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/the-advisor-strategy"
---
            希望更好地平衡智能与成本的开发者们，已经汇聚到我们称之为顾问策略的方法上：将 Opus 作为顾问，与作为执行者的 Sonnet 或 Haiku 配对。这能为你的智能体带来接近 Opus 级别的智能，同时将成本保持在接近 Sonnet 的水平。

今天，我们在 Claude 平台上推出了顾问工具，使顾问策略在 API 调用中仅需一行代码即可实现。

## 使用顾问策略构建经济高效的智能体

采用顾问策略时，Sonnet 或 Haiku 作为执行者端到端地运行任务，调用工具、读取结果，并迭代地寻找解决方案。当执行者遇到一个无法合理解决的决策时，它会向作为顾问的 Opus 寻求指导。Opus 访问共享上下文并返回一个计划、修正或停止信号，随后执行者继续工作。顾问从不调用工具或产生面向用户的输出，仅向执行者提供指导。

这颠覆了常见的子智能体模式——即一个较大的编排模型分解工作并委托给较小的执行模型。在顾问策略中，一个更小、更具成本效益的模型驱动和升级，无需分解、工作池或编排逻辑。前沿级别的推理仅在执行者需要时才被应用，其余运行时间都保持在执行者级别的成本。

在我们的评估中，Sonnet 搭配 Opus 作为顾问，在 [SWE-bench Multilingual](https://www.swebench.com/multilingual.html)¹ 上相比单独使用 Sonnet 提升了 2.7 个百分点，同时每个智能体任务的成本降低了 11.9%。

## **顾问工具**

我们正在通过 [**顾问工具**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool) 将顾问策略引入我们的 API，这是一个服务器端工具，Sonnet 和 Haiku 知道在需要指导或帮助处理特定任务时调用它。

在我们的评估中，Sonnet 搭配 Opus 顾问在 BrowseComp² 和 Terminal-Bench 2.0³ 基准测试中均提高了分数，同时每个任务的成本低于单独使用 Sonnet。

顾问策略同样适用于 Haiku 作为执行者。在 BrowseComp 上，Haiku 搭配 Opus 顾问得分 41.2%，是其单独得分 19.7% 的两倍多。Haiku 搭配 Opus 顾问在分数上比单独使用 Sonnet 低 29%，但每个任务成本降低 85%。与单独使用 Haiku 相比，顾问增加了成本，但组合价格仍只是 Sonnet 成本的一小部分，使其成为需要平衡智能与成本的高容量任务的强有力选择。

在 Messages API 请求中声明 `advisor_20260301`，模型交接发生在单个 `/v1/messages` 请求内部——无需额外的往返或上下文管理。执行模型决定何时调用它。当它调用时，我们将精选的上下文路由到顾问模型，返回计划，然后执行者继续，所有这些都在同一个请求内完成。

```
response = client.messages.create(
    model="claude-sonnet-4-6",  # 执行者
    tools=[
        {
            "type": "advisor_20260301",
            "name": "advisor",
            "model": "claude-opus-4-6",
            "max_uses": 3,
        },
        # ... 你的其他工具
    ],
    messages=[...]
)

# 顾问令牌在 usage 块中
# 单独报告。
```

**定价。** 顾问令牌按顾问模型的费率计费；执行者令牌按执行者模型的费率计费。由于顾问仅生成简短计划（通常 400-700 文本令牌），而执行者以其较低的费率处理完整输出，因此总体成本远低于端到端运行顾问模型。**内置成本控制。** 设置 `max_uses` 以限制每个请求的顾问调用次数。顾问令牌在 usage 块中单独报告，以便你按层级跟踪支出。

**与你现有的工具协同工作。** 顾问工具只是 Messages API 请求中的另一个条目。你的智能体可以在同一循环中[搜索网络](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool)、[执行代码](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)并咨询 Opus。

## 开始使用

顾问工具现已在 Claude 平台上以测试版形式原生提供。要开始使用：

1. 添加测试版功能标头：`anthropic-beta: advisor-tool-2026-03-01`
2. 将 `advisor_20260301` 添加到你的 Messages API 请求中
3. 根据你的用例修改你的[系统提示](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool#suggested-system-prompt-for-coding-tasks)

我们建议针对单独使用 Sonnet、Sonnet 执行者搭配 Opus 顾问以及单独使用 Opus 运行你现有的评估套件。查阅[文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool)了解更多信息。

## 脚注

1. **SWE-bench Multilingual：** 单独使用 Sonnet 4.6 时启用了自适应思考。Sonnet 4.6 + 顾问使用了我们建议的编码系统提示并关闭了思考。两次运行均使用高努力度以及 bash 和文件编辑工具。分数为九种语言 300 个问题五次试验的平均值。Opus 4.6 在所有运行中均作为顾问模型使用。
2. **BrowseComp：** 所有运行均关闭思考，使用网络搜索和网络获取工具。Sonnet 4.6 运行使用中等努力度。Sonnet 4.6 + 顾问使用了我们建议的编码系统提示；Haiku 4.5 + 顾问未使用。无程序化工具调用或上下文压缩。分数基于 1,266 个问题，每个问题尝试一次。Opus 4.6 在所有运行中均作为顾问模型使用。‍
3. **Terminal-Bench 2.0：** 所有运行均关闭思考，使用 bash 和文件编辑工具。Sonnet 4.6 运行使用中等努力度。两次顾问运行均未使用我们建议的编码系统提示。每个任务在隔离的 Pod 中运行，资源分配为 3 倍，超时时间为 1 倍。分数为 89 个任务中每个任务五次尝试的平均值。Opus 4.6 在所有运行中均作为顾问模型使用。
