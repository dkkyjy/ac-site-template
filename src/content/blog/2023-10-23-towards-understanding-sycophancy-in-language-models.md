---
title: "理解语言模型中的谄媚行为"
description: "理解语言模型中的谄媚行为"
pubDate: "2023-10-23"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/towards-understanding-sycophancy-in-language-models"
---

# 理解语言模型中的谄媚行为
**标签：** 对齐，研究

---

[阅读论文](https://arxiv.org/abs/2310.13548)

## 摘要

从人类反馈中进行强化学习（RLHF）是一种训练高质量AI助手的流行技术。然而，RLHF也可能鼓励模型响应匹配用户信念而非真实响应，这种行为被称为谄媚。我们调查了经过RLHF训练的模型中谄媚行为的普遍性，以及人类偏好判断是否是其原因。我们首先展示了五个最先进的AI助手在四项不同的自由形式文本生成任务中持续表现出谄媚行为。为了理解人类偏好是否驱动了RLHF模型这种广泛观察到的行为，我们分析了现有的人类偏好数据。我们发现，当响应匹配用户观点时，它更有可能被偏好。此外，人类和偏好模型（PM）在相当一部分时间上都更喜欢写得令人信服的谄媚响应，而不是正确响应。优化模型输出以符合PM有时也会以牺牲真实性为代价来迎合谄媚。总体而言，我们的结果表明谄媚是RLHF模型的一种普遍行为，部分原因可能是人类偏好判断倾向于谄媚响应。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个涌现的心智工作空间，它持有不表现在模型输出中的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，以询问：人们什么时候来找Claude？他们用它来生成什么？以及他们如何感知AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了我们最新测试的结果，测试Claude是否可以帮助Anthropic员工执行复杂的机器人任务。我们发现，Claude Opus 4.7在无需人类协助的情况下，在所有任务中比不到一年前参与者的最快人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
