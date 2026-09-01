---
title: "在 Claude Code 中推出例行任务 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-04-14"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/introducing-routines-in-claude-code"
---

# 在 Claude Code 中推出例行任务 | Anthropic 的 Claude

---

今天，我们以研究预览的形式在 Claude Code 中推出了例行任务（Routines）。例行任务是一种只需配置一次的 Claude Code 自动化流程——包括提示词、代码仓库和连接器——之后可以按计划定时运行、通过 API 调用触发，或响应特定事件自动执行。例行任务运行在 [Claude Code 的云端基础设施](https://code.claude.com/docs/en/claude-code-on-the-web)上，因此无需保持笔记本电脑处于开机状态。

开发者已经在使用 Claude Code 来自动化软件开发流程，但在此之前，他们需要自行管理 cron 定时任务、基础设施以及 MCP 服务器等额外工具。例行任务内置了对您代码仓库和[连接器](https://claude.com/connectors)的访问权限，因此您可以将自动化流程打包，并设置其按计划或触发条件运行。

## 工作原理

### 定时例行任务

为 Claude Code 提供一段提示词和一个执行频率（每小时、每晚或每周），它就会按照该计划自动运行：

```
每晚凌晨 2 点：从 Linear 中拉取优先级最高的缺陷，尝试修复，并创建一个草稿 PR。
```

如果您在 CLI 中使用 [/schedule](https://code.claude.com/docs/en/scheduled-tasks#compare-scheduling-options)，这些任务现在即为定时例行任务。

### API 例行任务

您还可以将例行任务配置为通过 API 调用触发。每个例行任务都会获得独立的端点和身份验证令牌。发送一条 POST 请求，即可获取一个会话链接。将 Claude Code 接入您的告警系统、部署钩子或内部工具——任何可以发起 HTTP 请求的场景均可：

```
读取告警负载，定位所属服务，并将一份分诊摘要连同建议的首步操作发布到 #oncall 频道。
```

如果您在 Claude 平台上构建的是云端托管的智能体而非自动化 Claude Code，[Claude 托管智能体的定时部署](https://claude.com/blog/whats-new-in-claude-managed-agents) 可以让您自己的智能体实现同样的按计划运行行为。

### Webhook 例行任务，率先支持 GitHub

将例行任务订阅到 GitHub 仓库事件上，使其在事件发生时自动触发。Claude 会为每个匹配您筛选条件的 PR 创建一个新会话并运行您的例行任务。

```
请标记涉及 /auth-provider 模块的 PR。对该模块的任何更改都需要进行摘要并发布到 #auth-changes 频道。
```

Claude 会为每个 PR 开启一个会话，并持续将该 PR 的后续更新推送至该会话，以便处理评论、CI 失败等后续事项。

我们计划在未来将基于 Webhook 的例行任务扩展至更多事件源。

## 团队正在构建的应用

早期用户在使用例行任务时，已经涌现出几种常见模式：

### 定时例行任务

* 待办事项管理：每晚对新缺陷进行分类处理，添加标签、分配负责人，并将摘要发布到 Slack
* 文档漂移检测：每周扫描已合并的 PR，标记引用了已变更 API 的文档，并创建更新 PR

### API 例行任务

* 部署验证：CI/CD 流水线在每次部署后发送请求，Claude 对新构建执行冒烟测试，扫描错误日志以检测回归问题，并在发布频道中发布通过/不通过的结论
* 告警分诊：将 Datadog 指向例行任务的端点，Claude 拉取调用链路，将其与近期部署进行关联分析，在值班人员打开页面之前便已准备好修复草稿
* 反馈处理：文档反馈组件或内部仪表盘提交报告后，Claude 在代码仓库中针对该问题开启一个包含上下文的会话，并起草修改方案

### GitHub 例行任务

* 多语言库移植：每次有 PR 合并到 Python SDK 时，触发例行任务将变更移植到对应的 Go SDK，并创建一个匹配的 PR
* 定制化代码审查：PR 创建时，运行团队自定义的安全与性能检查清单，在人工审查者介入之前留下内联评论

## 快速上手

例行任务现已面向启用 [Claude Code 网页版](https://code.claude.com/docs/en/claude-code-on-the-web#who-can-use-claude-code-on-the-web) 的 Pro、Max、Team 和 Enterprise 套餐用户开放。前往 [claude.ai/code](http://claude.ai/code) 创建您的第一个例行任务，或在 CLI 中输入 /schedule。

例行任务与交互式会话一样，会消耗订阅使用额度。此外，例行任务设有每日上限：Pro 用户每天最多可运行 5 个例行任务，Max 用户每天最多可运行 15 个，Team 和 Enterprise 用户每天最多可运行 25 个。超出上述限额后，可通过额外用量继续运行更多例行任务。[请参阅文档](http://code.claude.com/docs/en/routines)了解更多详情。
