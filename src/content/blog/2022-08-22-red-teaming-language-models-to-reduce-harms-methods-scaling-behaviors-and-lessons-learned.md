---
title: "红队测试语言模型以减少危害：方法、扩展行为与经验教训"
description: "红队测试语言模型以减少危害：方法、扩展行为与经验教训"
pubDate: "2022-08-22"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: ""
---

# 红队测试语言模型以减少危害：方法、扩展行为与经验教训

**日期:** 2022-08-22 00:00 UTC  
**链接:** https://www.anthropic.com/research/red-teaming-language-models-to-reduce-harms-methods-scaling-behaviors-and-lessons-learned  
**标签:** 社会影响  

---

[阅读论文](https://arxiv.org/abs/2209.07858)

## 摘要

我们描述了早期红队测试语言模型的努力，旨在同时发现、衡量并尝试减少其潜在的有害输出。我们做出了三项主要贡献。首先，我们研究了红队测试在3种模型规模（2.7B、13B和52B参数）和4种模型类型中的扩展行为：普通语言模型（LM）；被提示为乐于助人、诚实且无害的LM；使用拒绝采样的LM；以及通过人类反馈强化学习（RLHF）训练而成的乐于助人且无害的模型。我们发现，RLHF模型随着规模扩大越来越难以被红队测试，而其他模型类型则呈现出平坦趋势。其次，我们发布了包含38,961次红队攻击的数据集，供他人分析和学习。我们提供了自己的数据分析，发现了各种有害输出，从攻击性语言到更微妙的非暴力不道德输出。第三，我们详尽描述了我们的指令、流程、统计方法以及在红队测试中的不确定性。我们希望这种透明度能够加速我们作为一个社区共同努力，为如何对语言模型进行红队测试制定共同规范、实践和技术标准。

## 政策备忘录

[红队测试政策备忘录](https://www-cdn.anthropic.com/82564d4ec2451b2eed2e0796b7c658fc989f0c1a/Anthropic_RedTeaming.pdf)

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中一个涌现的心理工作空间，它持有模型输出中不出现的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节律

在我们最新的经济指数报告中，我们首次按小时采样，提出以下问题：人们何时使用Claude？他们用它生成了什么？以及他们如何看待AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### Project Fetch：第二阶段

我们报告了最新测试结果，测试Claude是否能够帮助Anthropic员工完成复杂的机器人任务。我们发现，在不到一年前参与者完成的所有任务中，无需人工协助的Claude Opus 4.7比最快的人类团队快约20倍。

[阅读更多](/research/project-fetch-phase-two)
