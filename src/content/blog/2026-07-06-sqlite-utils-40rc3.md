---
title: "sqlite-utils 4.0rc3"
description: "sqlite-utils 4.0rc3"
pubDate: "2026-07-06"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/6/sqlite-utils"
---

> *摘要：发布：sqlite-utils 4.0rc3
        我原本希望本周末发布稳定的 sqlite-utils 4.0，但在使用 Claude Fable 5 和 GPT-5.5 组合处理积压的问题和拉取请求时，*

2026年7月6日

[发布](/elsewhere/release/)
[sqlite-utils 4.0rc3](https://github.com/simonw/sqlite-utils/releases/tag/4.0rc3)
——用于操作 SQLite 数据库的 Python CLI 工具和库

我原本希望本周末发布稳定的 `sqlite-utils 4.0`，但在使用 Claude Fable 5 和 GPT-5.5 组合处理积压的问题和拉取请求时，自 rc2 以来的更新日志[不断变长](https://sqlite-utils.datasette.io/en/latest/changelog.html#rc3-2026-07-05)。

最大的新功能是支持内省和创建复合外键——该功能涉及对 [table.foreign_keys](https://sqlite-utils.datasette.io/en/latest/python-api.html#foreign-keys) 的细微破坏性变更，因此需要在 4.0 稳定版中落地。

`sqlite-utils` 现在也遵循 SQLite 对不区分大小写列名的约定，这最终[同时影响了多个地方](https://sqlite-utils.datasette.io/en/latest/changelog.html#case-insensitive-column-matching)。

发表于 [2026年7月6日](/2026/Jul/6/) 凌晨5:40
