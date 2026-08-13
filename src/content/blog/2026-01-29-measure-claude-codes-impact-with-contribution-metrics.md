---
title: "通过贡献度指标衡量 Claude Code 的影响"
description: "今天，我们在 Claude Code 中推出贡献度指标，现已进入公开测试阶段。工程团队现在可以衡量 Claude Code 如何影响团队的开发速度，追踪在 Claude 协助下提交的 PR 和提交的代码。"
pubDate: "2026-01-29"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/contribution-metrics"
---
            今天，我们在 Claude Code 中推出贡献度指标，现已进入公开测试阶段。工程团队现在可以衡量 Claude Code 如何影响团队的开发速度，追踪在 Claude 协助下提交的 PR 和提交的代码。

## **我们在 Anthropic 的交付方式**

Anthropic 的工程团队广泛使用 Claude Code，贡献数据帮助我们量化了其影响。随着 Claude Code 在内部采用率的提高，我们看到每位工程师每天合并的 PR 数量增加了 67%。在各团队中，70-90% 的代码现在是在 Claude Code 的协助下编写的。

虽然仅凭拉取请求无法完全衡量开发人员的速度，但我们发现它是工程团队所关注事项的紧密代理指标：更快地交付功能、修复错误以及让用户满意。

Claude Code 中新增的贡献度指标可帮助您在组织内部衡量这一影响。

## **使用 Claude Code 衡量开发速度**

通过与 GitHub 集成，贡献度指标可呈现以下数据点：

* **合并的拉取请求**：追踪在 Claude Code 协助下和未协助下创建的 PR
* **提交的代码**：查看在 Claude Code 协助下和未协助下提交到仓库的代码行数
* **按用户划分的贡献数据**：识别团队中的采用模式

贡献数据通过将 Claude Code 会话活动与 GitHub 提交和 PR 进行匹配来计算。我们采用保守的计算方式，只有我们高度确信 Claude Code 参与其中的代码才会被计为协助代码。

这些指标会显示在您现有的 Claude Code 分析仪表板中，工作区管理员和所有者均可访问。无需外部工具或数据管道。只需安装我们的 GitHub 应用并验证您组织的 GitHub 账户，指标就会自动填充到仪表板上。

贡献度指标旨在补充您现有的工程 KPI。将其与 DORA 指标、冲刺速度或其他衡量标准结合使用，以了解将 Claude Code 引入团队后带来的方向性变化。

## **入门指南**

代码贡献度指标现已面向 Claude Team 和企业客户提供测试版。要启用它们：

1. 为您的组织安装 [Claude GitHub 应用](https://github.com/apps/claude)
2. 导航至 [管理设置 > Claude Code](http://claude.ai/admin-settings/claude-code) 并开启 GitHub 分析
3. 验证您的 GitHub 组织

当您的团队使用 Claude Code 时，指标会自动开始填充。查看[文档](https://code.claude.com/docs/en/analytics)以获取详细的设置说明和指标解读指导。
