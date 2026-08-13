---
title: "在 AWS、Google Cloud 和 Microsoft Foundry 上的 Claude Desktop"
description: "通过 AWS、Google Cloud 和 Microsoft Foundry 使用 Claude Desktop 的组织现在可以获得完整的桌面体验——聊天、Claude Cowork 和 Claude Code，全部集成在一个应用程序中。"
pubDate: "2026-06-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/the-full-claude-desktop-experience-on-aws-google-cloud-and-microsoft-foundry"
---
            通过 AWS、Google Cloud 和 Microsoft Foundry 使用 Claude Desktop 的组织现在可以获得完整的桌面体验——聊天、Claude Cowork 和 Claude Code，全部集成在一个应用程序中。

现在，IT 团队可以将推理过程保留在自己的产品环境内，并通过每用户 SSO、MDM 策略模板、离线安装程序选项以及可完全在设备上运行的 M365 连接器，在整个组织范围内部署 Claude Desktop。

推理过程在你配置的区域内的云环境中运行，对话历史记录存储在本地。你控制着数据连接器可以访问的端点以及 Anthropic 接收的聚合遥测数据。

### 面向整个组织的统一界面

在此之前，通过 AWS、Google Cloud 和 Microsoft Foundry 使用 Claude Desktop 的客户只能访问 Claude Cowork 和 Claude Code。现在，一次部署即可覆盖所有角色，并且每个界面都有自己的策略密钥，因此你可以决定谁可以访问什么内容以及何时访问。

聊天功能用于快速获取答案和思考问题。Claude Cowork 用于处理你的员工更愿意移交的工作：Claude 在经批准的来源中进行研究，处理设备上已有的文件，构建可交付成果，并在完成后呈现结果。Claude Code 适用于希望进行智能编码但又不愿一直待在终端中的工程师。

### 部署控制

在整个组织范围内部署 Claude Desktop 意味着要与你已有的系统协同工作。

**像任何工作应用程序一样登录。** 员工使用与其他所有操作相同的企业账户登录：IAM Identity Center、Workforce Identity Federation、Microsoft Entra ID 或任何 OIDC 提供商（如 Okta）。无需轮换共享密钥，最终用户机器上无需存储云凭证。

**像管理任何已有应用程序一样部署。** 从设置界面导出策略模板，并通过 Intune、GPO 或 Jamf 推送它们。离线安装程序可覆盖气隙环境。

**在任何人看到之前确保其正常工作。** 测试每个连接器，确认你的提供商提供哪些 Claude 模型，并验证连接，所有这些都在正式发布之前完成。一个模型保护机制确保路由保持在 Claude 上，包括在 GovCloud 中，即使某个设置配置错误也是如此。

**从小处着手，随着采用率的增长而扩展。** 聊天、Claude Cowork 和 Claude Code 各自拥有自己的策略密钥，因此你可以为非技术团队提供聊天和 Claude Cowork，为工程团队提供 Claude Code，然后随着各团队采用每个界面而扩大访问权限。你的硬性拒绝规则适用于每个选项卡。

**将 Claude 带到工作所在之处。** 一个 Microsoft 365 连接器允许 Claude 通过你自己的 Entra 应用程序访问邮件和文档，支持租户允许列表，并对 GCC High/DoD 端点提供测试版支持。对于最严格的数据驻留要求，请使用我们的本地连接器，连接将保持在设备和 Microsoft 之间。

> "我们通过现有的云环境快速部署了 Claude Desktop——无需单独的供应商合同。我们自己的 LLM 网关让一个团队能够将其部署到全球数百名用户，无需进行大量的基础设施搭建。" - Sarang Oh，韩华解决方案分析/AI 团队负责人

### 开始使用

对于管理员，[部署指南](http://claude.com/docs/third-party/claude-desktop/installation) 详细介绍了 SSO、策略模板和预发布验证。或者联系你的客户团队，我们将帮助你规划发布。
