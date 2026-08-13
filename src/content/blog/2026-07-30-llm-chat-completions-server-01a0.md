---
title: "llm-chat-completions-server 0.1a0"
description: "llm-chat-completions-server 0.1a0"
pubDate: "2026-07-30"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/30/llm-chat-completions-server"
---

> *Feed 摘要：发布：llm-chat-completions-server 0.1a0
        LLM 0.32rc1 中新增的内容可寻址日志的一个关键目标是支持 OpenAI Chat Completion 风格的请求，其中每个传入的消*

2026年7月30日

[发布](/elsewhere/release/)
[llm-chat-completions-server 0.1a0](https://github.com/simonw/llm-chat-completions-server/releases/tag/0.1a0)
— 用于提供 OpenAI Chat Completions API 端点的 LLM 插件

LLM 0.32rc1 中新增的内容可寻址日志[在 LLM 0.32rc1 中](https://simonwillison.net/2026/Jul/30/llm-rc1/)的一个关键目标是支持 OpenAI Chat Completion 风格的请求，其中每个传入的消息都会扩展之前的对话，如下所示：

```
curl http://localhost:8002/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -d '{
    "model": "qwen3.5-4b",
    "messages": [
      {"role": "user", "content": "Capital of France?"},
      {"role": "assistant", "content": "Paris."},
      {"role": "user", "content": "Germany?"}
    ]
  }'
```

这里对话状态由客户端跟踪，因此每个请求都会变得越来越长。LLM 中的新架构设计旨在利用各个消息部分的哈希值对这些请求进行去重。

为了测试这一点，我构建了这个插件：

```
uv tool install llm --pre
llm install llm-chat-completions-server
llm chat-completions-server -p 9001
```

运行此命令会启动一个本地主机服务器，端口为 9001，该服务器使用 ChatGPT Completions 兼容的端点暴露您所有已安装的 LLM 模型（来自任何已安装的插件）。

GPT-5.6 Sol [编写了整个内容](https://gist.github.com/simonw/53be513c1bd4a29a7aa480d9bde9b4a5) - 事实证明它非常了解 OpenAI Chat Completions API 的形状。

发布于 [2026年7月30日](/2026/Jul/30/) 下午 3:43
