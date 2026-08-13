---
title: "为 MCP 连接器集中管理授权 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-06-18"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/enterprise-managed-auth"
---

# 为 MCP 连接器集中管理授权 | Anthropic 的 Claude

---

管理员现在可以通过其身份提供商为其整个组织预置 MCP 连接器，首先是 Okta。用户在首次登录时会自动获得连接器访问权限，授权由组织集中配置。

连接器让 Claude 在工作中更有用——它们为 Claude 提供了团队已在使用工具所需的上下文。在此之前，启用它们需要两步操作：管理员为组织启用连接器，然后每个用户自己进行授权。

企业托管授权简化了第二步。管理员授权一次连接器，用户通过他们已有的 IdP 组和角色继承访问权限，并且连接器在用户首次打开 Claude 时就已经存在。最终结果是终端用户实现零接触连接器设置。

企业托管授权是 [Model Context Protocol 的企业托管授权扩展](https://modelcontextprotocol.io/extensions/auth/enterprise-managed-authorization) 的首个实现。它基于开放标准构建，因此任何连接器都可以支持该标准——包括你自己的团队构建的自定义连接器——而且它们对每位 Claude 客户的工作方式都一样。

### 工作原理

将你的身份提供商连接到 Claude，并选择要为你的组织启用哪些 MCP 连接器。当员工登录时，他们的连接器已经就位。访问权限在 Claude 聊天、Claude Code 和 Cowork 中保持一致。

对于管理员来说，这会将 MCP 访问管理整合到管理其余技术栈的同一工作流中：预置一次，按组限定范围，通过 IdP 管理吊销。由于通过 IdP 检查访问权限无摩擦，管理员可以缩短访问令牌的生命周期而不影响生产力——因此当某人被取消预置时，他们的连接器访问权限会快速过期，而不是停留在旧令牌上。访问权限通过你已经信任的身份提供商运行，因此连接器遵循与其他所有内容相同的安全性和访问控制，而不是需要监控的独立表面。

管理员还可以要求某个连接器仅通过 IdP 连接，这可以干净地将工作和个人使用分开，并防止有人意外将个人账户链接到工作工具。

### 与生态系统一起构建

企业托管授权在三个群体之间运作：管理访问权限的身份提供商、支持该标准的 MCP 提供商，以及在其团队中部署托管连接的 Claude 客户。

**身份提供商。** 启动时支持 Okta，即将支持更多身份提供商。

**MCP 提供商。** Asana、Atlassian、Canva、Figma、Granola、Linear 和 Supabase 在启动时支持企业托管授权，Slack 即将支持。

**Claude 客户。** Hubspot、Ramp 和 Webflow 是正在其团队中推广企业托管授权的组织之一。
