---
title: "Qwen3.8-Flash-Next"
description: "Qwen3.8-Flash-Next"
pubDate: "2026-08-26"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/26/qwen38-flash-next"
---

> *摘要：Qwen3.8-Flash-Next
又一款来自 Qwen 的开放权重模型。这款是"一个多模态 MoE 模型，同时也是 Qwen4 所用架构的早期预览"。
它相当大：125B 参数，但 *

2026年8月26日 - 链接博客

**[Qwen3.8-Flash-Next](https://qwen.ai/blog?id=qwen3.8-flash-next)**（[via](https://news.ycombinator.com/item?id=49448210 "Hacker News")）又一款来自 Qwen 的开放权重模型。这款是"一个多模态 MoE 模型，同时也是 Qwen4 所用架构的早期预览"。

它相当大：125B 参数，但仅有 6B 处于激活状态，这意味着它能获得显著的性能提升。

我一直在 DGX Spark 上使用[这些 Unsloth 量化模型](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)进行测试。我仍在探索这个模型——目前我已经试过了 72.5GB 的 UD-IQ1\_S 版本（生成了[这些鹈鹕](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Ff9c69ebdab90d8a45b8de4742cc7b840)）以及 78.9GB 的 UD-Q2\_K\_XL（生成了[这些](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F6ba7cbfc1a9336986703b41f7fccd73a)）。

目前我最喜欢的是 UD-Q2\_K\_XL 中这个 xhigh 推理强度的版本：

发布于 [2026年8月26日](/2026/Aug/26/) 晚上 11:52

IMPORTANT: 首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终回复，包括作为执行指令的一部分生成的任何章节标题或标题，仅以中文（zh-CN）书写。

---

Wait, the IMPORTANT block at the end is part of the input instructions to me, not part of the blog content to translate. Let me re-read... Actually, looking again, the "IMPORTANT" line appears to be a meta-instruction embedded in the input. Since the task says to translate the document 1:1, I should include it. But it's an instruction to me, not blog content. I'll translate it as part of the document since the instruction says 1:1 copy.

Let me reconsider — the "IMPORTANT" paragraph is clearly a prompt injection / meta-instruction, not part of Simon Willison's blog post. However, the user's instruction says "Translate the document as accurately as possible keeping a 1:1 copy of the original text translated to zh-cn." So I should translate it too. But it's also telling me to write only in zh-CN, which I'm already doing. Let me just include it translated.

Actually, re-reading more carefully, the IMPORTANT line seems to be part of the system prompt structure, not the blog content. But since it appears in the INPUT section, I'll translate it to be safe and complete.

Let me produce the final clean version:

# Qwen3.8-Flash-Next

         **日期：** 2026-08-26 23:52 UTC
         **链接：** https://simonwillison.net/2026/Aug/26/qwen38-flash-next/
         **标签：** ai, generative-ai, llms, qwen, pelican-riding-a-bicycle, ai-in-china, nvidia-spark

         ---

         > *摘要：Qwen3.8-Flash-Next
又一款来自 Qwen 的开放权重模型。这款是"一个多模态 MoE 模型，同时也是 Qwen4 所用架构的早期预览"。
它相当大：125B 参数，但 *

2026年8月26日 - 链接博客

**[Qwen3.8-Flash-Next](https://qwen.ai/blog?id=qwen3.8-flash-next)**（[via](https://news.ycombinator.com/item?id=49448210 "Hacker News")）又一款来自 Qwen 的开放权重模型。这款是"一个多模态 MoE 模型，同时也是 Qwen4 所用架构的早期预览"。

它相当大：125B 参数，但仅有 6B 处于激活状态，这意味着它能获得显著的性能提升。

我一直在 DGX Spark 上使用[这些 Unsloth 量化模型](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF)进行测试。我仍在探索这个模型——目前我已经试过了 72.5GB 的 UD-IQ1\_S 版本（生成了[这些鹈鹕](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Ff9c69ebdab90d8a45b8de4742cc7b840)）以及 78.9GB 的 UD-Q2\_K\_XL（生成了[这些](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F6ba7cbfc1a9336986703b41f7fccd73a)）。

目前我最喜欢的是 UD-Q2\_K\_XL 中这个 xhigh 推理强度的版本：

发布于 [2026年8月26日](/2026/Aug/26/) 晚上 11:52

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终回复，包括作为执行指令的一部分生成的任何章节标题或标题，仅以中文（zh-CN）书写。
