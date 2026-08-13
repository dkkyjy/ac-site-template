---
title: "使用 Claude Code 自动化安全审查 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-08-06"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/automate-security-reviews-with-claude-code"
---

# 使用 Claude Code 自动化安全审查 | Anthropic 的 Claude

---

今天，我们推出了 Claude Code 中的自动化安全审查功能。借助我们的 GitHub Actions 集成和新的 /security-review 命令，开发人员可以轻松要求 Claude 识别安全问题——然后让它修复这些问题。

随着开发人员越来越依赖 AI 来加快交付速度并构建更复杂的系统，确保代码安全变得至关重要。这些新功能让您可以将安全审查集成到现有的工作流程中，帮助您在漏洞进入生产环境之前就将其捕获。

### 从终端审查代码中的漏洞

新的 /security-review 命令让您可以在提交代码之前从终端运行临时安全分析。在 Claude Code 中运行该命令，Claude 将搜索您的代码库以查找潜在漏洞，并提供已发现问题的详细说明。

此命令使用专门的、以安全为重点的提示，检查常见的漏洞模式，包括：

* SQL 注入风险
* 跨站脚本（XSS）漏洞
* 身份验证和授权缺陷
* 不安全的数据处理
* 依赖项漏洞

您还可以要求 Claude Code 在识别出每个问题后实施修复。这样，安全审查就保留在您的内部开发循环中，可以在问题最容易修复时尽早发现它们。

### 自动化新拉取请求的安全审查

Claude Code 的新 GitHub Action 进一步推进了安全审查，它会在每次拉取请求打开时自动进行分析。配置后，该操作将：

* 在新拉取请求上自动触发
* 审查代码更改中的安全漏洞
* 应用可自定义的规则来过滤误报和已知问题
* 在拉取请求中直接发布内联评论，包含发现的任何问题以及修复建议

这为整个团队创建了统一的安全审查流程，确保没有代码未经基本安全审查就进入生产环境。该操作与您现有的 CI/CD 流水线集成，并且可以根据您团队的安全策略进行自定义。

### 在 Anthropic 提升产品安全性

我们自己在使用这些功能来帮助保护我们团队交付到生产环境的代码，包括 Claude Code 本身。自设置 GitHub Action 以来，它已经在我们的代码中发现了安全漏洞，并阻止了它们被发布。

例如，上周，我们的团队为内部工具构建了一项新功能，该功能依赖于启动一个旨在接受本地连接的本地 HTTP 服务器。GitHub Action 识别出了一个可通过 DNS 重新绑定利用的远程代码执行漏洞，并且在拉取请求合并之前就修复了该漏洞。

另一个例子中，一位工程师构建了一个代理系统来实现内部凭证的安全管理。GitHub Action 自动标记出该代理容易受到 SSRF 攻击，我们迅速修复了这个问题。

### 开始使用

这两个功能现在对所有 Claude Code 用户开放。要开始使用自动化安全审查：

* **对于 /security-review 命令：** 只需将 Claude Code 更新到最新版本，然后在项目目录中运行 /security-review。[查看文档](https://github.com/anthropics/claude-code-security-review/tree/main?tab=readme-ov-file#security-review-slash-command) 以自定义您自己的命令版本
* **对于 GitHub Action：** [查看文档](https://github.com/anthropics/claude-code-security-review) 了解分步安装和配置说明
