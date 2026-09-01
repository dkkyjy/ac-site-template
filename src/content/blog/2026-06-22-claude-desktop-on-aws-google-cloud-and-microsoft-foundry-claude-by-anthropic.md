---
title: "在 AWS、Google Cloud 和 Microsoft Foundry 上使用 Claude 桌面版 | Anthropic 出品 Claude"
description: "---"
pubDate: "2026-06-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/the-full-claude-desktop-experience-on-aws-google-cloud-and-microsoft-foundry"
---

# 在 AWS、Google Cloud 和 Microsoft Foundry 上使用 Claude 桌面版 | Anthropic 出品 Claude

---

通过 AWS、Google Cloud 和 Microsoft Foundry 使用 Claude 桌面版的企业，现在即可获得完整的桌面体验——对话、Claude 协作和 Claude 代码，全部集成于一个应用之中。

如今，IT 团队可在各产品中确保推理始终运行在自身环境内，并以按用户单点登录（SSO）、MDM 策略模板、离线安装程序选项以及可完全在设备端运行的 Microsoft 365 连接器，在全组织范围内部署 Claude 桌面版。

推理在您所配置的云区域中运行，对话历史存储在本地。您可以控制数据连接器所访问的端点，以及 Anthropic 所接收的聚合遥测数据。

### 面向全组织的统一界面

在此之前，通过 AWS、Google Cloud 和 Microsoft Foundry 使用 Claude 桌面版的客户仅能使用 Claude 协作和 Claude 代码。如今，一次部署即可覆盖所有角色，每个界面拥有独立的策略密钥，由您决定谁在何时获得哪些权限。  
  
对话用于快速获取答案和梳理思路。Claude 协作用于团队成员更愿意交代的任务：Claude 在已批准的来源中进行研究，处理设备上已有的文件并生成交付成果，完成后呈现结果。Claude 代码面向希望进行智能体编程而无需依赖终端的工程师。

### 部署管控

在全组织范围内部署 Claude 桌面版，意味着在您现有的系统框架内完成工作。

**像使用任何工作应用一样登录。** 员工使用与其他工作相同的账户：IAM 身份中心、工作身份联合、Microsoft Entra ID，或 Okta 等任何 OIDC 提供商。无需轮换共享密钥，终端用户设备上无需存储云凭证。

**像管理现有应用一样部署。** 从设置界面导出策略模板，通过 Intune、GPO 或 Jamf 进行推送。离线安装程序可覆盖隔离网络环境。

**在用户看到之前确认一切就绪。** 在全面推广之前，测试每个连接器，确认您的服务提供商所支持的 Claude 模型，并验证连接。模型防护机制确保路由始终指向 Claude（包括 GovCloud），即使设置出现错误也不会偏离。

**小范围起步，随采用率扩大。** 对话、Claude 协作和 Claude 代码各有独立的策略密钥，因此您可以为非技术团队开放对话和 Claude 协作，为工程团队开放 Claude 代码，然后随着各团队逐步采用而扩大访问范围。您的强制拒绝规则适用于所有标签页。

**将 Claude 带到工作发生的地方。** Microsoft 365 连接器通过您自己的 Entra 应用让 Claude 访问邮件和文档，支持租户白名单，并为 GCC High/国防部端点提供测试版支持。对于最严格的数据驻留要求，请使用我们的本地连接器，连接将保持在设备与 Microsoft 之间。

> "我们通过现有的云环境快速部署了 Claude 桌面版——无需额外的供应商合同。我们自己的大语言模型网关让一个团队将其部署到全球数百名用户，无需大量基础设施搭建。"——Sarang Oh，韩华解决方案（Hanwha Solutions）分析/人工智能团队负责人

### 快速上手

对于管理员，[部署指南](http://claude.com/docs/third-party/claude-desktop/installation)将逐步介绍单点登录（SSO）、策略模板和全面推广前的验证流程。或者联系您的客户团队，我们将协助您制定推广计划。
