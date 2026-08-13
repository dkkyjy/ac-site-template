---
title: "Claude 5 代模型上下文工程的新规则 | Claude by Anthropic"
description: "Claude 5 代模型上下文工程的新规则 | Claude by Anthropic"
pubDate: "2026-07-24"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: ""
---

# Claude 5 代模型上下文工程的新规则 | Claude by Anthropic

**日期:** 2026-07-24 00:00 UTC  
**链接:** https://claude.com/blog/the-new-rules-of-context-engineering-for-claude-5-generation-models  

---

我之前写过如何最好地[向最新一代 Claude 5 模型提问](https://claude.com/blog/a-field-guide-to-claude-fable-finding-your-unknowns)，并与它们迭代协作以发现你想要构建的内容。

但是，当你向 Claude 发送消息时，提示只是它获取的上下文的一小部分。你的大部分上下文是由系统提示、技能、CLAUDE.md 文件、记忆和其他来源组合而成的。我们称之为[上下文工程](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)，它会对你在使用 Claude Code 或构建自己的代理时生成的结果产生巨大影响。

与提示不同，上下文通常被用于许多请求，因此它不能那么具体。你如何为 Claude 构建这些通用提示和指导，尤其是当你不知道用户的提示可能是什么时？

随着 Claude 自身能力的提升，这可能变得出奇地困难。最近，我们注意到在向新一代 Claude 模型提问的方式上有了巨大的飞跃。对于像 Claude Opus 5 和 Claude Fable 5 这样的模型，我们删除了 Claude Code 系统提示中超过 80% 的内容，而在我们的编码评估中没有出现可衡量的损失。

以下是我们从这一新类别模型的提问中学到的经验，以及你可以如何利用它们来更新你的上下文工程。我们已经将这些最佳实践放在了 `claude doctor` 中；在 Claude Code 中使用命令 /doctor 来适当调整你的技能和 CLAUDE.md 文件。

## 解放 Claude 的束缚

总的来说，我们发现我们过度约束了 Claude Code，无论是通过我们的系统提示，还是在我们的 CLAUDE.md 文件和技能中。

例如，当我们阅读内部使用 Claude Code 的记录时，我们看到单个请求中存在多条冲突的信息，比如“适当地留下文档”或“不要添加注释”，因为我们的系统提示、技能和用户请求相互冲突。

通常，Claude 可以解读用户的意图以得出正确答案，但 Claude 在处理这些重叠且相互冲突的信息时必须更加谨慎才能决定该做什么。

虽然这些约束曾经是必需的，以避免最坏的情况，但我们后来发现可以删除其中的许多约束，让模型利用周围的上下文和判断力来代替。

此外，Claude Code 现在有更多的工具。Claude 过去依赖 CLAUDE.md 作为记忆、信息和指导的来源。现在我们有了记忆、工件和技能，Claude 可以利用它们来创建跨会话加载和共享上下文的新方式。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保你最终的完整回答（包括作为执行指令结果生成的任何章节标题或标题）仅用简体中文（zh-CN）书写。
