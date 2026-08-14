---
title: "问题分解提升了模型生成推理的忠实性"
description: "问题分解提升了模型生成推理的忠实性"
pubDate: "2023-07-18"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/question-decomposition-improves-the-faithfulness-of-model-generated-reasoning"
---

# 问题分解提升了模型生成推理的忠实性  
**标签：** 对齐，研究

---

[下载论文](https://www-cdn.anthropic.com/8154fb1d828cdc390dc1fa442d84034948679c47/question-decomposition-improves-the-faithfulness-of-model-generated-reasoning.pdf)

## 摘要

随着大型语言模型（LLM）执行越来越困难的任务，验证其行为正确性和安全性的难度也随之增加。解决该问题的一种方法是提示LLM外化其推理过程，例如让模型在回答问题时生成逐步推理（思维链，CoT）。这种推理使我们能够检验模型完成任务所使用的过程。然而，这种方法依赖于陈述的推理忠实反映模型的实际推理，但情况并非总是如此。为了提升CoT推理的忠实性，我们让模型通过将问题分解为子问题来生成推理。基于分解的方法在问答任务上取得了强劲的性能，有时接近CoT的水平，同时在几个最近提出的指标上提高了模型陈述推理的忠实性。通过迫使模型在独立的上下文中回答更简单的子问题，我们大大提升了模型生成推理相对于CoT的忠实性，同时仍然实现了CoT的部分性能提升。我们的结果表明，提升模型生成推理的忠实性是可行的；持续改进有望带来能够使我们验证LLM行为正确性和安全性的推理。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中存在一个涌现的心理工作空间，该空间持有不在模型输出中出现的内部思考。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们的最新经济指数报告中，我们首次按小时采样，提出以下问题：人们什么时候使用Claude？他们用它来产出什么？他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### “Fetch项目”第二阶段

我们报告了最新测试的结果，该测试检验Claude能否帮助Anthropic员工执行复杂的机器人任务。我们发现，在不到一年前由参与者完成的所有任务中，无需人类协助的Claude Opus 4.7比最快的人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
