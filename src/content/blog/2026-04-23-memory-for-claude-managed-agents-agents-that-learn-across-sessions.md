---
title: "Memory for Claude Managed Agents: agents that learn across sessions"
description: "---"
pubDate: "2026-04-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents-memory"
---

# Memory for Claude Managed Agents: agents that learn across sessions

            ---

            [Claude 托管代理](https://claude.com/blog/claude-managed-agents) 的记忆功能今日以公测版形式推出。您的代理现在可以从每次会话中学习，利用一个兼顾性能与灵活性的智能优化记忆层。由于记忆以文件形式存储，开发者可以导出它们、通过 API 进行管理，并完全控制代理保留哪些内容。

## 记忆在 Claude 托管代理中的工作原理

Claude 托管代理的记忆是一个内置的、智能优化的记忆层，让代理从每次会话中学习。该记忆针对内部基准进行了优化，适用于那些能够跨会话改进并相互分享所学内容的长期运行代理。

我们发现，当记忆建立在代理已经使用的工具之上时，代理的效果最佳。Claude 托管代理的记忆直接挂载到文件系统上，因此 Claude 可以依赖同样的 bash 和代码执行能力——这些能力使其在代理任务中表现出色。借助基于文件系统的记忆，[我们最新的模型](https://www.anthropic.com/news/claude-opus-4-7#:~:text=Memory.%20Opus%204.7%20is%20better%20at%20using%20file%20system%2Dbased%20memory.%20It%20remembers%20important%20notes%20across%20long%2C%20multi%2Dsession%20work%2C%20and%20uses%20them%20to%20move%20on%20to%20new%20tasks%20that%2C%20as%20a%20result%2C%20need%20less%20up%2Dfront%20context.) 能够保存更全面、组织更完善的记忆，并能更精准地判断在特定任务中该记住什么。

## 面向生产级代理的可移植记忆

记忆专为企业级部署而设计，具备作用域权限、审计日志和完整的程序化控制。存储库可以在多个代理之间共享，且具有不同的访问范围。例如，一个组织级存储库可能为只读，而每个用户级存储库则允许读写。多个代理可以同时操作同一个存储库，而不会互相覆盖。

记忆是文件，可以通过 API 导出和独立管理，让开发者拥有完全控制权。所有变更都会通过详细的审计日志记录下来，因此您可以查明记忆来自哪个代理和会话。您可以回滚到更早的版本，或从历史记录中删除内容。更新还会在 [Claude 控制台](https://platform.claude.com/) 中作为会话事件呈现，使开发者能够追溯代理学到了什么以及信息来源。

## **团队正在构建的内容**

团队一直在利用记忆来闭合反馈循环、加快验证速度，并取代自定义的检索基础设施：

* **Netflix** 的代理能够跨会话携带上下文，包括需要多轮交互才能发现的信息以及人类在对话中途给出的修正，而无需手动更新提示词和技能。
* **Rakuten** 基于任务的长期运行代理利用记忆从每次会话中学习，避免重复过去的错误，将首次通过的错误率降低了 97%，且所有这些都在工作区限定的可观察边界内完成。
* **Wisedocs** 在托管代理上构建了文档验证流水线，利用跨会话记忆来识别并记住重复出现的文档问题，将验证速度提升了 30%。
* **Ando** 正在托管代理上构建其工作场所消息平台，捕捉每个组织的交互方式，而不是自行构建记忆基础设施。

## 入门指南

托管代理的记忆功能现已在 Claude 平台上以公测版形式推出。请访问 [Claude 控制台](https://platform.claude.com/workspaces/default/memory-stores) 或使用我们的新 CLI 部署您的第一个带有记忆功能的代理。查阅[文档](https://platform.claude.com/docs/en/managed-agents/memory)以了解更多信息。
