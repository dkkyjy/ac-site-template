---
title: "Inference Hooks：Claude 企业版的内联数据防泄漏 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-05"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-enterprise-inference-hooks"
---

# Inference Hooks：Claude 企业版的内联数据防泄漏 | Claude by Anthropic

---

Inference Hooks 使您的合规团队能够在每个提示和工具调用响应到达 Claude 之前对其进行审查并执行策略——覆盖包括对话、Claude Code、Claude Cowork 在内的所有 Claude 企业版界面。您的 DLP 服务器负责决定拦截或放行，Claude 则实时执行该决策，在未经批准的内容到达 Claude 之前将其拦截。

安全团队要求员工能够传输敏感数据的每一个通道都必须经过其团队控制的检查点。在此之前，原生的内联执行仅限于 Claude Code 的客户端钩子。Inference Hooks 通过一个统一的执行层填补了这一空白，该执行层覆盖所有 Claude 企业版界面，无需为每个产品进行单独的集成工作或部署代理。

## Inference Hooks 的工作原理

当组织启用 Inference Hooks 后，每个推理请求都会通过一条经过签名的 WebSocket 连接路由至安全服务器。在模型开始生成之前，Claude 会将提示及其上下文环境发送至您的服务器。您的服务器返回判定结果——允许或拒绝——Claude 只有在收到判定结果后才会继续执行。相同的检查机制也适用于工具调用：当 Claude 调用工具时——包括通过 MCP、技能和插件连接的工具——工具响应在返回模型之前会先经过检查。

各团队已在将该实时检查机制付诸实践。"Inference Hooks 增加了一个检查点，能够在模型看到数据之前实时审查流入 Claude 的内容，"Bandwidth 信息安全副总裁 Andrew Grimmett 表示。"这让我们能够在不失去控制的前提下，安全地加速 AI 应用。"

## Inference Hooks 的使用方式

将现有的 DLP 方案扩展至 Claude。Inference Hooks 采用开放的、基于 Webhook 的协议，并提供公开的架构规范。这使得部署变得简单——只需将其指向您的其他工具已在使用的服务器即可，包括 Netskope、Palo Alto Networks、Proofpoint、Zscaler，或您自行搭建的 AI 安全服务器。

通过一套配置覆盖对话、Claude Code、Cowork 及其他 Claude 企业版产品。在组织层面启用一次 Inference Hooks，即可适用于所有 Claude 企业版界面，包括通过 MCP 连接器、技能和插件发起的工具调用。

通过影子模式（始终允许）、基于角色的排除规则和基于百分比的逐步推广来简化部署。自定义故障策略容忍度、超时时间及其他设置，以匹配组织的风险承受水平。

## 开始使用

Inference Hooks 现已以测试版形式向 Claude 企业版客户开放。阅读[文档](https://platform.claude.com/docs/en/manage-claude/inference-hooks)以配置组织的 DLP 服务器，并在所有 Claude 企业版界面上开始执行策略。

对于安全厂商而言，Inference Hooks 基于具有文档化架构规范的 Webhook 协议构建，因此您可以开发集成方案，Claude 企业版客户也可以将组织指向您的平台。
