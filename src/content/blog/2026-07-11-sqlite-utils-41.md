---
title: "sqlite-utils 4.1"
description: "sqlite-utils 4.1"
pubDate: "2026-07-11"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/11/sqlite-utils"
---

> *供稿摘要: 发布: sqlite-utils 4.1
        > 自几天前的4.0以来的第一个点版本，引入了许多次要的新功能。

        sqlite-utils insert 和 sqlite-utils upsert 现在接受 --code 选项*

2026年7月11日

[发布](/elsewhere/release/)
[sqlite-utils 4.1](https://github.com/simonw/sqlite-utils/releases/tag/4.1)
——用于操作SQLite数据库的Python CLI工具和库

自[几天前的4.0](https://simonwillison.net/2026/Jul/7/sqlite-utils-4/)以来的第一个点版本，引入了许多次要的新功能。

> * `sqlite-utils insert` 和 `sqlite-utils upsert` 现在接受 `--code` 选项，用于[提供一段Python代码](https://sqlite-utils.datasette.io/en/stable/cli.html#cli-insert-code)（或指向 `.py` 文件的路径），该代码定义一个 `rows()` 函数或 `rows` 可迭代对象以插入行，作为从文件导入的替代方案。([#684](https://github.com/simonw/sqlite-utils/issues/684))

`sqlite-utils` 已经具有允许将Python代码块作为CLI参数传递的功能，例如 `sqlite-utils convert` 命令的[这个功能](https://sqlite-utils.datasette.io/en/stable/cli.html#converting-data-in-columns)：

```
sqlite-utils convert content.db articles headline '
def convert(value):
    return value.upper()'
```

允许代码块[直接生成新行](https://sqlite-utils.datasette.io/en/stable/cli.html#inserting-rows-generated-by-python-code)是该模式的一个明显扩展：

```
sqlite-utils insert data.db creatures --code '
def rows():
    yield {"id": 1, "name": "Cleo"}
    yield {"id": 2, "name": "Suna"}
' --pk id
```

> * `sqlite-utils insert` 和 `sqlite-utils upsert` 现在接受 `--type column-name type` 以[覆盖创建表时自动选择的类型](https://sqlite-utils.datasette.io/en/stable/cli.html#cli-insert-csv-tsv-column-types)。这对于诸如邮政编码之类的CSV或TSV列非常有用，这些列看起来像整数，但应存储为 `TEXT` 以保留前导零。([#131](https://github.com/simonw/sqlite-utils/issues/131))

一个长期存在的功能请求，结果实现[很简单](https://github.com/SAY-5/sqlite-utils/commit/d2ac3765ed9f0516bb0cbc2508a5c3907fb6a71a)。

> * 新增 `table.drop_index(name)` 方法和 `sqlite-utils drop-index` 命令，用于按名称删除索引。两者都支持 `ignore=True`/`--ignore` 以忽略不存在的索引。([#626](https://github.com/simonw/sqlite-utils/issues/626))
> * `sqlite-utils query` 现在可以通过传入 `-` 代替查询语句来从标准输入读取SQL查询，例如 `echo "select * from dogs" | sqlite-utils query dogs.db -`。([#765](https://github.com/simonw/sqlite-utils/issues/765))

另外两个小功能。我让Codex审查了所有未解决的问题并突出了最简单的！

> * `sqlite-utils upsert` 现在可以推断现有表的主键，因此当向已有主键的表进行upsert时，可以省略 `--pk`。

另一个Codex建议，这是从4.0版本中发布的一个Python库改进中缺失的一个明显CLI功能。

> * `table.transform()` 和 `table.transform_sql()` 现在接受 `strict=True` 或 `strict=False` 来更改表的[SQLite严格模式](https://www.sqlite.org/stricttables.html)。省略该选项则保留现有模式。([#787](https://github.com/simonw/sqlite-utils/issues/787))
> * `sqlite-utils transform` 命令现在接受 `--strict` 和 `--no-strict` 来更改表的严格模式。([#787](https://github.com/simonw/sqlite-utils/issues/787))

这两个功能受Evan Hahn的[《在SQLite中首选严格表》](https://evanhahn.com/prefer-strict-tables-in-sqlite/)启发，该文章今天在[Hacker News](https://news.ycombinator.com/item?id=48873940)上引起了讨论。Evan指出：

> 不幸的是，我认为没有办法通过ALTER TABLE使表成为严格表。我认为必须将数据从非严格表复制到严格表中。

这正是[sqlite-utils转换机制](https://sqlite-utils.datasette.io/en/stable/python-api.html#transforming-a-table)所做的，因此我扩展了它，增加了将表从严格模式切换到非严格模式以及反之的能力。

以下是我用来实现这些新严格表功能的[GPT-5.6 Sol xhigh Codex对话记录](https://gist.github.com/simonw/ab8256b81646ad967a601975e206de64)。我运行的最有用的提示之一是：

> `使用 uv run python -c 并手动测试新的 .transform(strict=) 选项，看看是否能发现任何边界情况或错误`

实际上，这告诉模型手动测试其工作，而不是依赖已经编写的自动化测试。这发现了两个小问题，然后我们修复了它们。

发布于[2026年7月11日](/2026/Jul/11/) 晚上11:50
