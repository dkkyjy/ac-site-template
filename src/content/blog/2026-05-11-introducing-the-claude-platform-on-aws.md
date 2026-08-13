---
title: "在 AWS 上推出 Claude 平台"
description: "Claude 平台现已正式在 AWS 上推出，为 AWS 客户提供了一种全新方式，通过 AWS 身份验证、计费和承诺消费抵扣来访问 Claude 平台的完整功能集。Claude 仍可在 Amazon Bedrock 上使用，其中 AWS 作为数据处理方。"
pubDate: "2026-05-11"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-platform-on-aws"
---
Claude 平台现已正式在 AWS 上推出，为 AWS 客户提供了一种全新方式，通过 AWS 身份验证、计费和承诺消费抵扣来访问 Claude 平台的完整功能集。Claude 仍可在 Amazon Bedrock 上使用，其中 AWS 作为数据处理方。

从今天开始，AWS 上的 Claude 平台客户可以使用 [Claude 托管代理](https://claude.com/blog/claude-managed-agents) 大规模部署代理，并使用代码执行、技能、顾问策略等工具进行构建。

## 通过 AWS 访问完整的 Claude 平台

AWS 上的 Claude 平台首次将 Claude API 的完整功能集带给 AWS 客户，所有新功能和测试版功能将在原生 Claude API 上线的同一天发布。

身份验证通过 AWS IAM 运行，审计日志通过 CloudTrail 进行，计费通过单一 AWS 发票完成，该发票可完全抵扣现有承诺消费。客户使用其现有的 AWS 凭证和 IAM 策略，因此团队可以继续使用他们已经管理的工具和权限。

AWS 上的 Claude 平台将在大多数 AWS 商业区域可用，并支持全球和美国推理地理位置。

## 包含的内容

AWS 上的 Claude 平台包含原生平台功能，例如：

* [**Claude 托管代理（测试版）**](https://platform.claude.com/docs/en/managed-agents/overview) 用于大规模构建和部署代理
* [**顾问策略（测试版）**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool) 通过咨询顾问模型为代理提供智能提升
* [**网络搜索**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-search-tool) **和** [**网络获取**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/web-fetch-tool) 用来自网络的最新真实世界数据增强 Claude 的知识
* [**代码执行**](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool) 用于在 API 调用中直接运行 Python 代码、创建可视化效果和分析数据
* [**文件 API（测试版）**](https://platform.claude.com/docs/en/build-with-claude/files) 用于跨对话上传和引用文档
* [**技能（测试版）**](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) 用于教授 Claude 最佳实践，使其提供一致的结果
* [**MCP 连接器（测试版）**](https://platform.claude.com/docs/en/agents-and-tools/mcp-connector) 用于将 Claude 连接到任何远程 MCP 服务器，无需编写客户端代码
* [**提示缓存**](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) 用于降低重复上下文的成本和延迟
* [**引用**](https://platform.claude.com/docs/en/build-with-claude/citations) 用于将响应基于源文档
* [**批处理**](https://platform.claude.com/docs/en/build-with-claude/batch-processing) 用于高容量、异步工作负载

AWS 上的 Claude 平台客户还可以访问 Claude 控制台，这是 Anthropic 用于构建和测试 Claude 的开发环境。该控制台包括代理、技能、环境、保险库、可观测性工具等的管理功能。

Claude Opus 4.7、Sonnet 4.6 和 Haiku 4.5 现已可用，新模型将在推出时同步在 AWS 上的 Claude 平台上发布。

## 为开发者选择正确的路径

AWS 上的 Claude 平台和 Amazon Bedrock 上的 Claude 都使 AWS 客户能够在 Claude 模型上进行构建。区别在于谁运营服务以及哪些功能可用。

**AWS 上的 Claude 平台** 是 Anthropic 首次推出的此类产品，从第一天起就为您提供所有原生 Claude API 功能。Anthropic 运营该服务，数据在 AWS 边界之外处理。这对于希望获得完整 Claude 平台体验的公司来说是一个不错的选择。

**Amazon Bedrock 上的 Claude** 保持 AWS 作为数据处理方，并在 AWS 边界内运营。这适合有严格区域数据驻留要求或需要其数据仅在 AWS 基础设施内处理的公司。

## 开始使用

AWS 上的 Claude 平台现已可用。要开始使用，请访问 [AWS 上的 Claude 平台](https://aws.amazon.com/claude-platform/) 或浏览 [文档](https://platform.claude.com/docs/en/build-with-claude/claude-platform-on-aws)。

如果您有现有的 Bedrock 私有报价，请在开始使用 AWS 上的 Claude 平台之前联系您的 Anthropic 或 AWS 客户经理，以确保您的折扣正确应用。折扣不能追溯应用于在 Claude 平台私有报价被接受之前产生的使用量。
