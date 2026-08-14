---
title: "朝着衡量语言模型中主观全球意见代表性的方向"
description: "朝着衡量语言模型中主观全球意见代表性的方向"
pubDate: "2023-06-29"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: ""
---

# 朝着衡量语言模型中主观全球意见代表性的方向

**日期:** 2023-06-29 00:00 UTC
**链接:** https://www.anthropic.com/research/towards-measuring-the-representation-of-subjective-global-opinions-in-language-models
**标签:** 社会影响

---

[阅读论文](https://arxiv.org/abs/2306.16388)

## 摘要

大型语言模型（LLM）可能无法公平地代表关于社会问题的多样全球观点。在本文中，我们开发了一个定量框架来评估模型生成的回应与谁的意见更相似。我们首先构建了一个数据集GlobalOpinionQA，包含来自跨国调查的问题和答案，旨在捕捉不同国家在全球问题上的多样观点。接下来，我们定义了一个指标，量化LLM生成的调查回应与人类回应之间的相似性，并按国家进行条件化。利用我们的框架，我们在一个经过宪法AI训练的有用、诚实且无害的LLM上运行了三个实验。默认情况下，LLM的回应更倾向于与某些人群的意见相似，例如来自美国、一些欧洲和南美国家的人群，这突显了潜在的偏见。当我们提示模型考虑特定国家的观点时，回应会转变为与所提示人群的意见更加相似，但可能会反映出有害的文化刻板印象。当我们把GlobalOpinionQA问题翻译成目标语言时，模型的回应并不一定变得与这些语言使用者的意见最相似。我们发布我们的数据集供他人使用和构建。我们的数据位于[此URL](https://huggingface.co/datasets/Anthropic/llm_global_opinions)。我们还在[此URL](https://llmglobalvalues.anthropic.com/)提供了一个交互式可视化。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个新兴的心理工作空间，它持有模型输出中不出现的内部想法。

[了解更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们的最新经济指数报告中，我们首次按小时抽样询问：人们何时使用Claude？他们用它产生什么？以及他们如何感知AI对其工作的影响？

[了解更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了关于Claude能否帮助Anthropic员工执行复杂机器人任务的最新测试结果。我们发现，在不到一年前参与者完成的所有任务中，Claude Opus 4.7在无人协助下运行的速度比最快的人类团队快约20倍。

[了解更多](/research/project-fetch-phase-two)
