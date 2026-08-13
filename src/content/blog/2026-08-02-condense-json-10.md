---
title: "condense-json 1.0"
description: "condense-json 1.0"
pubDate: "2026-08-02"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/2/condense-json"
---

> *Feed 摘要：发布：condense-json 1.0
> 我正试着在发布 1.0 版本时更大胆一些。这个小型库已经有一年半的历史了——我已经应用了一些合理且无破坏性的修复，并 shippe*

2026年8月2日

[发布](/elsewhere/release/)
[condense-json 1.0](https://github.com/simonw/condense-json/releases/tag/1.0)
— 使用替换字符串来压缩 JSON 的 Python 函数

我正试着在发布 1.0 版本时更大胆一些。这个小型库已经有一年半的历史了——我应用了一些合理且无破坏性的修复，并为它发布了 1.0 这个大版本。

下面是一个它能做什么的示例，摘自 README：

```
{
  "foo": {
    "bar": {
      "string": "This is a string with foxes in it",
      "nested": {
        "more": ["Here is a string", "another with foxes in it too"]
      }
    }
  }
}
```

将其与一个 replacements 对象结合：

```
{"1": "with foxes in it"}
```

然后 `condense_json(input_json, replacements)` 会生成以下结果：

```
{
  "foo": {
    "bar": {
      "string": {"$r": ["This is a string ", {"$": "1"}]},
      "nested": {
        "more": ["Here is a string", {"$r": ["another ", {"$": "1"}, " too"]}]
      }
    }
  }
}
```

它会扫描该 replacements 对象中存在的字符串或子字符串，并在输出中用特殊的 `{"$r": ...}` 语法替换它们。

你可以通过 `uncondense_json(condensed, replacements)` 逆转这一效果。

其目的是让存储包含来自其他相关结构的重复数据的 JSON 变得更加容易。我用它来节省由 [LLM](https://llm.datasette.io/) 生成的 SQLite 日志中的空间——请参阅 [PR #1586](https://github.com/simonw/llm/pull/1586) 了解该方案的最新迭代。

发布于 [2026年8月2日](/2026/Aug/2/) 晚上 10:19
