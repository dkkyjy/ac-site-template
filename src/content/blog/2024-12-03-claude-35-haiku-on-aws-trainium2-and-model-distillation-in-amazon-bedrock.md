---
title: "Claude 3.5 Haiku 在 AWS Trainium2 上运行及 Amazon Bedrock 中的模型蒸馏"
description: "作为我们与 AWS 扩大[合作](https://www.anthropic.com/news/anthropic-amazon-trainium)的一部分，我们已开始优化 Claude 模型，使其能够在 AWS 最先进的 AI 芯片 [AWS Trainium2](https://aws.amazon.com/ai/machine-learning/trainium/) 上运行。"
pubDate: "2024-12-03"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/trainium2-and-distillation"
---
            作为我们与 AWS 扩大[合作](https://www.anthropic.com/news/anthropic-amazon-trainium)的一部分，我们已开始优化 Claude 模型，使其能够在 AWS 最先进的 AI 芯片 [AWS Trainium2](https://aws.amazon.com/ai/machine-learning/trainium/) 上运行。

为了预览 Trainium2 的能力，Claude 3.5 Haiku 现已支持在 [Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 中进行延迟优化的推理，使模型速度显著提升，同时不牺牲准确性。

我们还在 Amazon Bedrock 中增加了对模型蒸馏的支持，将更大 Claude 模型的智能能力引入我们更快、更具成本效益的模型中。

### Trainium2 上的下一代模型

我们正与 AWS 合作构建 Project Rainier——一个包含数十万颗 Trainium2 芯片的 Trn2 UltraServer EC2 超级集群。该集群将提供超过我们当前训练领先 AI 模型所用算力五倍（以 exaflops 计）的计算能力。

Trainium2 使我们能够在 Amazon Bedrock 中提供更快的模型，首先从 Claude 3.5 Haiku 开始，该模型现已支持延迟优化的推理（公开预览）。通过启用延迟优化，Claude 3.5 Haiku 可提供高达 60% 的推理速度提升——使其成为从代码补全到实时内容审核和聊天机器人等各种用例的理想选择。

由 Trainium2 驱动的更快版 Claude 3.5 Haiku 已在美国东部（俄亥俄）区域通过[跨区域推理](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)提供，价格为每百万输入 token 1 美元，每百万输出 token 5 美元。

### Amazon Bedrock 模型蒸馏

我们还使客户能够从 Claude 3 Haiku（我们上一代最具成本效益的模型）中获得前沿性能。通过蒸馏，Claude 3 Haiku 现在可以实现显著的性能提升，在特定任务上达到类似 Claude 3.5 Sonnet 的准确性——同时保持我们最具成本效益模型的价格和速度。

这项技术将知识从"教师"模型（Claude 3.5 Sonnet）转移到"学生"模型（Claude 3 Haiku），使客户能够以极低的成本运行检索增强生成（RAG）和数据分析等复杂任务。

与需要开发者手动构建训练示例并持续调整参数的传统微调不同，Amazon Bedrock 模型蒸馏通过以下方式自动化整个流程：

1. **从 Claude 3.5 Sonnet 生成合成训练数据**
2. **训练和评估** Claude 3 Haiku
3. **托管**最终蒸馏模型用于推理

Amazon Bedrock 模型蒸馏会自动应用不同的数据合成方法——从生成相似提示到根据您的示例提示-响应对创建新的高质量响应。

Amazon Bedrock 中 Claude 3 Haiku 的蒸馏功能现已提供预览。了解更多信息，请访问 AWS [发布博客](https://aws.amazon.com/blogs/aws/build-faster-more-cost-efficient-highly-accurate-models-with-amazon-bedrock-model-distillation-preview/)和[文档](https://docs.aws.amazon.com/bedrock/latest/userguide/model-distillation.html)。

### Claude 3.5 Haiku 降价

除了在 Trainium2 上提供更快的版本外，客户仍可继续通过 [Anthropic API](https://console.anthropic.com/workbench)、[Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 和 [Google Cloud 的 Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) 访问 [Claude 3.5 Haiku](https://www.anthropic.com/claude/haiku)。

为了使该模型更广泛地适用于各种用例，我们正在降低 Claude 3.5 Haiku 的价格，在所有平台上调整为每百万输入 token 0.80 美元，每百万输出 token 4 美元。

### 开始使用

从今天起，模型蒸馏和更快的 Claude 3.5 Haiku 已在 Amazon Bedrock 中提供预览。对于寻求价格、性能和速度最佳平衡的开发者，您现在可以通过 Claude 获得更多模型选择：

* 由 Trainium2 驱动的延迟优化版 Claude 3.5 Haiku，适用于通用用例
* 具有前沿性能蒸馏版的 Claude 3 Haiku，适用于高容量、重复性用例

要开始使用，请访问 [Amazon Bedrock 控制台](https://signin.aws.amazon.com/signup?request_type=register)。我们期待看到您的构建成果。
