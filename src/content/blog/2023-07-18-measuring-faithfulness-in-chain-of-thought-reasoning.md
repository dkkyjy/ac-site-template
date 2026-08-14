---
title: "衡量思维链推理中的忠实性"
description: "衡量思维链推理中的忠实性"
pubDate: "2023-07-18"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/measuring-faithfulness-in-chain-of-thought-reasoning"
---

# 衡量思维链推理中的忠实性  
**标签：** 对齐，研究

---

[下载论文](https://www-cdn.anthropic.com/827afa7dd36e4afbb1a49c735bfbb2c69749756e/measuring-faithfulness-in-chain-of-thought-reasoning.pdf)

## 摘要

大型语言模型（LLM）在回答问题前生成逐步的“思维链”（Chain-of-Thought, CoT）推理时表现更好，但目前尚不清楚所陈述的推理是否忠实于模型的实际推理过程（即其回答问题的过程）。我们研究了CoT推理可能不忠实的几种假设，方法是对CoT进行干预（例如添加错误或改写后）观察模型预测的变化。模型在不同任务中对CoT的依赖程度差异很大，有时高度依赖CoT，有时则基本忽略它。CoT带来的性能提升似乎并非仅仅来自CoT增加的测试时计算量，也非来自CoT特定措辞所编码的信息。随着模型规模增大、能力增强，在我们研究的大多数任务中，模型产生的推理忠实性反而降低。总体而言，我们的结果表明，如果仔细选择模型大小和任务等条件，CoT可以是忠实的。

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了Claude中存在一个涌现的心理工作空间，它持有模型输出中未呈现的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时抽样以探究：人们何时使用Claude？他们用它来做什么？他们如何感知AI对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### 项目Fetch：第二阶段

我们报告了最新测试结果，测试Claude能否帮助Anthropic员工执行复杂的机器人任务。我们发现，Claude Opus 4.7在无需人工协助的情况下，完成参与者不到一年前完成的所有任务的速度大约是人工最快团队的20倍。

[阅读更多](/research/project-fetch-phase-two)
