---
title: "将 Claude Code 和 Claude Cowork 引入政府机构"
description: "将 Claude Code 和 Claude Cowork 引入政府机构"
pubDate: "2026-07-07"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/bringing-claude-code-and-claude-cowork-to-government"
---

# 将 Claude Code 和 Claude Cowork 引入政府机构

            ---

            [Claude Code](https://claude.com/product/claude-code) 和 [Claude Cowork](https://claude.com/product/cowork) 现已在 Claude for Government Desktop 中公开测试版提供，该桌面版基于我们商业客户使用的相同应用程序构建，并通过 FedRAMP High 授权环境交付。

借助 Claude Code，公共部门团队可以构建和现代化支撑公共服务软件系统。Claude Cowork 可直接处理桌面上的文件，使机构工作人员能够将备忘录创建、RFP 审查、案件工作和演示文稿委托给 Claude。

扩展后的体验还带来了额外的治理能力。管理员可以设置配置默认值，以及跨部门分配和控制支出。安全团队和授权官员可获得防篡改审计日志和支持机构 ATO 流程的文档。

今天的发布使各机构更容易获取、授权和分配 AI，以完成其使命。

## 新增功能

**Claude Code 和 Claude Cowork。** 各机构将获得与商业用户相同节奏的新功能。对话历史记录存储在机构管理的设备本地。推理在 FedRAMP High 授权环境中运行。

**符合拨款要求的计费方式。** 项目办公室可以通过标准席位将 AI 支出与拨款资金挂钩，也可以自行定义具有支出和模型限制的席位层级，并且使用量以固定增量购买，设有严格的不可超支上限。管理员可以在管理控制台中按用户和按模型跟踪使用情况，自动消耗提醒会在余额不足前发出警告。

**与部门组织架构匹配的管理方式。** 部门级管理员可以将席位和预付费使用量分配给下属机构，同时允许每个机构自行管理其用户。管理员可以使用 SCIM 组映射为特定席位层级设置速率限制、金额上限和允许的模型。此外，分层配置为下属机构设置默认值，包括 Claude 可以连接的内容、可用的功能，以及指导 Claude 如何与用户交互的指令。

**设计内置的监督机制。** 每项管理操作都记录在哈希链审计日志中，组织管理员可以直接在产品中查看。Anthropic 方的敏感操作需要两人批准。使用量导出仅包含计量数据，因此机构可以回答 ATO 和 IG 的请求，而无需传输敏感材料。

## 安全与监督

对于评估桌面部署的安全团队，我们正在发布 FedRAMP 安全配置指南作为公开文档，客户可以使用该指南以安全方式配置其 Claude for Government 产品。

此外，FedRAMP 要求我们提供正式的变更通知，其中包含与此变更相关的详细信息。

最后，新的桌面客户端已提供渗透测试摘要，后续的渗透测试摘要将在可用时提供。变更通知和渗透测试摘要可通过 Anthropic 的信任中心在 NDA 下获取。该应用程序通过标准的机构 MDM 平台部署。

## 入门指南

Claude for Government 从今天起开始提供测试版。Anthropic 仍然是签约和计费方——机构无需单独与云提供商建立关系即可开始使用。

新客户可以通过 [claude.com/solutions/government](https://claude.com/solutions/government) 申请访问权限。

安全团队可以通过[以下链接](https://trust.anthropic.com/resources?s=gutlu13ft6nd7famxolbxg&name=[anthropic]-2025-annual-penetration-testing-reports)下载渗透测试工件。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终响应（包括作为执行指令一部分生成的任何章节标题或标题）仅以 zh-CN 语言书写。
