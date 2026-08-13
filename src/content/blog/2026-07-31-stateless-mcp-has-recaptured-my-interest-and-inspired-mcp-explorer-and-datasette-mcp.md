---
title: "无状态 MCP 重新引起了我的兴趣（并启发了 mcp-explorer 和 datasette-mcp）"
description: "无状态 MCP 重新引起了我的兴趣（并启发了 mcp-explorer 和 datasette-mcp）"
pubDate: "2026-07-31"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/31/stateless-mcp"
---

> *Feed 摘要：周二是无状态 MCP 日——MCP 2.0 的发布，或者按照更正式但更难记的名称，是 2026-07-28 的模型上下文协议规范。这是对 MCP 规范最重大的变化。*

## 无状态 MCP 重新引起了我的兴趣（并启发了 mcp-explorer 和 datasette-mcp）

2026 年 7 月 31 日

周二是[无状态 MCP 日](https://x.com/ade_oshineye/status/2082129440943866149)——MCP 2.0 的推出，或者按照更正式但更难记的名称，是 [2026-07-28 的模型上下文协议规范](https://blog.modelcontextprotocol.io/posts/2026-07-28/)。这是 MCP 规范自首次发布以来最重要的一次变化，也重新点燃了我个人对该协议的兴趣。

背景介绍：MCP 即模型上下文协议（Model Context Protocol），它描述了一种将新工具暴露给由 LLM 驱动的智能体框架的标准方式。它由 Anthropic 早在 [2024 年 11 月](https://www.anthropic.com/news/model-context-protocol) 推出，在 2025 年的大部分时间里引发了 *巨大* 的关注热潮，之后逐渐被 [Skills](https://simonwillison.net/2025/Oct/16/claude-skills/)（Anthropic 的另一项发明）盖过风头，因为人们发现，一个能访问终端和 `curl` 的智能体框架，可以以更灵活的方式完成 MCP 能做的大部分事情。我在 [2025 年回顾](https://simonwillison.net/2025/Dec/31/the-year-in-llms/#the-only-year-of-mcp) 中写过这一点。

我现在又回到了 MCP。给智能体一个能够访问互联网的 shell 环境是[充满风险的](https://simonwillison.net/2026/Jul/22/openai-cyberattack/)，并且需要一个足够强大的模型来有效驾驭这样的环境。MCP 工具更容易审计和控制，而且足够简单，即使在笔记本电脑上运行的较小模型也能较好地使用它们。

新的无状态 MCP 规范还大大降低了为该协议实现客户端和服务器的复杂度。这周我构建了三个这样的实现！

#### 无状态 MCP 让哪些事情变得更简单

有状态与无状态 MCP 之间的差异，在这篇介绍了新规范 RC 版本的 [5 月 21 日博客文章](https://blog.modelcontextprotocol.io/posts/2026-07-28-release-candidate/)中得到了最好的展示。其中包含一个清晰的前后对比示例。

旧的有状态 MCP（我打算称之为“legacy MCP”）需要两个 HTTP 请求——第一个用于初始化会话并获得 `Mcp-Session-Id`，第二个用于真正调用工具：

```
POST /mcp HTTP/1.1
Content-Type: application/json

{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "initialize",
  "params": {
    "protocolVersion": "2025-11-25",
    "capabilities": {
    },
    "clientInfo": {
      "name": "my-app",
      "version": "1.0"
    }
  }
}

POST /mcp HTTP/1.1
Mcp-Session-Id: 1868a90c-3a3f-4f5b
Content-Type: application/json

{
  "jsonrpc": "2.0",
  "id": 2,
  "method": "tools/call",
  "params": {
    "name": "search",
    "arguments": {
      "q": "otters"
    }
  }
}
```

新的无状态方式使用单个 HTTP 请求，如下所示：

```
POST /mcp HTTP/1.1
MCP-Protocol-Version: 2026-07-28
Mcp-Method: tools/call
Mcp-Name: search
Content-Type: application/json

{
  "jsonrpc": "2.0",
  "id": 1,
  "method": "tools/call",
  "params": {
    "name": "search",
    "arguments": {
      "q": "otters"
    },
    "_meta": {
      "io.modelcontextprotocol/clientInfo": {
        "name": "my-app",
        "version": "1.0"
      }
    }
  }
}
```

无论从客户端还是服务端的实现角度来看，这都要干净得多。它也更适合构建可扩展的 Web 应用程序，因为现在你无需维护服务端状态来跟踪这些会话 ID，也无需担心将同一会话路由到同一后端机器。

#### mcp-explorer

我没能找到一款可以交互式探查 MCP 服务器的优秀 CLI 工具，所以我让 Codex 帮我构建了一个。

**[mcp-explorer](https://github.com/simonw/mcp-explorer)** 就是结果。它是一个无状态的 Python CLI 工具，所以你甚至不需要安装它就能试用——它可以配合 [uvx](https://docs.astral.sh/uv/guides/tools/#running-tools) 使用，如下所示：

```
uvx mcp-explorer list https://agentic-mermaid.dev/mcp
```

这会查询 Ade Oshineye 的 [agentic-mermaid.dev](https://agentic-mermaid.dev/) 演示 MCP。上面的命令返回以下工具列表：

```
execute(code: string, timeoutMs?: integer) - Execute Mermaid SDK code
  Run JavaScript in an isolated sandbox; return a value.

describe_sdk(family: string, detail?: string) - Describe Mermaid SDK operations
  Return version-matched mutation operations for one diagram family.

render_svg(source: string, options?: object) - Render Mermaid as SVG
  Render a Mermaid source string to themeable SVG. Returns { ok, svg }.

render_ascii(source: string, useAscii?: boolean, targetWidth?: integer, options?: object) - Render Mermaid as text
  Render a Mermaid source string to text. Returns { ok, text }.

render_png(source: string, scale?: number, background?: string, fitTo?: object, options?: object) - Render Mermaid as PNG
  Rasterize a Mermaid source string to PNG. Returns { ok, png_base64 }.
...
```

接下来，要检查某个工具：

```
uvx mcp-explorer inspect render_svg
```

这会输出一大堆信息，包括输入和输出的 JSON Schema。

要调用该工具并向它传递参数：

```
uvx mcp-explorer call \
  https://agentic-mermaid.dev/mcp \
  render_svg \
  -a source 'graph TD; A-->B' \
  -a options '{"padding":24}'
```

它返回：

```
{"ok":true,"svg":"<svg xmlns=\"http://www.w3.org/2000/svg\" width=...
```

如果只想获取原始 SVG，试着在该命令后加上 `| jq .svg -r`。我得到了[这张图片](https://gist.github.com/simonw/b07c62f0ce103be6932477659d5dd1ac)：

README 里还有[几个命令](https://github.com/simonw/mcp-explorer/blob/main/README.md)，不过你应该已经了解大致思路了。我发现，即使大部分实际代码是由智能体编写的，像这样构建 CLI 工具也是熟悉规范的一种非常高效的方式。

#### datasette-mcp

第二个项目是 **[datasette-mcp](https://github.com/datasette/datasette-mcp)**，它是一个 Datasette 插件，为任何 Datasette 实例添加 `/-/mcp` 端点。

这大概是我第四次尝试构建这个插件，但多亏了新的无状态 MCP 规范，我终于有了一个觉得可以发布的版本。

它只提供三个工具：`list_databases()`、`get_database_schema(database_name)` 和 `execute_sql(database_name, sql)`。它们的功能正如你所预期的那样——不过目前 `execute_sql()` 是只读的。

把这些工具接入智能体，或接入 ChatGPT、Claude 这类聊天工具后，它们就能针对你托管的 Datasette 实例运行 SQL 查询。

到目前为止，我把它运行在我的博客的 Datasette 镜像上，地址是 <datasette.simonwillison.net/-/mcp>。我费了一番折腾才搞明白如何把它接入 ChatGPT 和 Claude，不过最后还是成功了。这里有[一篇新的 TIL](https://til.simonwillison.net/llms/mcp-in-claude-and-chatgpt) 精确展示了具体做法。

这里有一个[共享的 Claude 会话](https://claude.ai/share/de1ad9bf-f7c2-4fb9-a9a0-2a1ae39995db)，我在其中问它：

> `list tables in simonwillison.net`

然后：

> `what has Simon said recently about MCP?`

它运行了 7 个独立的 SQL 查询来得出结论。

#### llm-mcp-client

我的 [LLM 工具](https://llm.datasette.io/) 早该有官方 MCP 集成了。新的 alpha 版 [llm-mcp-client](https://github.com/simonw/llm-mcp-client) 插件正是我为此做出的尝试：

```
llm install llm-mcp-client
llm -T 'MCP("https://datasette.simonwillison.net/-/mcp")' 'count the notes'
```

以下是输出（包含推理轨迹；我正在使用 [LLM 0.32rc2](https://simonwillison.net/2026/Jul/30/llm-rc2/)）：

> ***考虑笔记数量***
>
> *我看到“count the notes”这个问题可能是在要求我统计博客笔记的总数。也可能指已发布的笔记或草稿，所以这里有些歧义。我需要弄清笔记总数，可能要通过同时查询已发布笔记和草稿的数量来得到明确答案。让我执行这个计数吧！*
>
> 共有 **151 条笔记**。

还有该提示词对应的 [llm logs 输出](https://gist.github.com/simonw/4e8f558766150658ce35eab4f0fc3e04)。

一旦它完全成熟，我正在考虑直接把它并入 LLM 核心。我也很期待在 [Datasette Agent](https://agent.datasette.io/) 和 [llm-coding-agent](https://github.com/simonw/llm-coding-agent) 里试试 MCP。

#### MCP 是使用智能体构建应用的更安全方式

MCP 首次发布几个月后，我写了[《Model Context Protocol 存在提示注入安全问题》](https://simonwillison.net/2025/Apr/9/mcp-prompt-injection/)，在文中我指出，让终端用户自行混搭工具的模式，把防范数据外泄攻击的责任推给了用户自己。当时我还没有提出[“致命三重奏”（the Lethal Trifecta）](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)这个说法，但那绝对就是我心里所想的。

后来，拥有任意 shell 和 `curl` 访问权限的通用智能体出现了，而要让它们保持安全就难得多了！

我逐渐欣赏 MCP 的一点是，与在开放网络环境中任意执行命令相比——那是当今大多数通用智能体和编码智能体工具的默认方式——它能让你更轻松地推演智能体的能力范围以及可能出错的地方。

在基于 LLM 构建敏感应用时，我打算更加倚重 MCP。

发布于 [2026 年 7 月 31 日](/2026/Jul/31/) 晚上 11:13 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
