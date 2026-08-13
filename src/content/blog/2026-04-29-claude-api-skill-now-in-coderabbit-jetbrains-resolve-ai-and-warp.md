---
title: "Claude API 技能现已登陆 CodeRabbit、JetBrains、Resolve AI 和 Warp"
description: "今天，CodeRabbit、JetBrains、Resolve AI 和 Warp 正在集成 [claude-api 技能](https://github.com/anthropics/skills/tree/main/skills/claude-api)，让开发者无论在哪里构建，都能获得生产就绪的 Claude API 代码。该技能于今年三月首次在 Claude Code 中引入，现已覆盖更多开"
pubDate: "2026-04-29"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-api-skill"
---
            今天，CodeRabbit、JetBrains、Resolve AI 和 Warp 正在集成 [claude-api 技能](https://github.com/anthropics/skills/tree/main/skills/claude-api)，让开发者无论在哪里构建，都能获得生产就绪的 Claude API 代码。该技能于今年三月首次在 Claude Code 中引入，现已覆盖更多开发者已在使用中的工具。

## 使用 Claude API 技能进行构建

`claude-api` 技能捕捉了使 Claude API 代码良好运行的关键细节，例如哪种代理模式适合特定任务、模型代际之间哪些参数会发生变化，以及何时应用提示缓存。其结果是更少的错误、更好的缓存、更清晰的代理模式以及更平滑的模型迁移。

该技能会随着我们 SDK 的更新而保持最新。当新模型发布或 API 获得新功能时，Claude 已经知晓。

在任何提供该技能的地方，都可以要求 Claude 执行以下操作：

*   **"提高我的缓存命中率。"** 该技能会应用许多开发者忽略的提示缓存规则。
*   **"为我的代理添加上下文压缩。"** 它会引导你了解我们文档中的压缩原语和代理模式。
*   **"将我升级到最新的 Claude 模型。"** Claude 会审查你的代码，并引导你更新模型名称、提示词和努力设置，以适应像 [Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7) 这样的新模型。在 Claude Code 中，你也可以直接使用 `/claude-api migrate` 命令运行此操作。**‍**
*   **"为我的行业构建一个深度研究代理。"** Claude 会引导你配置 [Claude 托管代理](https://platform.claude.com/docs/en/managed-agents/overview)，使长期运行的研究只需几个提示词，而无需一个自定义项目。在 Claude Code 中，你也可以直接使用 `/claude-api managed-agents-onboard` 命令运行此操作。

## 针对基于 Claude 的编码代理

任何编码代理都可以集成 `claude-api` 技能，为其用户提供关于 Claude API 的专业知识。如果你正在构建一个开发者编写 Claude API 代码的工具，该技能是开源的，位于 [anthropics/skills](https://github.com/anthropics/skills/tree/main/skills/claude-api)。我们的集成指南会引导你在大约 20 行 CI 代码中完成设置，并且该技能会自动保持最新。

## 开始使用

该技能已存在于 [Claude Code](https://code.claude.com/docs/en/overview)、[CodeRabbit](https://www.coderabbit.ai/)、[JetBrains](https://www.jetbrains.com/)、[Junie](https://www.jetbrains.com/junie/)、[Resolve AI](https://resolve.ai/) 和 [Warp](https://www.warp.dev/) 中。要了解更多信息，请参阅 [claude-api 技能文档](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/claude-api-skill)。
