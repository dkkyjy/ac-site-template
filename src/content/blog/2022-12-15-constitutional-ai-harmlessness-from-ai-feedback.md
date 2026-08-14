---
title: "宪法式AI：通过AI反馈实现无害性"
description: "宪法式AI：通过AI反馈实现无害性"
pubDate: "2022-12-15"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/constitutional-ai-harmlessness-from-ai-feedback"
---

# 宪法式AI：通过AI反馈实现无害性
**标签：** 对齐，研究

---

[阅读论文](https://arxiv.org/abs/2212.08073)

## 摘要

随着AI系统能力日益增强，我们希望借助它们来监督其他AI。我们实验了通过自我改进来训练无害AI助手的方法，无需任何人工标注有害输出。唯一的人工监督是通过一系列规则或原则提供的，因此我们将该方法称为"宪法式AI"。该过程包括监督学习和强化学习两个阶段。在监督阶段，我们从初始模型中采样，生成自我批评和修订，然后在修订后的回答上微调原始模型。在强化学习阶段，我们从微调后的模型中采样，使用一个模型评估两个样本中哪个更好，然后从这一AI偏好数据集训练偏好模型。随后我们使用强化学习进行训练，以偏好模型作为奖励信号，即我们使用"来自AI反馈的强化学习"。最终，我们能够训练出一个无害但不回避问题的AI助手，它通过解释对有害查询的反对意见来应对这些查询。监督学习和强化学习方法都可以利用思维链式推理来提高人类评判的AI决策表现和透明度。这些方法使得更精确地控制AI行为成为可能，并且所需的人工标注数量大大减少。

## 政策备忘录

[宪法式AI政策备忘录](https://www-cdn.anthropic.com/7512771452629584566b6303311496c262da1006/Anthropic_ConstitutionalAI_v2.pdf)

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个涌现的心理工作空间，它持有不出现在模型输出中的内部思维。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，询问：人们何时使用Claude？他们用它产生什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了关于Claude能否帮助Anthropic员工执行复杂机器人任务的最新测试结果。我们发现，Claude Opus 4.7在无人协助的情况下运行，完成所有任务的速度比不到一年前参与者中完成所有任务的最快人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
