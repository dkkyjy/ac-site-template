---
title: "开源电路追踪工具"
description: "开源电路追踪工具"
pubDate: "2025-05-29"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/open-source-circuit-tracing"
---

# 开源电路追踪工具  
**标签：** 可解释性

---

在我们最近的可解释性研究中，我们引入了一种新方法来[追踪大型语言模型的思维](https://www.anthropic.com/research/tracing-thoughts-language-model)。今天，我们将该方法开源，以便任何人都可以在我们研究的基础上继续推进。

我们的方法是生成*归因图*，这些图（部分）揭示了模型内部为决定特定输出而采取的步骤。我们发布的开源[库](https://github.com/safety-research/circuit-tracer)支持在流行的开放权重模型上生成归因图——同时由 Neuronpedia 托管的前端让你可以交互式地探索这些图。

本项目由我们[Anthropic Fellows](https://alignment.anthropic.com/2024/anthropic-fellows-program/)项目的参与者主导，并与[Decode Research](https://www.decoderesearch.org/)合作完成。

Neuronpedia 上交互式图表探索器 UI 的概览。

要开始使用，你可以访问[Neuronpedia 界面](https://www.neuronpedia.org/gemma-2-2b/graph)为你选择的提示生成并查看你自己的归因图。对于更高级的使用和研究，你可以查看[代码仓库](https://github.com/safety-research/circuit-tracer)。此次发布使研究人员能够：

1. **追踪电路**——在支持的模型上生成自己的归因图；
2. **可视化、注释和分享**——在交互式前端中完成；
3. **测试假设**——通过修改特征值并观察模型输出如何变化。

我们已经使用这些工具研究了 Gemma-2-2b 和 Llama-3.2-1b 中有趣的行为，如多步推理和多语言表示——请参阅我们的演示[笔记本](https://github.com/safety-research/circuit-tracer/blob/main/demos/circuit_tracing_tutorial.ipynb)获取示例和分析。我们也邀请社区帮助我们找到更多有趣的电路——作为启发，我们在演示笔记本和 Neuronpedia 上提供了我们尚未分析的额外归因图。

我们的 CEO Dario Amodei[最近撰文](https://www.darioamodei.com/post/the-urgency-of-interpretability)谈到可解释性研究的紧迫性：目前，我们对人工智能内部运作的理解远远落后于我们在人工智能能力方面取得的进展。通过开源这些工具，我们希望让更广泛的社区更容易研究语言模型内部发生的事情。我们期待看到这些工具在研究模型行为方面的应用，以及改进工具本身的扩展。

*开源电路查找库由[Anthropic Fellows](https://alignment.anthropic.com/2024/anthropic-fellows-program/) Michael Hanna 和 Mateusz Piotrowski 开发，并在 Emmanuel Ameisen 和 Jack Lindsey 的指导下完成。Neuronpedia 集成由[Decode Research](https://www.decoderesearch.org/)实现（Neuronpedia 负责人：Johnny Lin；科学负责人/主任：Curt Tigges）。我们的 Gemma 图基于[GemmaScope](https://ai.google.dev/gemma/docs/gemma_scope)项目训练的超编码器。如有问题或反馈，请在 GitHub 上提交 issue。*

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了 Claude 中一个涌现的心理工作空间，该空间持有模型输出中不会出现的内部思维。

[阅读更多](/research/global-workspace)

### Anthropic 经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，以探究：人们何时来到 Claude？他们用它生产什么？以及他们如何看待人工智能对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### Project Fetch：第二阶段

我们报告了关于 Claude 能否帮助 Anthropic 员工执行复杂机器人任务的最新测试结果。我们发现，Claude Opus 4.7 在无需人工协助的情况下，在不到一年前参与者完成的所有任务中，速度比最快的人类团队快约 20 倍。

[阅读更多](/research/project-fetch-phase-two)
