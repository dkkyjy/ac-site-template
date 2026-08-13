---
title: "通过升级版 Anthropic Console 更快投入生产"
description: "我们重新设计了 Anthropic Console，使其成为您与 Claude 和团队成员一起构建、测试和迭代 AI 部署的一站式平台。"
pubDate: "2025-03-06"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/upgraded-anthropic-console"
---
            我们重新设计了 Anthropic Console，使其成为您与 Claude 和团队成员一起构建、测试和迭代 AI 部署的一站式平台。

开发者现在可以直接在 [Anthropic Console](https://console.anthropic.com/) 中共享提示词，以便与团队成员协作。我们还增加了对我们最新模型 Claude 3.7 Sonnet 的支持，以及控制扩展思考预算的能力。

### 构建可靠的 AI 应用

提示词质量在很大程度上决定了模型对特定任务的响应成功率。Anthropic Console 已经通过编写、评估和优化提示词的工具，简化了 AI 开发流程。

**编写提示词：** 使用工作台有效构建提示词结构，融入示例，并集成外部工具，在交互式环境中测试 API 调用。

**自动生成提示词：** 描述您想要实现的目标，Claude 将使用链式思维推理等提示词工程技术，创建有效、精确且可靠的提示词。

**评估模型响应：** 通过自动生成测试用例和并排输出比较，评估提示词在真实场景中的表现。轻松运行测试套件，对响应质量进行评分，并基于数据做出部署哪些提示词的决策。

**改进提示词：** 使用高级提示词工程技术自动优化提示词。这非常适合适配最初为其他 AI 模型编写的提示词，以及优化手写提示词。

确定提示词后，您可以点击"获取代码"，获得一个可直接部署的生产级 API 调用。

### 通过可共享提示词进行协作

开发者、领域专家、产品经理和 QA 专家通常需要协作编写提示词以获得最佳结果。以往，团队只能通过复制粘贴的方式在文档或聊天应用之间传递提示词，导致版本控制问题和知识孤岛。

Anthropic Console 中的可共享提示词现在提供了一种集中化的方式，用于在整个组织内开发、优化和标准化提示词。团队成员可以访问并协作共享提示词库，从而更容易建立最佳实践，并确保所有基于 Claude 的应用保持一致的品质。

### 优化提示词以支持扩展思考

[Claude 3.7 Sonnet](https://www.anthropic.com/news/claude-3-7-sonnet) 是我们最新、最智能的模型，能够生成近乎即时的响应，或向用户展示可见的、逐步扩展的思考过程。

虽然启用扩展思考后提示词的基本工作方式保持不变，但我们简化了优化提示词的流程，以充分利用这一新能力。只需指定该提示词将在启用扩展思考的情况下使用，Claude 就会生成最佳响应。您还可以使用 Anthropic Console 控制思考预算，设置最大思考令牌数。

### 开始使用

升级版 Anthropic Console 现已对所有用户开放。[登录](https://console.anthropic.com/) 即可开始使用，或探索我们的[文档](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)了解更多信息。
