---
title: "通过贡献指标衡量 Claude Code 的影响 | Claude by Anthropic"
description: "---"
pubDate: "2026-01-29"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/contribution-metrics"
---

# 通过贡献指标衡量 Claude Code 的影响 | Claude by Anthropic

---

今天，我们在 Claude Code 中推出贡献指标（公开测试版）。工程团队现在可以衡量 Claude Code 如何影响团队的开发速度，追踪在 Claude 帮助下提交的 PR 和提交的代码。

## **我们在 Anthropic 的交付方式**

Anthropic 的工程团队广泛使用 Claude Code，而贡献数据帮助我们量化其影响。随着 Claude Code 内部采用率的提高，我们观察到每位工程师每日合并的 PR 数量增长了 67%。在各个团队中，70%–90% 的代码现在都是在 Claude Code 的辅助下编写的。

虽然仅凭拉取请求无法全面衡量开发者效率，但我们发现它与工程团队关心的事项紧密相关：更快地交付功能、修复错误以及让用户满意。

Claude Code 中新增的贡献指标可帮助你在自己的组织中衡量这一影响。

## **通过 Claude Code 衡量开发速度**

通过与 GitHub 集成，贡献指标呈现以下数据点：

* **合并的拉取请求**：追踪在 Claude Code 辅助下创建的 PR 与未使用 Claude Code 创建的 PR
* **提交的代码**：查看在 Claude Code 辅助下提交到你仓库的代码行数，以及未使用 Claude Code 提交的行数
* **每位用户的贡献数据**：了解你团队中的采用模式

贡献数据的计算方式是将 Claude Code 会话活动与 GitHub 提交和 PR 进行匹配。我们采用保守的计算方式，只有对 Claude Code 的参与有高度信心的代码才会被计为辅助代码。

这些指标会出现在你现有的 Claude Code 分析仪表板中，工作区管理员和所有者均可访问。无需外部工具或数据管道。只需安装我们的 GitHub 应用并验证你组织的 GitHub 账户，指标便会自动填充到仪表板中。

贡献指标旨在补充你现有的工程 KPI。你可以将其与 DORA 指标、冲刺速度或其他衡量标准结合使用，了解将 Claude Code 引入团队带来的方向性变化。

## **开始使用**

代码贡献指标目前以测试版形式提供给 Claude Team 和企业客户。要启用它们：

1. 为你的组织安装 [Claude GitHub 应用](https://github.com/apps/claude)
2. 导航至 [管理员设置 > Claude Code](http://claude.ai/admin-settings/claude-code)，开启 GitHub 分析
3. 验证你的 GitHub 组织

当你的团队开始使用 Claude Code 时，指标会自动填充。查看[文档](https://code.claude.com/docs/en/analytics)获取详细的设置说明以及解读指标的指导。
