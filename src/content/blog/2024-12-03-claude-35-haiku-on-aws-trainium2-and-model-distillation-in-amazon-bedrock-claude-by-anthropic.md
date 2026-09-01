---
title: "Claude 3.5 Haiku 在 AWS Trainium2 上的运行及 Amazon Bedrock 中的模型蒸馏 | Anthropic 出品 Claude"
description: "---"
pubDate: "2024-12-03"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/trainium2-and-distillation"
---

# Claude 3.5 Haiku 在 AWS Trainium2 上的运行及 Amazon Bedrock 中的模型蒸馏 | Anthropic 出品 Claude

---

作为我们与 [AWS 合作](https://www.anthropic.com/news/anthropic-amazon-trainium)不断扩展的一部分，我们已开始优化 Claude 模型，使其能够在 [AWS Trainium2](https://aws.amazon.com/ai/machine-learning/trainium/) 上运行——这是 AWS 最先进的 AI 芯片。

为了预览 Trainium2 所能实现的效果，Claude 3.5 Haiku 现已支持在 [Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 中进行延迟优化推理，使模型在保持精度不受影响的前提下速度大幅提升。

我们还在 Amazon Bedrock 中新增了对模型蒸馏的支持，将更大规模 Claude 模型的智能能力赋予我们更快、更具成本效益的模型。

### 在 Trainium2 上运行下一代模型

我们正在与 AWS 合作构建 Project Rainier——一个由 Trn2 UltraServer 组成的 EC2 UltraCluster，其中包含数十万颗 Trainium2 芯片。该集群将提供超过五倍于我们当前一代领先 AI 模型训练所用算力（以百亿亿次浮点运算计）的计算能力。

Trainium2 使我们能够在 Amazon Bedrock 中提供速度更快的模型，Claude 3.5 Haiku 率先支持延迟优化推理，目前已面向公众预览。通过启用延迟优化，Claude 3.5 Haiku 的推理速度可提升最高达 60%——使其成为从代码补全到实时内容审核和聊天机器人等各类应用场景的理想选择。

这款由 Trainium2 驱动的 Claude 3.5 Haiku 加速版本已通过 [跨区域推理](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html) 在美国东部（俄亥俄）区域上线，定价为每百万输入 token 1 美元、每百万输出 token 5 美元。

### Amazon Bedrock 模型蒸馏

我们还让客户能够以 Claude 3 Haiku——我们上一代最具成本效益的模型——获得前沿性能。通过蒸馏技术，Claude 3 Haiku 现在可以取得显著的性能提升，在特定任务上达到与 Claude 3.5 Sonnet 相当的精度——而价格与速度与我们最具成本效益的模型保持一致。

该技术将知识从"教师"模型（Claude 3.5 Sonnet）迁移到"学生"模型（Claude 3 Haiku），使客户能够以极低的成本运行检索增强生成（RAG）和数据分析等复杂任务。

与传统微调不同，传统微调要求开发人员手动构建训练示例并持续调整参数，Amazon Bedrock 模型蒸馏通过以下方式实现了整个流程的自动化：

1. **从 Claude 3.5 Sonnet 生成合成训练数据**
2. **对 Claude 3 Haiku 进行训练和评估**
3. **托管**最终的蒸馏模型以供推理使用

Amazon Bedrock 模型蒸馏会自动应用不同的数据合成方法——从生成相似提示到基于您的示例提示-响应对创建新的高质量响应。

Amazon Bedrock 中 Claude 3 Haiku 的蒸馏功能现已开放预览。更多详情请参阅 AWS [发布博客](https://aws.amazon.com/blogs/aws/build-faster-more-cost-efficient-highly-accurate-models-with-amazon-bedrock-model-distillation-preview/)和[文档](https://docs.aws.amazon.com/bedrock/latest/userguide/model-distillation.html)。

### Claude 3.5 Haiku 价格下调

除了在 Trainium2 上提供更快的版本外，客户仍可继续通过 [Anthropic API](https://console.anthropic.com/workbench)、[Amazon Bedrock](https://aws.amazon.com/bedrock/claude/) 和 [Google Cloud 的 Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) 访问 [Claude 3.5 Haiku](https://www.anthropic.com/claude/haiku)。

为使该模型在更广泛的使用场景中更加触手可及，我们将 Claude 3.5 Haiku 的价格在所有平台上下调至每百万输入 token 0.80 美元、每百万输出 token 4 美元。

### 快速上手

从今天起，模型蒸馏和加速版 Claude 3.5 Haiku 已在 Amazon Bedrock 中开放预览。对于寻求价格、性能与速度最佳平衡的开发人员来说，您现在拥有了更多 Claude 模型选择：

* 由 Trainium2 驱动、启用延迟优化的 Claude 3.5 Haiku，适用于通用场景
* 经蒸馏获得前沿性能的 Claude 3 Haiku，适用于高吞吐量、重复性任务场景

如需开始使用，请访问 [Amazon Bedrock 控制台](https://signin.aws.amazon.com/signup?request_type=register)。我们迫不及待想看到您构建出的作品。
