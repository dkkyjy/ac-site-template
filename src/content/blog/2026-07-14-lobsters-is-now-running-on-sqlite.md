---
title: "lobste.rs 现在运行在 SQLite 上"
description: "lobste.rs 现在运行在 SQLite 上"
pubDate: "2026-07-14"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/14/lobsters-sqlite"
---

> *摘要：lobste.rs 现在运行在 SQLite 上
社区网站 Lobsters 自 2018 年 8 月起计划从 MariaDB 迁移——最初目标是 PostgreSQL，但去年他们决定研究 SQLite。*

2026 年 7 月 14 日 - 链接博客

**[lobste.rs 现在运行在 SQLite 上](https://lobste.rs/s/ko1ji1/lobste_rs_is_now_running_on_sqlite)**。社区网站 [Lobsters](https://lobste.rs) 自 [2018 年 8 月起](https://github.com/lobsters/lobsters/issues/539#issuecomment-4959857588) 一直在计划从 MariaDB 迁移——最初目标是 PostgreSQL，但去年他们决定转而 [研究 SQLite](https://github.com/lobsters/lobsters/issues/539#issuecomment-2964114295)。

本周末他们完成了迁移，并且现在认为它足够稳定，这看起来将是该网站未来的永久架构：

> SQLite 似乎以优异成绩通过了测试：CPU 使用率下降，内存使用率下降，至少对我来说网站似乎更快了，一旦 MariaDB VPS 停用，VPS 成本降低一半

Lobsters 的 Rails 应用程序现在运行在单个 VPS 上，主内容 SQLite 数据库文件大小约为 3.8GB。此外，[还有](https://lobste.rs/s/ko1ji1/lobste_rs_is_now_running_on_sqlite#c_c9ydhs) 一个 1.1GB 的缓存数据库、一个 218MB 的队列数据库，以及一个仍在增长的 555MB 的 rack_attack 数据库，由 [Rack::Attack](https://github.com/rack/rack-attack) 中间件用于阻止和限制恶意请求。

在链接的讨论帖和 Thomas Dziedzic 的 [SQLite 迁移 PR](https://github.com/lobsters/lobsters/pull/1927) 中还有更多细节，该 PR 在 30 次提交和 188 个文件中添加了 735 行代码并删除了 593 行代码。该 PR 建立在之前的 PR [#1705](https://github.com/lobsters/lobsters/pull/1705)、[#1871](https://github.com/lobsters/lobsters/pull/1871) 和 [#1924](https://github.com/lobsters/lobsters/pull/1924) 之上。

这是一个非常有用的案例研究，也提醒我们，在 2026 年，使用单台服务器和 SQLite 可以实现很多事情。

发布于 [2026 年 7 月 14 日](/2026/Jul/14/) 晚上 7:44
