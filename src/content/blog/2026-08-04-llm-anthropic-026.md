---
title: "llm-anthropic 0.26"
description: "llm-anthropic 0.26"
pubDate: "2026-08-04"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/4/llm-anthropic"
---

> *订阅摘要：发布：llm-anthropic 0.26
        包含 LLM 0.32 启用的新功能：


新模型：claude-fable-5、claude-sonnet-5 和 claude-opus-5。#75、#76
添加了 WebSearch、WebFetc 的服务端工具*

2026年8月4日

[发布](/elsewhere/release/)
[llm-anthropic 0.26](https://github.com/simonw/llm-anthropic/releases/tag/0.26)
— 通过 LLM 访问 Anthropic 的模型，包括 Claude 系列

包含 [LLM 0.32](https://simonwillison.net/2026/Aug/4/new-release-of-llm/) 启用的新功能：

> * 新模型：`claude-fable-5`、`claude-sonnet-5` 和 `claude-opus-5`。[#75](https://github.com/simonw/llm-anthropic/issues/75)、[#76](https://github.com/simonw/llm-anthropic/issues/76)
> * 添加了用于 `WebSearch`、`WebFetch`、`CodeExecution` 和 `AnthropicMCP` 的服务端工具，可通过 LLM 的 `-T` 接口或 Python `tools=` 使用。之前的 `-o web_search*` 选项已被移除，取而代之的是 `-T WebSearch`。[#79](https://github.com/simonw/llm-anthropic/issues/79)
> * 升级到 [llm>=0.32](https://llm.datasette.io/en/stable/changelog.html#v0-32)。推理、工具调用、工具结果和服务端工具结果现在以类型化事件流式传输。`llm` CLI 提示中的推理现在会输出到标准错误，除非您传入 `--hide-reasoning/-R`。
> * 将扩展思考简化为 `thinking` 和 `thinking_effort`（`low`、`medium`、`high`、`xhigh` 或 `max`）。Claude 5 模型默认进行思考；`-o thinking 0` 会禁用 Sonnet 5 和 Opus 5 的思考，而 Fable 5 始终进行思考。`-R/--hide-reasoning` 现在会从响应和日志中省略推理。`thinking_budget`、`thinking_display` 和 `thinking_adaptive` 选项已被移除。[#80](https://github.com/simonw/llm-anthropic/issues/80)

发布于 [2026年8月4日](/2026/Aug/4/) 晚上10点
