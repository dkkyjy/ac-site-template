---
title: "sqlite-utils 4.2"
description: "sqlite-utils 4.2"
pubDate: "2026-08-13"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/13/sqlite-utils"
---

> *摘要：发布：sqlite-utils 4.2
        此版本中有大量改进与 table.transform() 功能相关，该功能通过创建新表、复制一个*

2026年8月13日

[发布](/elsewhere/release/)
[sqlite-utils 4.2](https://github.com/simonw/sqlite-utils/releases/tag/4.2)
— 用于操作 SQLite 数据库的 Python CLI 工具和库

此版本中有大量改进与 [table.transform() 功能](https://sqlite-utils.datasette.io/en/stable/python-api.html#transforming-a-table) 相关，该功能通过创建新表、将数据复制过去，然后删除并替换旧表，从而支持复杂的 ALTER TABLE 操作。

`transform()` 现在能保留更多边界情况的模式定义，包括检查约束、唯一约束，甚至描述列的注释。

此外，还有用于检查约束的 [新的自省属性](https://sqlite-utils.datasette.io/en/stable/python-api.html#checks)，以及其他许多较小的更改。

包括来自 [Bunlong Heng](https://github.com/bunlongheng)、[ethanhawkes-gif](https://github.com/ethanhawkes-gif)、[Rami Abdelrazzaq](https://github.com/RamiNoodle733)、[nyxst4ck](https://github.com/nyxst4ck) 和 [ikatyal2110](https://github.com/ikatyal2110) 的贡献。

（后来发现 4.2 有一个 [崩溃 bug](https://github.com/simonw/sqlite-utils/issues/842)，已在 [4.2.1](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-2-1) 中修复。）

发布于 [2026年8月13日](/2026/Aug/13/) 晚上 8:11
