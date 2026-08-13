---
title: "使用 DSPy 评估和改进 Datasette Agent 的 SQL 系统提示"
description: "使用 DSPy 评估和改进 Datasette Agent 的 SQL 系统提示"
pubDate: "2026-07-02"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/2/dspy-datasette-agent-prompts"
---

> *Feed 摘要：研究：使用 DSPy 评估和改进 Datasette Agent 的 SQL 系统提示
        今天早上的 AIE 主题演讲之一涉及了 dspy，这提醒我一直想看看它能否帮助我改进 D*

2026 年 7 月 2 日

[研究](/elsewhere/research/)
[使用 DSPy 评估和改进 Datasette Agent 的 SQL 系统提示](https://github.com/simonw/research/tree/main/dspy-datasette-agent-prompts#readme)
— 借助 DSPy 框架，本项目评估并优化了 Datasette Agent 的只读 SQL 问答器所使用的核心生产系统提示。该方法涉及一个测试框架：DSPy 代理调用 Datasette Agent 实际工具实现和提示，针对一个运行中的进程内 Datasette 实例；同时，一个自动生成的金标准数据集通过自定义指标提供严格评估。

今天早上的 AIE 主题演讲之一涉及了 [dspy](https://github.com/stanfordnlp/dspy)，这提醒我一直想看看它能否帮助我改进 [Datasette Agent](https://agent.datasette.io) 所使用的系统提示——于是我通过 Claude Fable 5 在 Claude Code for Web 中启动了一个异步研究任务：

> `Pip 安装最新的 Datasette alpha 版、datasette-agent 和 dspy——然后弄清楚如何使用 dspy 来评估和改进 Datasette Agent 的主要系统提示，该提示用于执行只读 SQL 查询以回答用户关于数据的问题。`

Fable 选择使用 GPT 4.1 mini 和 nano 进行测试，并确定了几个有希望的改进方向。我尤其喜欢这个：

> 模式列表只提供表名；“如果你已经拥有信息，不要调用 describe\_table”这条建议导致基线追踪中出现列名猜测（page\_count、o.order\_id、first\_name）和错误重试循环。要么在提示的模式列表中包含列名，要么软化这条建议。

发布于 [2026 年 7 月 2 日](/2026/Jul/2/) 18:25
