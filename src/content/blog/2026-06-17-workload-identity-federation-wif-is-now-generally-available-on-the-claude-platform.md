---
title: "工作负载身份联合（WIF）现已在 Claude 平台上正式可用。"
description: "工作负载身份联合（WIF）现已在 Claude 平台上正式可用。WIF 兼容任何符合 OIDC 标准的身份提供商，并覆盖所有 Claude API 端点，包括通过我们的第一方 SDK 和 Claude Code 访问端点时。"
pubDate: "2026-06-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/workload-identity-federation"
---
            工作负载身份联合（WIF）现已在 Claude 平台上正式可用。WIF 兼容任何符合 OIDC 标准的身份提供商，并覆盖所有 Claude API 端点，包括通过我们的第一方 SDK 和 Claude Code 访问端点时。

借助适用于工作负载的 WIF 和适用于交互式会话的 [ant auth login](https://platform.claude.com/docs/en/cli-sdks-libraries/cli/quickstart#authentication)，开发者在使用 Claude 平台进行构建时，永远无需处理静态 API 密钥。

## 工作负载身份联合的工作原理

WIF 使用在请求时颁发的短期、作用域受限的凭证来替代静态 API 密钥。无论您是运行 GitHub Actions 的两人创业公司，还是拥有详细凭证策略的企业，现在都可以使用与验证其余技术栈相同的方式来验证 Claude 平台。

使用 WIF，无需创建、轮换或泄露任何静态的 Anthropic 凭证。工作负载使用其已有的身份进行身份验证：AWS IAM 角色、GCP 或 Kubernetes 服务账户、Azure 托管标识、GitHub Actions 令牌、Okta 或其他符合 OIDC 标准的提供商。

我们还将在 Claude 平台上引入服务账户，因此每个工作负载都可以拥有自己的身份、角色和审计跟踪，而不是共享的 API 密钥。首先，联合规则将外部身份绑定到服务账户。然后，当工作负载请求访问时，Claude 平台会验证工作负载的签名 OIDC 令牌，将其声明与您的联合规则进行匹配，并颁发一个受服务账户角色限制的短期访问令牌。每次交换和请求都会在您的审计日志中记录到该服务账户。

## 在几分钟内设置您的第一个工作负载

[Claude 控制台](https://platform.claude.com/) 提供了一个引导式设置流程，用于配置工作负载身份。该设置会验证每个步骤，并以一个测试命令结束，确认您的工作负载可以成功进行身份验证。

## 无需静态密钥即可运行整个组织

WIF 与用于组织管理的 [管理 API](https://platform.claude.com/docs/en/build-with-claude/administration-api) 兼容。通过细粒度作用域，可以配置联合规则以实现最低权限访问。

对于大规模运营的组织，联合配置也完全支持编程方式。新的管理 API 端点允许您创建和更新颁发者、服务账户和联合规则。

## 入门指南

API 密钥与 WIF 可同时使用，因此您可以一次迁移一个工作负载。请阅读每个身份提供商的设置 [指南](https://platform.claude.com/docs/en/build-with-claude/workload-identity-federation)，或打开 [Claude 控制台](https://platform.claude.com/) 来连接您的第一个工作负载。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终响应（包括作为执行指令一部分生成的任何章节标题或标题）仅使用 zh-CN 语言编写。
