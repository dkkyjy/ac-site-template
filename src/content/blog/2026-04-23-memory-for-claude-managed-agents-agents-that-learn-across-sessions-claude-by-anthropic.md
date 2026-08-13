---
title: "Memory for Claude Managed Agents: agents that learn across sessions | Claude by Anthropic"
description: "---"
pubDate: "2026-04-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents-memory"
---

# Memory for Claude Managed Agents: agents that learn across sessions | Claude by Anthropic

---

[Claude Managed Agents](https://claude.com/blog/claude-managed-agents) 的内存功能现已公开发布测试版。您的代理现在可以从每次会话中学习，利用一个兼顾性能与灵活性的智能优化内存层。由于记忆以文件形式存储，开发者可以导出它们、通过 API 进行管理，并对代理保留的内容保持完全控制。

## Claude Managed Agents 中内存的工作原理

Claude Managed Agents 的内存是一个内置的、智能优化的内存层，让代理能够从每次会话中学习。该内存针对内部基准进行了优化，适用于那些跨会话持续改进并共享所学内容的长期运行代理。

我们发现，当内存建立在代理已使用的工具之上时，代理的效果最佳。Claude Managed Agents 的内存直接挂载到文件系统上，因此 Claude 可以依赖同样使其在代理任务中高效工作的 bash 和代码执行能力。借助基于文件系统的内存，[我们最新的模型](https://www.anthropic.com/news/claude-opus-4-7#:~:text=Memory.%20Opus%204.7%20is%20better%20at%20using%20file%20system%2Dbased%20memory.%20It%20remembers%20important%20notes%20across%20long%2C%20multi%2Dsession%20work%2C%20and%20uses%20them%20to%20move%20on%20to%20new%20tasks%20that%2C%20as%20a%20result%2C%20need%20less%20up%2Dfront%20context.) 能够保存更全面、更有条理的记忆，并更精准地判断在特定任务中需要记住什么。

## 面向生产级代理的可移植内存

内存专为企业级部署设计，具备作用域权限、审计日志和完整的程序化控制。存储空间可以跨多个代理共享，并赋予不同的访问范围。例如，组织级存储空间可能为只读，而每个用户的存储空间则允许读写。多个代理可以同时操作同一个存储空间而不会互相覆盖。

记忆是可通过 API 导出和独立管理的文件，为开发者提供完全控制权。所有变更都会通过详细的审计日志进行跟踪，因此您可以了解某条记忆来自哪个代理和哪个会话。您可以回滚到早期版本，或从历史记录中删除内容。更新还会以会话事件的形式显示在 [Claude Console](https://platform.claude.com/) 中，从而让开发者能够追溯代理学到了什么以及信息来源。

## **团队正在构建的内容**

各团队正在利用内存来闭环反馈、加速验证并替代自定义检索基础设施：

* **Netflix** 的代理能够在会话间携带上下文，包括需要多次交互才能发现的洞察以及人类在对话中途给出的修正，从而无需手动更新提示词和技能。
* **Rakuten** 的基于任务的长期运行代理利用内存从每次会话中学习，避免重复过去的错误，首次通过错误率降低了 97%，所有这些都在工作空间范围内、可观测的边界内完成。
* **Wisedocs** 将其文档验证流程构建在 Managed Agents 之上，利用跨会话内存来发现并记住反复出现的文档问题，将验证速度提升了 30%。‍
* **Ando** 正在基于 Managed Agents 构建其工作场所消息平台，直接捕获每个组织的交互方式，而无需自己构建内存基础设施。
