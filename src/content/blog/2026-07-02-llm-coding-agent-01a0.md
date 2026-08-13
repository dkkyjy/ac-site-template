---
title: "llm-coding-agent 0.1a0"
description: "llm-coding-agent 0.1a0"
pubDate: "2026-07-02"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/2/llm-coding-agent"
---

> *Feed 摘要：发布：llm-coding-agent 0.1a0
        > 又一个 Fable 5 实验。既然我的 LLM 库已经演变成一个更像代理框架的东西，那么是时候看看基于它构建的简单编码代理会是什么样子了*

2026年7月2日

[发布](/elsewhere/release/)
[llm-coding-agent 0.1a0](https://github.com/simonw/llm-coding-agent/releases/tag/0.1a0)
— 一个基于 LLM 构建的编码代理

又一个 Fable 5 实验。既然我的 [LLM 库](https://llm.datasette.io/) 已经演变成一个更像代理框架的东西，那么是时候看看基于它构建的简单编码代理会是什么样子了。

我使用我的 [python-lib-template-repository](https://github.com/simonw/python-lib-template-repository) GitHub 模板仓库启动了一个[新 Python 库](https://github.com/simonw/llm-coding-agent/tree/2466fa03ba8e5122c3bfa93d52167d33bce40ac6)，然后运行了以下两个提示（这是 [Claude Code 用于网页的转录](https://claude.ai/code/session_01TEUBvBbMipbFSoqjMiJ7ha)）：

> `为这个项目编写一个 spec.md——它将依赖于 PyPI 上最新的“llm”alpha 版本，并实现一个 Claude 代码风格的编码代理，配备用于读取和编辑文件以及执行命令的工具`

然后：

> `提交 spec，然后使用红/绿 TDD 方法在一系列合理的提交中构建它（每个提交都有通过的测试和更新的文档）——偶尔使用你环境中的 OpenAI API 密钥进行手动测试`

这里是 [spec 文件](https://github.com/simonw/llm-coding-agent/blob/0.1a0/spec.md)，[生成的 README 文件](https://github.com/simonw/llm-coding-agent/blob/0.1a0/README.md)，以及[提交序列](https://github.com/simonw/llm-coding-agent/commits/0.1a0)。

我已经将一个 slop-alpha 版本发布到了 PyPI，所以你可以像这样运行新的代理：

```
uvx --prerelease=allow --with llm-coding-agent llm code
```

作为第一次尝试，它相当不错！这里是（由 Fable 撰写的）[README](https://github.com/simonw/llm-coding-agent/blob/0.1a0/README.md)，其中列出了诸如 `llm code --yolo` 和 `llm code --allow "pytest*" --allow "git diff*"` 之类的配方。

它还提供了一个[Python API](https://github.com/simonw/llm-coding-agent/blob/0.1a0/README.md#codingagent)，基于一个 `CodingAgent(model="gpt-5.5", root="/path", approve=True).run("修复 tests/test_parser.py 中失败的测试")` 类，我并没有要求这个，但我很高兴看到它被实现了。

这是[它实现的工具集](https://github.com/simonw/llm-coding-agent/blob/0.1a0/llm_coding_agent/tools.py#L22)，通过 `uvx ... llm tools` 列出：

> `CodingTools_edit_file(path: str, old_string: str, new_string: str, replace_all: bool = False) -> str`
>
> 替换文件中的精确字符串。
>
> old_string 必须与文件内容完全匹配（包括空白），并且除非 replace_all 为真，否则必须标识唯一位置。返回更改的 diff，以便验证。
>
> `CodingTools_execute_command(command: str, timeout: int = 120) -> str`
>
> 在会话根目录中运行 shell 命令。
>
> 返回组合的 stdout 和 stderr，后跟一个退出代码行。timeout 以秒为单位（最大 600）；超时时，整个进程树将被终止。
>
> `CodingTools_list_files(pattern: str = '**/*', path: str = '.') -> str`
>
> 列出匹配 glob 模式的文件，最新的优先。
>
> 跳过隐藏目录、node_modules、\_\_pycache\_\_ 以及（在 git 仓库中）任何被 .gitignore 覆盖的内容。最多返回 200 个相对于搜索目录的路径。
>
> `CodingTools_read_file(path: str, offset: int = 0, limit: int = 2000) -> str`
>
> 读取文本文件，像 cat -n 一样返回带行号的文本。
>
> 路径相对于会话根目录。使用 offset（从 0 开始的首行）和 limit（最大行数）来分页阅读过大的文件。
>
> `CodingTools_search_files(pattern: str, path: str = '.', glob: str = None, max_results: int = 100) -> str`
>
> 搜索文件内容中的正则表达式。
>
> 返回匹配项，格式为 path:line_number:line，上限为 max_results。使用 glob（例如 "*.py"）来限制搜索的文件。
>
> `CodingTools_write_file(path: str, content: str) -> str`
>
> 使用给定内容创建或覆盖文件。
>
> 根据需要创建父目录。对于修改现有文件，更推荐使用 edit_file。

我通过运行 `llm code --yolo` 然后提示来尝试：

> `mkdir /tmp/demo 然后在该文件夹中创建一个简单的 SwiftUI CLI 应用，用于以 ASCII 艺术形式显示时间`

这里是[转录](https://gist.github.com/simonw/750009007050124cd1b390cfe8488e41)，其中 GPT-5.5 的推理笔记指出“SwiftUI 不适合真正的 CLI”，然后构建了一个在 `swift run AsciiTime` 时输出以下内容的应用：

```
      █    █████         ████     █             █     ███   
     ██    █        █        █   ██      █     ██    █   █  
      █    ████           ███     █             █       █   
      █        █    █        █    █      █      █      █    
     ███   ████          ████    ███           ███   █████
```

发布于 [2026年7月2日](/2026/Jul/2/) 晚上7点33分
