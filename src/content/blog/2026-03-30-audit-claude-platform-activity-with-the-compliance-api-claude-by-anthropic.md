---
title: "使用合规性 API 审计 Claude 平台活动 | Claude by Anthropic"
description: "---"
pubDate: "2026-03-30"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-platform-compliance-api"
---

# 使用合规性 API 审计 Claude 平台活动 | Claude by Anthropic

---

## 使用合规性 API 审计 Claude 平台活动

合规性 API 现已在 Claude 平台上可用，让管理员能够以编程方式访问整个组织的审计日志。安全和合规团队可以跟踪用户活动、监控配置更改，并将 Claude 使用数据集成到现有的合规基础设施中。

## 为您的组织提供审计日志

受监管行业（如金融服务、医疗保健、法律）的组织需要详细记录谁在何时访问了什么内容、发生了什么变化。如果没有对该数据的编程访问权限，合规团队必须依赖手动导出和定期审查，这种方式无法扩展。

合规性 API 提供一个活动源，记录您组织中与安全相关的事件。管理员可以获取按时间范围、特定用户或 API 密钥筛选的活动日志。

该 API 目前跟踪两类活动：

* **管理员和系统活动：** 修改资源访问权限或配置的操作，例如向工作区添加成员、创建 API 密钥、更新账户设置或修改实体访问权限。
* **资源活动：** 用户驱动的创建或修改资源数据的操作，例如创建文件、下载文件或删除技能。这些操作涵盖可能影响数据或允许资源访问敏感信息的操作，但不包括与模型的直接交互。

综合来看，这些活动覆盖了用户登录和登出事件、账户设置更新、工作区更改以及其他组织审计事件。该 API 不会记录推理活动，例如用户与模型的交互或模型活动。

## 开始使用

联系您的客户团队，为您的组织启用合规性 API。启用后，创建一个管理员 API 密钥，并使用它查询活动源端点。请注意，日志记录从 API 启用时开始——在此之前的历史活动不可用。

已经为 Claude Enterprise 使用合规性 API 的组织，可以将他们的 Claude API 组织添加到同一父组织中，并从单个源筛选两个组织的活动。

请阅读 [Anthropic 信任中心](https://trust.anthropic.com/resources?s=tob70gqyan60x3dwb7nkap&name=anthropic-compliance-api) 上的文档以了解更多信息。
