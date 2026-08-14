---
title: "从重复数据中学习的缩放定律与可解释性"
description: "从重复数据中学习的缩放定律与可解释性"
pubDate: "2022-05-21"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/scaling-laws-and-interpretability-of-learning-from-repeated-data"
---

# 从重复数据中学习的缩放定律与可解释性  
**标签：** 可解释性，研究  

---

[阅读论文](https://arxiv.org/abs/2205.10487)

## 摘要

近期的大语言模型已经在海量数据集上进行了训练，但往往也接触到了重复数据——无论是为了加权高质量数据而有意为之，还是因为数据去重不彻底，导致模型在句子、段落或文档层面接触到重复数据。一些研究已报告这种重复数据会对性能产生显著的负面影响。在本文中，我们试图系统地研究重复数据，并从机制上理解其影响。为此，我们训练了一系列模型，其中大部分数据是唯一的，但有一小部分数据被重复了多次。我们发现了一种强烈的双下降现象，即重复数据可能导致测试损失在训练中途增加。在某个可预测的重复频率范围内，性能会出现令人惊讶的严重退化。例如，一个8亿参数的模型，如果将其0.1%的数据重复100次，尽管其余90%的训练token仍然是唯一的，其性能也会退化到只有一半大小（4亿参数）的模型水平。我们推测存在一个中间范围，在该范围内数据可以被记忆，而这样做会消耗模型的大部分容量，这或许就是退化峰值出现的位置。最后，我们将这些观察与近期机制可解释性研究——即尝试逆向工程模型执行的详细计算——联系起来，通过证明数据重复会不成比例地损害与泛化相关的复制和内部结构（如归纳头），从而为从泛化到记忆的转变提供了一种可能的机制。综合来看，这些结果为以下假设提供了依据：在大型语言模型中重复相对较小比例的数据，可能导致性能遭遇不成比例的严重损害。

## 作者

Amanda Askell, Yuntao Bai, Anna Chen, Dawn Drain, Deep Ganguli, Tom Henighan, Andy Jones, Nicholas Joseph, Ben Mann, Nova DasSarma, Nelson Elhage, Zac Hatfield-Dodds, Danny Hernandez, Jackson Kernion, Kamal Ndousse, Catherine Olsson, Dario Amodei, Tom Brown, Jack Clark, Sam McCandlish, Chris Olah, Jared Kaplan

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个涌现的心理工作空间，其中包含不会出现在模型输出中的内部思考。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，以探讨：人们何时使用Claude？他们用它生成了什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了最新测试结果——测试Claude能否帮助Anthropic员工执行复杂的机器人任务。我们发现，Claude Opus 4.7在无需人工协助的情况下，完成所有任务的速度比不到一年前人类最快的团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
