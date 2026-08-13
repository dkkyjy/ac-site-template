---
title: "smevals - 一个用于评估模型、提示词和测试框架的小型评估套件"
description: "smevals - 一个用于评估模型、提示词和测试框架的小型评估套件"
pubDate: "2026-07-31"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/31/smevals"
---

> *订阅源摘要：smevals - 一个用于评估模型、提示词和测试框架的小型评估套件
我一直在与 Jesse Vincent 的 Prime Radiant 应用人工智能研究实验室合作，构建这个评估框架，以帮助回答*

2026年7月31日 - 链接博客

**[smevals - 一个用于评估模型、提示词和测试框架的小型评估套件](https://primeradiant.com/blog/2026/smevals.html)**。我一直在与 Jesse Vincent 的 [Prime Radiant](https://primeradiant.com) 应用人工智能研究实验室合作，构建这个评估框架，以帮助回答关于不同模型能力的问题。

成果就是 **[smevals](https://github.com/prime-radiant-inc/smevals)**，一个用于在不同模型配置上运行小型评估套件并对结果进行评分的新工具。

这篇博客文章详细介绍了该工具。以下是十秒速览版：

1. 告诉你的编程代理运行 `run uvx smevals docs` 来了解该工具（这会输出 [README](https://github.com/prime-radiant-inc/smevals/blob/main/README.md)）
2. 然后告诉它为你构建一个评估套件

一旦你创建了一个评估——其形式为一个包含若干 YAML 文件的目录——你就可以像这样针对模型运行它：

```
uvx smevals run path-to-eval/ -m gpt-5.5 -m claude-opus-4.6
```

运行与评分操作是分开处理的——你可以使用以下命令对你的运行进行评分（根据你定义的一组检查）：

```
uvx smevals grade path-to-eval/
```

然后你可以运行一个本地 Web 服务器来浏览结果：

```
uvx smevals serve path-to-eval/
```

或者运行 `smevals build` 命令将该报告构建为静态 HTML，然后你可以将其托管在任何地方。这是[一个示例](https://static.simonwillison.net/static/2026/smevals-haiku-build/#/haiku)，展示了我构建的一个评估套件，用于评估模型写俳句的能力。

这个项目最耗时的部分是确定它的词汇表！以下是我最终确定的内容，引自公告：

> * 一个**评估（eval）** 是一组挑战的集合，旨在回答关于模型的问题，例如，该模型生成 SVG 的能力有多好？
> * 每个评估是一组**任务（tasks）** 的集合。一个任务是一个具体的挑战，例如"生成一只骑着自行车的鹈鹕的 SVG"。
> * 当你运行评估时，你是针对一个或多个**配置（configs）** 来运行的。每个配置指定一个待评估的模型，但也可以包含其他要测试的参数，例如不同的系统提示词、模型参数或代理框架。
> * 一次**运行（run）** 记录了当特定配置被用于执行特定任务时发生的情况。**运行器（runner）** 是执行运行的脚本。
> * 一旦你收集了一次或多次运行，你需要评估结果以了解模型（或配置）的表现如何。这是由**评分器（grader）** 完成的，它会产生一个**评分（grade）**。
> * 每个评分器运行一系列**检查（checks）**。这些可以是简单的操作，比如检查输出中是否包含特定字符串，或确认输出是有效的 XML。它们也可以是更复杂的自定义操作（实现为称为**检查器（checkers）** 的脚本），包括使用其他模型来回答关于运行的问题。

几年来，我一直在尝试找到一种我喜欢的评估方法。`smevals` 是我对这个想法的第三次迭代，我觉得它很合适。我期待在未来进一步扩展它，并将其应用于我自己的一些项目。

发布于 [2026年7月31日](/2026/Jul/31/) 晚上 9:15
