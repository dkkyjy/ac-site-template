---
title: "Softmax 线性单元"
description: "Softmax 线性单元"
pubDate: "2022-06-17"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/softmax-linear-units"
---

# Softmax 线性单元  
**标签：** 可解释性，研究  

---

[阅读论文](https://transformer-circuits.pub/2022/solu/index.html)

## 摘要

在本文中，我们报告了一项架构变更，该变更似乎能够显著提高MLP神经元中“可解释”（即对输入的某种可表述属性做出响应）的神经元比例，且几乎没有性能损失。具体而言，我们将激活函数替换为Softmax线性单元（我们称之为SoLU），并表明这一替换显著增加了MLP层中对快速调查而言似乎对应着易于人类理解的概念、短语或类别的神经元比例——这一点通过随机化盲测实验得到验证。随后，我们研究了SoLU模型，并利用它们获得了关于Transformer信息处理方式的一些新见解。然而，我们也发现了一些证据表明叠加假设成立，且没有免费的午餐：SoLU可能通过“隐藏”其他特征使部分特征更可解释，从而让被隐藏的特征变得更加深不可测。尽管如此，SoLU似乎仍然是一个净收益，因为在实际应用中，它显著提高了我们能够理解的神经元比例。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中存在一个涌现的心智工作空间，其中容纳了未出现在模型输出中的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时抽样，以提问：人们何时使用Claude？他们用Claude生成什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了最新测试的结果，该测试旨在评估Claude能否帮助Anthropic员工完成复杂的机器人任务。我们发现，Claude Opus 4.7在无人协助的情况下完成所有任务的速度，比不到一年前参与者的最快人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
