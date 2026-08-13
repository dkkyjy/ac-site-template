---
title: "Claude 托管代理的内置记忆功能"
description: "[Claude 托管代理](https://claude.com/blog/claude-managed-agents)的记忆功能即日起以公开测试版形式提供。您的代理现在可以从每次会话中学习，利用经过智能优化的记忆层，在性能与灵活性之间取得平衡。由于记忆以文件形式存储，开发者可以导出它们、通过 API 进行管理，并对代理保留的内容保持完全控制。"
pubDate: "2026-04-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents-memory"
---
[Claude 托管代理](https://claude.com/blog/claude-managed-agents)的记忆功能即日起以公开测试版形式提供。您的代理现在可以从每次会话中学习，利用经过智能优化的记忆层，在性能与灵活性之间取得平衡。由于记忆以文件形式存储，开发者可以导出它们、通过 API 进行管理，并对代理保留的内容保持完全控制。

## 跨会话学习的代理

托管代理将生产基础设施与针对性能优化的控制框架相结合。记忆功能扩展了这一能力：它针对内部基准进行了优化，适用于跨会话不断改进并相互分享所学内容的长期运行代理。

我们发现，当记忆建立在代理已使用的工具之上时，代理的效果最佳。托管代理上的记忆功能直接挂载到文件系统上，因此 Claude 可以依赖同样的 bash 和代码执行能力——这些能力使其在代理任务中表现出色。借助基于文件系统的记忆，[我们最新的模型](https://www.anthropic.com/news/claude-opus-4-7#:~:text=Memory.%20Opus%204.7%20is%20better%20at%20using%20file%20system%2Dbased%20memory.%20It%20remembers%20important%20notes%20across%20long%2C%20multi%2Dsession%20work%2C%20and%20uses%20them%20to%20move%20on%20to%20new%20tasks%20that%2C%20as%20a%20result%2C%20need%20less%20up%2Dfront%20context.)能够保存更全面、组织更完善的记忆，并且对于特定任务应该记住什么内容更具辨别力。

## 面向生产级代理的可移植记忆

记忆功能专为企业级部署而设计，具备作用域权限、审计日志和完整的编程控制能力。记忆存储可以在多个具有不同访问范围的代理之间共享。例如，组织级存储可能为只读，而每个用户的存储则允许读写操作。多个代理可以同时针对同一存储进行操作，而不会相互覆盖。

记忆是以文件形式存在的，可以通过 API 导出和独立管理，让开发者拥有完全控制权。所有更改都会通过详细的审计日志进行追踪，因此您可以了解记忆来自哪个代理和会话。您可以回滚到早期版本或从历史记录中删除内容。更新还会以会话事件的形式显示在 [Claude 控制台](https://platform.claude.com/)中，以便开发者追踪代理学到了什么以及信息来源。

## **团队正在构建的内容**

各团队一直在利用记忆功能来关闭反馈循环、加速验证，并取代自定义检索基础设施：

* **Netflix** 的代理跨会话携带上下文，包括需要多次交互才能发现的洞察以及人类在对话中途给出的修正，无需手动更新提示和技能。
* **Rakuten** 的基于任务的长期运行代理利用记忆功能从每次会话中学习，避免重复过去的错误，将首次通过错误率降低了 97%，所有这些都在工作区范围内的可观察边界内完成。
* **Wisedocs** 在托管代理上构建了文档验证管道，利用跨会话记忆来发现并记住重复出现的文档问题，将验证速度提高了 30%。
* **Ando** 正在托管代理上构建其工作场所消息平台，捕获每个组织的交互方式，而不是自行构建记忆基础设施。

## 开始使用

托管代理上的记忆功能现已在 Claude 平台上以公开测试版形式提供。访问 [Claude 控制台](https://platform.claude.com/workspaces/default/memory-stores)或使用我们的新 CLI 来部署您的第一个带有记忆功能的代理。查阅[文档](https://platform.claude.com/docs/en/managed-agents/memory)以了解更多信息。
