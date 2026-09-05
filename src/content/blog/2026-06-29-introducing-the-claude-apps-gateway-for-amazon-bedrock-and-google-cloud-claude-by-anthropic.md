---
title: "为 Amazon Bedrock 和 Google Cloud 推出 Claude 应用网关 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-29"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/introducing-the-claude-apps-gateway"
---

# 为 Amazon Bedrock 和 Google Cloud 推出 Claude 应用网关 | Claude by Anthropic

---

今天，我们为 Amazon Bedrock 和 Google Cloud 推出 Claude 应用网关。此前，在这些平台上运行 Claude Code 意味着需要为每位开发者配置云凭证、手动将设置推送到每台笔记本电脑，并搭建单独的工具来查看每位开发者的支出。该网关是一个自托管的控制平面，为您提供企业 SSO 登录、集中执行的策略、基于角色的访问以及 Claude Code 的按用户成本归属。

## **部署网关**

该网关以单个无状态容器的形式运行，部署在 Linux 上，并以 PostgreSQL 数据库作为后端。它持有您的上游凭证，对您的身份提供商进行开发者身份验证，分发并执行托管设置，并将按用户使用情况报告给您运营的收集器。开发者入职意味着将他们添加到您的身份提供商（IdP）中。离职则意味着将其移除。

该网关由 Anthropic 在您的开发者已经安装的同一个 `claude` 二进制文件中构建和发布，因此您可以将其作为单个无状态容器在您的基础设施上运行。由于网关和客户端是一起构建的，`/login` 流程支持网关感知，客户端在登录时自动应用托管设置，并且策略在每个请求上得到一致执行。

## **网关的工作原理**

网关处理以下事项：

* **身份。** 它充当 OpenID Connect（OIDC）依赖方，对接 Google Workspace、Microsoft Entra ID、Okta 或任何符合标准的 OIDC 提供商，并签发短期会话。开发者机器上不会存留长期密钥。
* **策略。** 您可以在服务器上一次性定义托管设置，客户端在登录时接收策略，网关在每个请求上执行该策略。您可以集中调整允许的模型和默认设置。
* **遥测。** 客户端为每个请求标记使用指标，网关通过 OTLP 将其中继到您配置的收集器，该收集器位于您的网络中，并遵循您的保留计划。
* **路由。** 网关持有您的上游凭证，并将推理路由到 Claude API、Amazon Bedrock 或 Google Cloud，并可在提供商之间进行可选故障转移。
* **支出上限。** 网关允许您设置每日、每周和每月的支出限制。限制可按组织、组或用户应用。

除非您配置网关使用 Claude API，否则网关不会将推理流量或使用数据发送给 Anthropic。我们还发布了网关使用的协议，以便其他网关开发者可以实现相同的功能。

## **入门**

网关现已可用。要开始使用：

* **部署网关：** 下载 Claude Code CLI 二进制文件，将 `gateway.yaml` 指向您的 OIDC 签发者和上游凭证，并在您的 IdP 中注册一个 OIDC 应用。
* **推广部署：** 在客户端机器上的 `managed-settings.json` 中配置 `forceLoginMethod` 和 `forceLoginGatewayUrl` 参数。客户端在首次启动时连接到您的网关。

[查看文档](https://code.claude.com/docs/en/claude-apps-gateway) 了解更多信息。
