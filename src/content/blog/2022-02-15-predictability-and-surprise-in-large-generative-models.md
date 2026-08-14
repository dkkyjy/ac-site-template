---
title: "大型生成模型中的可预测性与意外性"
description: "大型生成模型中的可预测性与意外性"
pubDate: "2022-02-15"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/predictability-and-surprise-in-large-generative-models"
---

# 大型生成模型中的可预测性与意外性  
**标签：** 社会影响  

---

[阅读论文](https://arxiv.org/abs/2202.07785)

## 摘要

大规模预训练近年来已成为一种创建通用、多功能生成模型（如GPT-3、Megatron-Turing NLG、Gopher等）的技术。在本文中，我们强调此类模型的一个反直觉特性，并讨论该特性的政策影响。具体而言，这些生成模型具有一种不同寻常的组合：在大范围训练分布上可预测的损失（体现在其“缩放定律”中），以及不可预测的特定能力、输入和输出。我们认为，高层级的可预测性及有用能力的表象推动了此类模型的快速发展，而不可预测的特性则使得难以预见模型部署的后果。我们通过文献和实际观察中的例子，展示了这种组合如何导致对社会有害的行为，并进行了两项新颖的实验来阐明我们关于不可预测性危害的观点。此外，我们分析了这些相互冲突的特性如何结合，为模型开发者提供部署这些模型的各种动机，以及可能阻碍部署的挑战。最后，我们列出了一些可能干预措施，人工智能社区可以采取这些措施来增加这些模型产生有益影响的机会。我们希望本文对想要理解和监管AI系统的政策制定者、关心其工作潜在政策影响的技术专家，以及希望分析、批判并可能开发大型生成模型的学者有所帮助。

### 政策备忘录

[可预测性与意外性备忘录](https://www-cdn.anthropic.com/4ff80d7f8a98bf096cd543ec61ddc50de3ad8b16/Anthropic_PredictabilityAndSurprise.pdf)

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示，Claude中存在一个涌现的心理工作空间，用于容纳不体现在模型输出中的内部思维。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时抽样，询问人们何时使用Claude？他们用它创造什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了最新测试的结果，该测试考察Claude能否帮助Anthropic员工执行复杂的机器人任务。我们发现，Claude Opus 4.7在无人协助的情况下运行，其完成所有任务的速度比不到一年前参赛者中的最快人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
