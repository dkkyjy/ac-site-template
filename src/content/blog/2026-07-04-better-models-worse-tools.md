---
title: "更好的模型：更差的工具"
description: "更好的模型：更差的工具"
pubDate: "2026-07-04"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/4/better-models-worse-tools"
---

> *摘要：更好的模型：更差的工具
阿明报告了他在捣鼓 Pi 时遇到的一个奇怪问题：*

*简而言之，较新的 Claude 模型有时会调用 Pi 的编辑工具，并在嵌套的 `edits[]` 数组中填入额外的、凭空捏造的字段。*

2026 年 7 月 4 日 - 链接博客

**[更好的模型：更差的工具](https://lucumr.pocoo.org/2026/7/4/better-models-worse-tools/)**。阿明报告了他在捣鼓 Pi 时遇到的一个奇怪问题：

> 简而言之，较新的 Claude 模型有时会调用 Pi 的编辑工具，并在嵌套的 `edits[]` 数组中填入额外的、凭空捏造的字段。而且不是 Haiku 或某个小模型：是 Opus 4.8。编辑本身通常是正确的，但参数与模式不匹配，因为模型捏造了不存在的键，Pi 因此拒绝该工具调用并要求重试。
>
> 这本身并不太令人惊讶，因为模型有时会发出格式错误的工具调用。特别是小模型。让我惊讶的是，随着较新的 Anthropic 模型的出现，这个问题变得更糟了——Opus 4.8 和 Sonnet 5 都表现出这个问题，但旧模型却没有。换句话说，该系列的最先进模型在特定工具模式上的表现比它们的旧版本更差。

阿明推测，这是因为较新的 Anthropic 模型经过了专门训练（可能通过强化学习），以更好地使用 Claude Code 内置的编辑工具。这产生了不幸的副作用：其他编码工具框架（如 Pi）可能会发现它们自己的自定义编辑工具更有可能被错误使用。

Claude 的编辑工具使用[搜索并替换](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool#str-replace)。OpenAI 的 Codex 改用[apply_patch 机制](https://developers.openai.com/api/docs/guides/tools-apply-patch)，并且 OpenAI 过去曾谈到他们如何训练模型有效使用该工具。

这是否意味着像 Pi 这样的第三方编码工具框架应该实现多个编辑工具，以便根据用户选择的底层模型使用性能最好的那个？

发布于 [2026 年 7 月 4 日](/2026/Jul/4/) 晚上 10 点 53 分
