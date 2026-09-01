---
title: "工作负载身份联合（WIF）现已在 Claude 平台正式可用 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/workload-identity-federation"
---

# 工作负载身份联合（WIF）现已在 Claude 平台正式可用 | Claude by Anthropic

---

工作负载身份联合（WIF）现已在 Claude 平台正式可用。WIF 兼容任何符合 OIDC 标准的身份提供商，并覆盖所有 Claude API 端点，包括通过我们的官方 SDK 和 Claude Code 访问端点时的场景。

通过面向工作负载的 WIF 和面向交互式会话的 [ant auth login](https://platform.claude.com/docs/en/cli-sdks-libraries/cli/quickstart#authentication)，开发者在使用 Claude 平台进行开发时，无需再处理静态 API 密钥。

## 工作负载身份联合的工作原理

WIF 以在请求时签发的短期有效、具有作用域限制的凭证替代了静态 API 密钥。无论您是运行 GitHub Actions 的双人初创团队，还是拥有详细凭证策略的大型企业，现在都可以以与认证其余技术栈相同的方式对 Claude 平台进行身份验证。

使用 WIF，无需创建、轮换或担心泄露静态的 Anthropic 凭证。工作负载使用其已有的身份进行认证：AWS IAM 角色、GCP 或 Kubernetes 服务账号、Azure 托管身份、GitHub Actions 令牌、Okta，或其他符合 OIDC 标准的提供商。

我们还为 Claude 平台引入了服务账号，使每个工作负载都可以拥有自己的身份、角色和审计记录，而非共用一个 API 密钥。首先，一条联合规则将外部身份绑定到一个服务账号。然后，当工作负载请求访问时，Claude 平台会验证工作负载的已签名 OIDC 令牌，将其声明与您的联合规则进行匹配，并签发一个受服务账号角色限制的短期访问令牌。每一次交换和请求都会被记录在该服务账号的审计日志中。

## 几分钟内完成第一个工作负载的配置

[Claude 控制台](https://platform.claude.com/) 提供了引导式配置流程，用于设置工作负载身份。该流程会逐步验证每个环节，并以一条测试命令结束，以确认您的工作负载能够成功完成身份验证。

## 让整个组织告别静态密钥

WIF 兼容用于组织管理的 [管理 API](https://platform.claude.com/docs/en/build-with-claude/administration-api)。联合规则可通过细粒度作用域配置最小权限访问。

联合配置也完全支持编程化操作，适用于大规模运营的组织。新的管理 API 端点允许您创建和更新签发者、服务账号及联合规则。
