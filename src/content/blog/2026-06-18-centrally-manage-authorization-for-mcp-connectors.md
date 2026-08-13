---
title: "集中管理 MCP 连接器的授权"
description: "管理员现在可以通过其身份提供商（从 Okta 开始）为整个组织预置 MCP 连接器。用户在首次登录时即可自动获得连接器访问权限，授权由其组织集中配置。"
pubDate: "2026-06-18"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/enterprise-managed-auth"
---
            管理员现在可以通过其身份提供商（从 Okta 开始）为整个组织预置 MCP 连接器。用户在首次登录时即可自动获得连接器访问权限，授权由其组织集中配置。

连接器使 Claude 在工作中更有用——它们为 Claude 提供团队已使用工具所需的上下文。在此之前，启用连接器需要两个步骤：管理员为组织启用连接器，然后每个用户自行授权。

企业托管授权简化了第二步。管理员只需授权一次连接器，用户通过其已有的 IdP 组和角色继承访问权限，连接器在用户首次打开 Claude 时即已就绪。最终结果是为最终用户实现零接触连接器设置。

企业托管授权是 [企业托管授权扩展](https://modelcontextprotocol.io/extensions/auth/enterprise-managed-authorization) 对模型上下文协议的首次实现。它基于开放标准构建，因此任何连接器（包括您自己团队构建的自定义连接器）都可以支持它，并且对于每位 Claude 客户，它们都以相同方式工作。

### 工作原理

将您的身份提供商连接到 Claude，并选择要为组织启用的 MCP 连接器。当员工登录时，他们的连接器已经就绪。访问权限在 Claude 聊天、Claude Code 和 Cowork 中保持一致。

对于管理员而言，这可将 MCP 访问管理纳入管理其余技术栈的相同工作流程：预置一次，按组划分范围，通过 IdP 管理撤销。由于通过 IdP 检查访问权限毫无阻碍，管理员可以缩短访问令牌的生命周期而不影响生产力——因此当某人被取消预置时，其连接器访问权限会快速过期，而不会滞留在旧令牌上。访问通过您已信任的身份提供商运行，因此连接器与其他所有内容受相同的安全性和访问控制约束，而非需要单独监控的独立表面。

管理员还可以要求连接器仅通过 IdP 进行连接，这可将工作和个人使用清晰分离，并防止某人意外将个人账户链接到工作工具。

### 构建于生态系统之上

企业托管授权适用于三个群体：管理访问权限的身份提供商、支持该标准的 MCP 提供商，以及在团队中部署托管连接的 Claude 客户。

**身份提供商。** 启动时支持 Okta，即将支持更多身份提供商。

**MCP 提供商。** Asana、Atlassian、Canva、Figma、Granola、Linear 和 Supabase 在启动时支持企业托管授权，Slack 即将支持。

**Claude 客户。** Hubspot、Ramp 和 Webflow 是正在其团队中推广企业托管授权的组织之一。

### 开始使用

企业托管授权现已作为测试版提供给 Claude Team 和 Enterprise 计划的客户。在我们的 [帮助中心](https://support.claude.com/en/articles/15537633) 了解更多信息，并 [申请访问权限](https://claude.com/form/ema-waitlist) 以开始使用。

任何身份提供商或 MCP 提供商都可以通过实现 MCP 授权规范的 [开放扩展](https://blog.modelcontextprotocol.io/posts/enterprise-managed-auth) 来添加对企业托管授权的支持。在此 [提交兴趣](https://docs.google.com/forms/d/e/1FAIpQLSf1goHGNDVFK7rncYuh6wnRpWSy7eGOcgL1i8uw3oyKFO9UUA/viewform?usp=sharing&ouid=101055591948883487705) 加入测试版。
