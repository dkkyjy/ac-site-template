---
title: "Server is now running on http://127.0.0.1:9000/v1"
description: "Server is now running on http://127.0.0.1:9000/v1"
pubDate: "2026-08-04"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/4/new-release-of-llm"
---

```

现在，你可以使用新的 `llm openai endpoint` 命令，通过该服务器运行针对 LLM 的提示！

```
llm openai endpoint http://127.0.0.1:9000/v1 'hello' -m gpt-5.4-mini
```

这类 API 面临的更大挑战与日志记录有关。如果我们要支持在每次请求时追加消息序列的模式，那么理想情况下，我们可以避免在每一轮都记录所有重复的 JSON。

解决方案是新的[内容寻址消息存储](https://llm.datasette.io/en/stable/logging.html#the-message-store)，仿照 Git 构建。你可以在[文档](https://llm.datasette.io/en/stable/logging.html#sql-schema)中查看新的架构，但 `llm logs` 和 `llm logs --json` 命令都已升级，可将该格式转换回易于使用的内容。

#### 其余更新

这个版本中还有更多内容。[0.32 版本说明](https://llm.datasette.io/en/stable/changelog.html#v0-32)相当全面，而 [0.32rc2](https://llm.datasette.io/en/stable/changelog.html#rc2-2026-07-30)、[0.32rc](https://llm.datasette.io/en/stable/changelog.html#rc1-2026-07-30)、[0.32a3](https://llm.datasette.io/en/stable/changelog.html#a3-2026-06-09)、[0.32a2](https://llm.datasette.io/en/stable/changelog.html#a2-2026-05-12) 和 [0.32a0](https://llm.datasette.io/en/stable/changelog.html#a0-2026-04-28) 的说明应该可以填补任何空白。

现有的 LLM 插件应该都能继续工作，但提供额外模型的插件需要升级到 0.32 才能完全参与新的流式事件系统。文档中有一份关于使用[结构化消息和流式事件](https://llm.datasette.io/en/stable/plugins/advanced-model-plugins.html#structured-messages-and-streaming-events)实现插件的指南。

我已经更新了我自己的一些插件：

* [llm-anthropic 0.26](https://github.com/simonw/llm-anthropic/releases/tag/0.26) 增加了对 Claude 5 模型系列的支持，以及 `WebSearch`、`WebFetch`、`CodeExecution` 和 `AnthropicMCP` 服务端工具。
* [llm-gemini](https://github.com/simonw/llm-gemini)、[llm-openrouter](https://github.com/simonw/llm-openrouter) 和 [llm-mistral](https://github.com/simonw/llm-mistral) 已接近完成，即将发布。

#### 我想 LLM 现在是一个智能体框架了

本次发布中不少底层工具更改是由 [Datasette Agent](https://agent.datasette.io/) 的需求驱动的。当我开始开发 LLM 时，“agent”一词的定义还非常模糊，以至于我拒绝使用它。在 [2025 年 9 月](https://simonwillison.net/2025/Sep/18/agents/)，我开始认同这样的观点：“**LLM 智能体通过循环运行工具来实现目标**” 这一说法现在已经足够成熟，我可以不再完全回避这个词了。

工具链现在可以[暂停等待人工批准](https://llm.datasette.io/en/stable/python-api.html#python-api-tools-pause)，也可以[从存储的消息历史中恢复](https://llm.datasette.io/en/stable/python-api.html#python-api-tools-resume)——这两者都是 Datasette Agent 所需要的。

如今看 LLM，它已经开始变得很有智能体的样子了。能有一个 CLI 实用程序，把来自不同来源的不同工具与不同模型以一行命令混搭在一起，还带有一个足够强大的 Python 库来构建像 [Datasette Agent](https://agent.datasette.io/) 和 [llm-coding-agent](https://github.com/simonw/llm-coding-agent) 这样的系统，这实在很妙。

也许下一版 LLM 会把“智能体”的概念融入核心库。我仍在尝试弄清楚那会是什么样子。

发布于 [2026年8月4日](/2026/Aug/4/) 晚上 11:58 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的新闻通讯](https://simonwillison.net/about/#subscribe)
