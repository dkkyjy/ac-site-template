---
title: "在 Amazon Bedrock 中微调 Claude 3 Haiku"
description: "客户现在可以在 [Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 中微调 Claude 3 Haiku——我们最快且最具成本效益的模型——以根据其业务定制其知识和能力，使该模型在专业化任务中更加有效。"
pubDate: "2024-07-10"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/fine-tune-claude-3-haiku"
---
            ***更新：*** *在 Amazon Bedrock 中微调 Claude 3 Haiku 现已全面可用。（2024年11月1日）*

客户现在可以在 [Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 中微调 Claude 3 Haiku——我们最快且最具成本效益的模型——以根据其业务定制其知识和能力，使该模型在专业化任务中更加有效。

## 微调概述

微调是一种提高模型性能的流行技术。通过创建模型的定制版本，您可以训练模型在高度定制的工作流程中表现出色。

要微调 Claude 3 Haiku，您首先需要准备一组高质量的提示-完成对——即您希望 Claude 针对给定任务提供的理想输出。微调 API 现已在预览版中提供，将使用您的数据创建您自己的定制 Claude 3 Haiku。使用 Amazon Bedrock 控制台或 API，您可以测试和优化您的定制 Claude 3 Haiku 模型，直到其满足您的性能目标并准备好部署。

## 优势

微调允许您定制 Claude 3 Haiku，使其能够获取专业业务知识，从而提高准确性和一致性。优势包括：

* **在专业任务上取得更佳结果**：提升领域特定操作的性能，例如分类、与自定义 API 的交互或行业特定数据解释。通过编码公司和领域知识，与更通用的模型相比，微调使 Claude 3 Haiku 能够在对您的业务至关重要的领域表现出色。
* **以更低成本实现更快速度**：降低生产部署的成本，使 Claude 3 Haiku 能够替代 Sonnet 或 Opus，同时更快地返回结果。
* **一致且符合品牌形象的格式**：生成完全符合您确切规格的一致结构化输出，例如标准化报告或自定义模式，确保符合监管要求和内部协议。
* **易于使用的 API**：各种规模的公司都可以高效创新，无需广泛的内部 AI 专业知识或资源。任何人都可以无缝微调模型，无需深厚的技术专业知识。
* **安全可靠**：专有训练数据保留在客户的 AWS 环境内。Anthropic 的微调技术保留了 Claude 3 模型系列的低有害输出风险。

我们微调了 Haiku 以审核互联网论坛上的在线评论¹，包括识别侮辱、威胁和露骨内容。微调将分类准确率从 81.5% 提高到 99.6%，同时将每个查询的令牌数减少了 85%。

## 客户聚焦

[SK Telecom](https://www.claude.com/customers/skt) 是韩国最大的电信运营商之一，通过利用其行业特定专业知识，训练了一个定制的 Claude 模型以改进支持工作流程并实现更好的客户体验。

"将微调后的 Claude 嵌入我们的客户支持运营中，可衡量地改进了我们的内部流程和整体客户满意度。**通过定制 Claude，我们看到代理回复的正面反馈增加了 73%，电信相关任务的关键绩效指标提高了 37%**。微调后的模型现在可以高效地从客户通话记录中生成主题、行动项目和摘要，并将复杂的客户问题分解为可管理的步骤以实现更好的问题解决，"AI 技术协作集团副总裁 Eric Davis 表示。

[Thomson Reuters](https://www.claude.com/customers/thomson-reuters) 是一家全球内容和科技公司，在使用 Claude 3 Haiku 方面取得了积极成果。这家为法律、税务、会计、合规、政府和媒体领域的专业人士提供服务的公司，预计通过利用其行业专业知识微调 Claude，将获得更快、更相关的 AI 结果。

"我们很高兴能在 Amazon Bedrock 中微调 Anthropic 的 Claude 3 Haiku 模型，以进一步增强我们基于 Claude 的解决方案。Thomson Reuters 旨在提供准确、快速且一致的用户体验。通过围绕我们的行业专业知识和特定要求优化 Claude，我们预计将实现可衡量的改进，以更快的速度提供高质量的结果。**我们已经看到 Claude 3 Haiku 的积极成果，微调将使我们能够更精确地定制 AI 辅助功能**，"Thomson Reuters AI 与实验室负责人 Joel Hron 表示。

## 如何开始

在 Amazon Bedrock 中微调 Claude 3 Haiku 现已在美西（俄勒冈）AWS 区域提供预览版。在发布时，我们支持基于文本的微调，上下文长度最高可达 32K 令牌，并计划在未来引入视觉功能。更多详情请参阅 [AWS 发布博客](https://aws.amazon.com/blogs/machine-learning/fine-tune-anthropics-claude-3-haiku-in-amazon-bedrock-to-boost-model-accuracy-and-quality/) 和 [文档](https://docs.aws.amazon.com/bedrock/latest/userguide/custom-model-supported.html)。

要请求访问权限，请联系您的 AWS 账户团队或在 [AWS 管理控制台](https://console.aws.amazon.com/bedrock/) 中提交支持工单。
