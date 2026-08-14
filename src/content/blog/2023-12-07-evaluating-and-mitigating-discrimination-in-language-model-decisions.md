---
title: "评估与减轻语言模型决策中的歧视"
description: "评估与减轻语言模型决策中的歧视"
pubDate: "2023-12-07"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/evaluating-and-mitigating-discrimination-in-language-model-decisions"
---

# 评估与减轻语言模型决策中的歧视  
**标签：** 社会影响

---

[阅读论文](http://arxiv.org/abs/2312.03689)

## 摘要

随着语言模型的进步，人们越来越有兴趣将其应用于高风险的社会议决策，例如确定融资或住房资格。然而，在此类情境下它们可能产生歧视，这引发了伦理方面的担忧，促使我们需要更好的方法来评估这些风险。本文提出了一种方法，用于主动评估语言模型在广泛用例中的潜在歧视影响，包括假设的尚未部署的用例。具体来说，我们使用一个语言模型生成决策者可能输入给语言模型的多种潜在提示，涵盖70个不同的社会决策场景，并系统地改变每个提示中的的人口统计信息。应用该方法揭示了，在未施加任何干预措施时，Claude 2.0 模型在特定场景下展现出正向与负向歧视的模式。虽然我们不认可或允许将语言模型用于我们所研究的高风险用例中的自动决策，但本文展示了通过精心设计提示来显著降低正向与负向歧视的技术，为在适当用例中实现更安全的部署提供了路径。我们的工作使开发者和政策制定者能够在语言模型能力与应用不断扩展的同时，预测、衡量和应对歧视。我们在此发布数据集和提示：[https://huggingface.co/datasets/Anthropic/discrim-eval](https://huggingface.co/datasets/Anthropic/discrim-eval)。

## 政策备忘录

[评估与减轻语言模型决策中的歧视 政策备忘录](https://www-cdn.anthropic.com/f0dfb70b9b309d7c52845f73da8d964140669ff7/Anthropic_DiscriminationEval.pdf)

## 相关内容

### 语言模型中的全局工作空间

最新的可解释性研究揭示了Claude中存在一个涌现的心理工作空间，这个空间容纳了不出现在模型输出中的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在最新的经济指数报告中，我们首次按小时采样，提出以下问题：人们何时访问Claude？他们用它产生什么？以及他们如何看待人工智能对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目 Fetch：第二阶段

我们报告了最新测试的结果，该测试评估Claude是否能够帮助Anthropic员工完成复杂的机器人任务。我们发现，在不到一年前由参与者完成的所有任务中，Claude Opus 4.7在无需人类协助的情况下运行，其速度比最快的人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
