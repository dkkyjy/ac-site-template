---
title: "llm-anthropic 0.27"
description: "llm-anthropic 0.27"
pubDate: "2026-08-24"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/24/llm-anthropic"
---

> *摘要：发布：llm-anthropic 0.27
        本次 LLM 的 Anthropic 插件发布主要提供了对近期发布的 anthropic v1.0.0 Python 库的兼容性支持，该库将底层从 httpx 切换为 *

2026年8月24日

[发布](/elsewhere/release/)
[llm-anthropic 0.27](https://github.com/simonw/llm-anthropic/releases/tag/0.27)
— 通过 LLM 访问 Anthropic 的模型，包括 Claude 系列

本次 LLM 的 Anthropic 插件发布主要提供了对近期发布的 [anthropic v1.0.0](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v1.0.0) Python 库的兼容性支持，该库将底层从 `httpx` 切换为 [httpx2](https://github.com/pydantic/httpx2)。OpenAI 在两周前的 [v3.0.0 版本](https://github.com/openai/openai-python/releases/tag/v3.0.0)中也做出了相同的更改。

Anthropic 提供了这份[迁移指南](https://github.com/anthropics/anthropic-sdk-python/blob/v1.0.0/MIGRATION.md)用于升级到 1.0，因此我在 Claude Code 中向 Fable 5 发出了以下提示：

> `Upgrade to anthropic>=1 - read https://raw.githubusercontent.com/anthropics/anthropic-sdk-python/refs/heads/main/MIGRATION.md and get the tests passing`

这是由此产生的[拉取请求（PR）](https://github.com/simonw/llm-anthropic/pull/84)。

发布于 [2026年8月24日](/2026/Aug/24/) 下午4:27
</think>

# llm-anthropic 0.27

         **日期：** 2026-08-24 16:27 UTC
         **链接：** https://simonwillison.net/2026/Aug/24/llm-anthropic/
         **标签：** python, httpx, llm, anthropic, claude

         ---

         > *摘要：发布：llm-anthropic 0.27
        本次 LLM 的 Anthropic 插件发布主要提供了对近期发布的 anthropic v1.0.0 Python 库的兼容性支持，该库将底层从 httpx 切换为 *

2026年8月24日

[发布](/elsewhere/release/)
[llm-anthropic 0.27](https://github.com/simonw/llm-anthropic/releases/tag/0.27)
— 通过 LLM 访问 Anthropic 的模型，包括 Claude 系列

本次 LLM 的 Anthropic 插件发布主要提供了对近期发布的 [anthropic v1.0.0](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v1.0.0) Python 库的兼容性支持，该库将底层从 `httpx` 切换为 [httpx2](https://github.com/pydantic/httpx2)。OpenAI 在两周前的 [v3.0.0 版本](https://github.com/openai/openai-python/releases/tag/v3.0.0)中也做出了相同的更改。

Anthropic 提供了这份[迁移指南](https://github.com/anthropics/anthropic-sdk-python/blob/v1.0.0/MIGRATION.md)用于升级到 1.0，因此我在 Claude Code 中向 Fable 5 发出了以下提示：

> `Upgrade to anthropic>=1 - read https://raw.githubusercontent.com/anthropics/anthropic-sdk-python/refs/heads/main/MIGRATION.md and get the tests passing`

这是由此产生的[拉取请求（PR）](https://github.com/simonw/llm-anthropic/pull/84)。

发布于 [2026年8月24日](/2026/Aug/24/) 下午4:27
