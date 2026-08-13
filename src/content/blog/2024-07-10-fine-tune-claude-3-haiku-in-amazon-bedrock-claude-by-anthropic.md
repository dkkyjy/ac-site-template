---
title: "在 Amazon Bedrock 中微调 Claude 3 Haiku | Anthropic 的 Claude"
description: "---"
pubDate: "2024-07-10"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/fine-tune-claude-3-haiku"
---

# 在 Amazon Bedrock 中微调 Claude 3 Haiku | Anthropic 的 Claude

---

***更新：*** *在 Amazon Bedrock 中微调 Claude 3 Haiku 现已正式可用。（2024 年 11 月 1 日）*

客户现在可以在 [Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 中微调 Claude 3 Haiku——这是我们最快且最具成本效益的模型——以针对其业务定制其知识和能力，使该模型在 specialized 任务上更加高效。

## 微调概述

微调是一种广受欢迎的提升模型性能的技术。通过创建模型的定制版本，您可以训练模型在高度定制的工作流程中表现出色。

要微调 Claude 3 Haiku，您首先需要准备一组高质量的提示-完成对——即您希望 Claude 在给定任务下提供的理想输出。微调 API 现已提供预览版，将使用您的数据创建您自己的定制 Claude 3 Haiku。通过 Amazon Bedrock 控制台或 API，您可以测试并优化您的定制 Claude 3 Haiku 模型，直到其满足您的性能目标并准备好部署。

## 优势

微调允许您定制 Claude 3 Haiku，使其能够获取 specialized 的业务知识，从而提高准确性和一致性。优势包括：

* **在 specialized 任务上获得更佳结果**：提升领域特定操作（如分类、与定制 API 交互或行业特定数据解读）的性能。与更通用的模型相比，微调使 Claude 3 Haiku 能够通过编码公司和领域知识，在您的业务关键领域表现出色。
* **更快的速度，更低的成本**：降低生产部署的成本，因为 Claude 3 Haiku 可以替代 Sonnet 或 Opus，同时还能更快地返回结果。
* **一致的、符合品牌风格的输出格式**：生成按照您的精确规范定制的结构一致的输出，例如标准报告或定制模式，确保符合监管要求和内部规范。
* **易于使用的 API**：各种规模的企业都可以高效创新，无需广泛的内部 AI 专业知识或资源。任何人都可以无缝地微调模型，无需深厚的技术专业知识。
* **安全可靠**：专有训练数据保留在客户的 AWS 环境中。Anthropic 的微调技术保持了 Claude 3 模型系列的低有害输出风险。

我们对 Haiku 进行了微调，用于审核互联网论坛上的在线评论，包括识别侮辱、威胁和露骨内容。微调将分类准确率从 81.5% 提高到 99.6%，同时将每个查询的 token 数减少了 85%。

## 客户亮点

[SK Telecom](https://www.claude.com/customers/skt) 是韩国最大的电信运营商之一，他们通过利用行业特定专业知识，训练了一个定制的 Claude 模型，以改进支持工作流程并提升客户体验。

“将微调后的 Claude 嵌入我们的客户支持操作，已经显著改善了我们内部流程和整体客户满意度。**通过定制 Claude，我们代理回复的正面反馈提升了 73%，电信相关任务的关键绩效指标提升了 37%**。微调后的模型现在可以高效地从客户通话记录中生成主题、行动项和摘要，并将复杂的客户问题分解为可管理的步骤，以便更好地解决问题，”SK Telecom 人工智能技术合作集团副总裁 Eric Davis 表示。

[Thomson Reuters](https://www.claude.com/customers/thomson-reuters) 是一家全球性的内容和科技公司，在 Claude 3 Haiku 上看到了积极的结果。该公司服务于法律、税务、会计、合规、政府和媒体领域的专业人士，预期通过利用其行业专业知识微调 Claude，获得更快、更相关的 AI 结果。

“我们很高兴能在 Amazon Bedrock 中微调 Anthropic 的 Claude 3 Haiku 模型，以进一步增强我们基于 Claude 的解决方案。Thomson Reuters 旨在提供准确、快速且一致的用户体验。通过围绕我们的行业专业知识和特定需求优化 Claude，我们预期获得可衡量的改进，以更快的速度交付高质量结果。**我们已经看到了 Claude 3 Haiku 的积极成果，微调将使我们能够更精确地定制 AI 辅助功能**，”Thomson Reuters 人工智能与实验室负责人 Joel Hron 表示。

## 如何开始

在 Amazon Bedrock 中对 Claude 3 Haiku 进行微调现已在美国西部（俄勒冈）AWS 区域提供预览。启动时，我们支持基于文本的微调，上下文长度最高可达 32K token，并计划在未来引入视觉能力。更多详情请参阅 [AWS 启动博客](https://aws.amazon.com/blogs/machine-learning/fine-tune-anthropics-claude-3-haiku-in-amazon-bedrock-to-boost-model-accuracy-and-quality/) 和 [文档](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-model-supported.html)。

如需申请访问权限，请联系您的 AWS 账户团队，或在 [AWS 管理控制台](https://console.aws.amazon.com/bedrock/) 中提交支持工单。
