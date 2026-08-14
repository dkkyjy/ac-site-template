---
title: "大型语言模型中的道德自我修正能力"
description: "大型语言模型中的道德自我修正能力"
pubDate: "2023-02-15"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/the-capacity-for-moral-self-correction-in-large-language-models"
---

# 大型语言模型中的道德自我修正能力
**标签：** 社会影响

---

[阅读论文](https://arxiv.org/abs/2302.07459)

## 摘要

我们测试了以下假设：经过人类反馈强化学习（RLHF）训练的语言模型，在被指示这样做时，具备“道德自我修正”能力——即避免产生有害输出。我们在三个不同的实验中找到了支持这一假设的有力证据，每个实验都揭示了道德自我修正的不同方面。我们发现，道德自我修正能力在模型参数达到220亿时出现，并且通常随着模型规模增大和RLHF训练程度的提高而改善。我们认为，在这一规模水平上，语言模型获得了可用于道德自我修正的两种能力：（1）它们能够遵循指令；（2）它们能够学习关于伤害的复杂规范性概念，如刻板印象、偏见和歧视。因此，它们可以遵循指令以避免某些类型的有害道德输出。我们相信，我们的结果使我们有理由对训练语言模型遵守伦理原则的能力持谨慎乐观态度。

## 政策备忘录

[道德自我修正政策备忘录](https://www-cdn.anthropic.com/d14f58fe8f611858bc37ff8686bbda71e4927ce6/Anthropic_MoralSelfCorrection_v3.pdf)

## 相关内容

### 语言模型中的全局工作空间

一项新的可解释性研究揭示了Claude中存在一种涌现的心理工作空间，其中容纳了不显示在模型输出中的内部思考。

[了解更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，提出以下问题：人们何时会使用Claude？他们用它来做什么？他们如何看待人工智能对其工作的影响？

[了解更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了最新测试的结果，测试内容为Claude能否帮助Anthropic员工执行复杂的机器人任务。我们发现，在不借助人工辅助的情况下，Claude Opus 4.7在所有任务的完成速度上，比不到一年前参与实验的最快人类团队快约20倍。

[了解更多](/research/project-fetch-phase-two)
