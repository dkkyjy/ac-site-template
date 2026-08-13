---
title: "使用 Claude Code 自动化安全审查"
description: "今天，我们在 Claude Code 中引入了自动化安全审查功能。通过使用我们的 GitHub Actions 集成和新的 /security-review 命令，开发者可以轻松地让 Claude 识别安全问题——然后让它修复这些问题。"
pubDate: "2025-08-06"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/automate-security-reviews-with-claude-code"
---
            今天，我们在 Claude Code 中引入了自动化安全审查功能。通过使用我们的 GitHub Actions 集成和新的 /security-review 命令，开发者可以轻松地让 Claude 识别安全问题——然后让它修复这些问题。

随着开发者越来越依赖 AI 来加快交付速度并构建更复杂的系统，确保代码安全性变得更为关键。这些新功能让你能够将安全审查集成到现有工作流程中，帮助你在漏洞进入生产环境之前捕获它们。

### 在终端中审查代码漏洞

新的 /security-review 命令让你可以在提交代码之前，从终端运行临时安全分析。在 Claude Code 中运行该命令，Claude 将搜索你的代码库以查找潜在漏洞，并提供所发现问题的详细解释。

此命令使用专门针对安全的提示词，检查常见的漏洞模式，包括：

* SQL 注入风险
* 跨站脚本（XSS）漏洞
* 身份验证和授权缺陷
* 不安全的数据处理
* 依赖项漏洞

你还可以要求 Claude Code 在识别每个问题后实施修复。这使安全审查保持在你的内部开发循环中，在问题最容易修复的早期阶段捕获它们。

### 自动化新拉取请求的安全审查

Claude Code 的新 GitHub Action 将安全审查更进一步，在每次打开拉取请求时自动进行分析。配置后，该操作将：

* 在新拉取请求上自动触发
* 审查代码更改是否存在安全漏洞
* 应用可自定义规则以过滤误报和已知问题
* 在 PR 中内联发布评论，指出发现的任何问题，包括修复建议

这为整个团队创建了一致的安全审查流程，确保没有代码在未经过基线安全审查的情况下进入生产环境。该操作与你现有的 CI/CD 管道集成，并可根据团队的安全策略进行自定义。

### 在 Anthropic 改进产品安全性

我们自己也正在使用这些功能来帮助保护团队交付到生产环境的代码安全，包括 Claude Code 本身。自设置 GitHub Action 以来，它已经捕获了我们自己代码中的安全漏洞，并阻止了它们被发布。

例如，上周，我们的团队为内部工具构建了一个新功能，该功能依赖于启动一个旨在接受本地连接的本地 HTTP 服务器。GitHub Action 识别了一个可通过 DNS 重绑定利用的远程代码执行漏洞，并在 PR 合并之前修复了它。

在另一个案例中，一位工程师构建了一个代理系统，以实现内部凭据的安全管理。GitHub Action 自动标记出该代理容易受到 SSRF 攻击，我们迅速修复了此问题。

### 开始使用

这两个功能现已对所有 Claude Code 用户开放。要开始使用自动化安全审查：

* **对于 /security-review 命令**：只需将 Claude Code 更新到最新版本，并在项目目录中运行 /security-review。[查看文档](https://github.com/anthropics/claude-code-security-review/tree/main?tab=readme-ov-file#security-review-slash-command) 以自定义你自己的命令版本
* **对于 GitHub Action**：[查看文档](https://github.com/anthropics/claude-code-security-review) 获取分步安装和配置说明
