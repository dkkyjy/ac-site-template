---
title: "顾问策略：借助 Opus 为 Sonnet 提升智能水平 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/the-advisor-strategy"
---

# 顾问策略：借助 Opus 为 Sonnet 提升智能水平 | Claude by Anthropic

---

希望更好地平衡智能与成本的开发者们，已汇聚于我们称之为“顾问策略”的方法：将 Opus 作为顾问，与 Sonnet 或 Haiku 作为执行者搭配使用。这能让您的智能体获得接近 Opus 级别的智能，同时将成本保持在接近 Sonnet 的水平。

今天，我们在 Claude 平台上推出了顾问工具，使顾问策略在您的 API 调用中只需一行更改即可实现。

## 使用顾问策略构建高性价比的智能体

在顾问策略中，Sonnet 或 Haiku 作为执行者端到端地运行任务，调用工具、读取结果并迭代寻求解决方案。当执行者遇到无法合理解决的决策时，它会向作为顾问的 Opus 寻求指导。Opus 访问共享上下文并返回计划、修正或停止信号，随后执行者继续运行。顾问从不调用工具或产生面向用户的输出，仅向执行者提供指导。

这颠覆了常见的子智能体模式——在该模式中，较大的编排模型分解工作并委派给较小的执行模型。在顾问策略中，较小的、更具成本效益的模型驱动并升级任务，无需分解、工作池或编排逻辑。前沿级别的推理仅在执行者需要时应用，其余运行过程保持在执行者级别的成本。

在我们的评估中，使用 Opus 作为顾问的 Sonnet 在 [SWE-bench Multilingual](https://www.swebench.com/multilingual.html)1 上的得分比单独使用 Sonnet 提高了 2.7 个百分点，同时每个智能体任务的成本降低了 11.9%。

## **顾问工具**

我们正在通过 [**顾问工具**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool) 将顾问策略引入我们的 API。这是一种服务器端工具，Sonnet 和 Haiku 在需要针对特定任务获得指导或帮助时会调用它。

在我们的评估中，配备 Opus 顾问的 Sonnet 在 BrowseComp2 和 Terminal-Bench 2.03 基准测试中的得分均有所提升，同时每个任务的成本低于单独使用 Sonnet。

顾问策略同样适用于以 Haiku 作为执行者的情况。在 BrowseComp 上，配备 Opus 顾问的 Haiku 得分为 41.2%，是其单独得分 19.7% 的两倍多。配备 Opus 顾问的 Haiku 在得分上比单独使用 Sonnet 低 29%，但每个任务的成本降低了 85%。相对于单独使用 Haiku，顾问会增加成本，但组合价格仍仅为 Sonnet 成本的一小部分，使其成为需要智能与成本平衡的高吞吐量任务的强力选择。

在您的 Messages API 请求中声明 `advisor_20260301`，模型交接将在单个 `/v1/messages` 请求内完成——无需额外的往返或上下文管理。执行者模型决定何时调用它。当它调用时，我们会将精选上下文路由到顾问模型，返回计划，然后执行者在同一请求内继续运行。

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
        # ... 您的其他工具
    ],
    messages=[...]
)

# 顾问令牌在 usage 块中单独报告。
```

**定价。** 顾问令牌按顾问模型的费率计费；执行者令牌按执行者模型的费率计费。由于顾问仅生成简短的计划（通常为 400-700 个文本令牌），而执行者以其较低费率处理完整输出，因此总体成本远低于端到端运行顾问模型。**内置成本控制。** 设置 `max_uses` 以限制每个请求的顾问调用次数。顾问令牌在 usage 块中单独报告，以便您按层级跟踪支出。

**与您现有的工具协同工作。** 顾问工具只是您 Messages API 请求中的另一个条目。您的智能体可以在同一循环中[搜索网络](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool)、[执行代码](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)并咨询 Opus。
