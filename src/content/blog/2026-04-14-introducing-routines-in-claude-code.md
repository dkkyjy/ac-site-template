---
title: "在 Claude Code 中引入 routines"
description: "今天，我们以研究预览版的形式在 Claude Code 中引入 routines。Routine 是一种 Claude Code 自动化功能，您只需配置一次（包括提示词、仓库和连接器），然后即可按计划、通过 API 调用或响应事件来运行。Routines 运行在 [Claude Code 的 Web 基础设施](https://code.claude.com/docs/en/claude-code"
pubDate: "2026-04-14"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/introducing-routines-in-claude-code"
---
            今天，我们以研究预览版的形式在 Claude Code 中引入 routines。Routine 是一种 Claude Code 自动化功能，您只需配置一次（包括提示词、仓库和连接器），然后即可按计划、通过 API 调用或响应事件来运行。Routines 运行在 [Claude Code 的 Web 基础设施](https://code.claude.com/docs/en/claude-code-on-the-web)上，因此无需依赖您的笔记本电脑保持开机状态。

开发者已经在使用 Claude Code 来自动化软件开发生命周期，但在此之前，他们需要自行管理 cron 作业、基础设施以及 MCP 服务器等额外工具。Routines 附带了对您的仓库和[连接器](https://claude.com/connectors)的访问权限，因此您可以打包自动化任务，并设置它们按计划运行或通过事件触发。

## 工作原理

### 计划型 routines

为 Claude Code 提供一个提示词和一个执行频率（每小时、每晚或每周），它就会按该计划运行：

```
每晚凌晨2点：从 Linear 拉取最高优先级的 bug，尝试修复，并打开一个草稿 PR。
```

如果您在 CLI 中使用 [/schedule](https://code.claude.com/docs/en/scheduled-tasks#compare-scheduling-options)，这些任务现在就是计划型 routines。

### API routines

您还可以配置通过 API 调用触发的 routines。每个 routine 都有自己专属的端点和认证令牌。POST 一条消息，即可获得一个会话 URL。将 Claude Code 接入您的告警系统、部署钩子、内部工具——任何可以发起 HTTP 请求的地方：

```
读取告警负载，找到所属服务，并在 #oncall 频道发布一个分类摘要，同时提供建议的第一步操作。
```

### Webhook routines，从 GitHub 开始

订阅一个 routine，使其在 GitHub 仓库事件发生时自动启动。Claude 将为每个符合过滤条件的 PR 创建一个新会话，并运行您的 routine。

```
请标记涉及 /auth-provider 模块的 PR。任何对此模块的更改都需要进行总结并发布到 #auth-changes 频道。
```

Claude 为每个 PR 打开一个会话，并持续将来自该 PR 的更新反馈给会话，以便处理评论和 CI 失败等后续事项。

我们计划在未来将基于 webhook 的 routines 扩展到更多事件源。

## 团队正在构建的内容

早期用户在使用 routines 时，出现了一些常见模式：

### 计划型 routines

* 积压任务管理：每晚分类新问题，打标签、分配，并在 Slack 上发布总结
* 文档漂移：每周扫描已合并的 PR，标记引用了已变更 API 的文档，并打开更新 PR

### API routines

* 部署验证：您的 CD 流水线在每次部署后发送通知，Claude 对新构建运行冒烟检查，扫描错误日志以发现回归问题，并在发布频道发布通过/不通过
* 告警分类：将 Datadog 指向 routine 的端点，Claude 拉取追踪信息，将其与最近的部署关联起来，并在值班人员打开页面之前准备好一个草稿修复方案
* 反馈处理：文档反馈小部件或内部仪表板提交报告，Claude 在问题上下文中打开一个针对该仓库的会话，并起草修改方案

### GitHub routines

* 库移植：每个合并到 Python SDK 的 PR 都会触发一个 routine，将该变更移植到并行的 Go SDK，并打开一个匹配的 PR
* 定制代码审查：当 PR 打开时，运行您团队自己的安全性和性能检查清单，在人工审查者查看之前留下行内评论

## 开始使用

Routines 现已面向 Pro、Max、Team 和 Enterprise 套餐的 Claude Code 用户提供，前提是已启用 [Claude Code on the web](https://code.claude.com/docs/en/claude-code-on-the-web#who-can-use-claude-code-on-the-web)。前往 [claude.ai/code](http://claude.ai/code) 创建您的第一个 routine，或在 CLI 中输入 /schedule。

Routines 与交互式会话一样，会消耗订阅使用额度。此外，routines 还有每日限制：Pro 用户每天最多运行 5 个 routines，Max 用户每天最多运行 15 个 routines，Team 和 Enterprise 用户每天最多运行 25 个 routines。您可以通过额外使用额度来运行超出这些限制的额外 routines。更多信息请[参阅文档](http://code.claude.com/docs/en/routines)。
