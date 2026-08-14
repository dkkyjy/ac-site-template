---
title: "训练一个有用且无害的助手：基于人类反馈的强化学习"
description: "训练一个有用且无害的助手：基于人类反馈的强化学习"
pubDate: "2022-04-12"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/training-a-helpful-and-harmless-assistant-with-reinforcement-learning-from-human-feedback"
---

# 训练一个有用且无害的助手：基于人类反馈的强化学习  
**标签：** 对齐，研究  

---

[阅读论文](https://arxiv.org/abs/2204.05862)

## 摘要

我们应用偏好建模和基于人类反馈的强化学习（RLHF）对语言模型进行微调，使其成为有用且无害的助手。我们发现，这种对齐训练在几乎所有 NLP 评估中都提升了性能，并且与 Python 编程和摘要生成等专业技能的训练完全兼容。我们探索了一种迭代式的在线训练模式，其中偏好模型和 RL 策略每周根据新的人类反馈数据进行更新，从而高效地改进我们的数据集和模型。最后，我们研究了 RLHF 训练的鲁棒性，并发现 RL 奖励与策略及其初始化之间的 KL 散度的平方根大致呈线性关系。除了主要结果外，我们还进行了关于校准、竞争目标和 OOD 检测的辅助分析，将我们的模型与人类写作者进行比较，并提供了使用近期相关工作提示的模型输出样本。

## 作者

Yuntao Bai, Andy Jones, Kamal Ndousse, Amanda Askell, Anna Chen, Nova DasSarma, Dawn Drain, Stanislav Fort, Deep Ganguli, Tom Henighan, Nicholas Joseph, Saurav Kadavath, Jackson Kernion, Tom Conerly, Sheer El-Showk, Nelson Elhage, Zac Hatfield-Dodds, Danny Hernandez, Tristan Hume, Scott Johnston, Shauna Kravec, Liane Lovitt, Neel Nanda, Catherine Olsson, Dario Amodei, Tom Brown, Jack Clark, Sam McCandlish, Chris Olah, Ben Mann, Jared Kaplan

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了 Claude 中一个突现的心理工作空间，该空间容纳了模型中未出现在输出中的内部想法。

[了解更多](/research/global-workspace)

### Anthropic 经济指数报告：节奏

在我们的最新经济指数报告中，我们首次按小时采样，提出以下问题：人们何时使用 Claude？他们用它来生成什么？他们如何看待人工智能对其工作的影响？

[了解更多](/research/economic-index-june-2026-report)

### 项目 Fetch：第二阶段

我们报告了 Claude 能否帮助 Anthropic 员工执行复杂机器人任务的最新测试结果。我们发现，Claude Opus 4.7 在无需人类协助的情况下，完成所有任务的速度比不到一年前参与测试的最快人类团队快了约 20 倍。

[了解更多](/research/project-fetch-phase-two)
