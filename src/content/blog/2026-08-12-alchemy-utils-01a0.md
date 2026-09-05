---
title: "alchemy-utils 0.1a0"
description: "alchemy-utils 0.1a0"
pubDate: "2026-08-12"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/12/alchemy-utils"
---

发布：alchemy-utils 0.1a0 我长期以来一直在思考，我的 sqlite-utils Python 库和命令行工具的一个数据库无关版本会是什么样子。今天早上（字面意义上就是一个洗澡时的灵感项目），我让 Codex 和 GPT-5.6 Sol Ultra 构建了一个原型：做一个技术预研，看看构建一个拥有与 SQLite-utils 相同核心 API 的库需要付出什么——特别是 insert 和 upsert 以及 insert_all 和 upsert_all 和 create 和 update 方法，以及表结构内省功能——但由 SQLalchemy 驱动，使其适用于多种数据库引擎。针对 PostgreSQL、SQLite 和 duckdb 进行测试。使用 ~/dev/sqlite-utils 作为参考。为此创建一个 git 仓库，尽早且频繁地提交——使用 uv init 来启动项目——采用红/绿 TDD 和 pytest，可以参考 ~/dev/django-sql-dashboard 来了解 PostgreSQL 测试的一种实现思路。只用了很少的后续提示，就产出了一个足以作为 alpha 版本发布的项目。这是我用来列出本地 PostgreSQL 博客数据库副本中某张表行的一行命令：uvx --with 'alchemy-utils[postgresql]' alchemy-utils rows 'postgresql+psycopg://simon@localhost:5432/simonwillisonblog' redirects_redirect 输出结果大致如下：[ { "id": 2328, "domain": "simonwillison.net", "path": "2020/May/21/apple-photos-sqlite/", "target": "/2020/May/21/dogsheep-photos/", "created": "2020-05-21T13:03:46.591692-07:00" }, { "id": 3, "domain": "feeds.simonwillison.net", "path": "swn-links", "target": "https://simonwillison.net/atom/links/", "created": "2017-10-01T14:12:54.820729-07:00" } 或者，如果你想要一个包含旧金山每棵树的 DuckDB 数据库，schema 自动创建以匹配文件：curl 'https://raw.githubusercontent.com/simonw/sf-tree-history/refs/heads/main/Street_Tree_List.csv' | uvx --with 'alchemy-utils[duckdb]' alchemy-utils insert 'duckdb:////tmp/trees.db' trees - --csv （第一次运行这个命令花了将近一个小时，所以我让 Codex 优化了一下，缩短到了大约 35 秒。）标签：databases , postgresql , projects , python , sql , sqlalchemy , sqlite , sqlite-utils ,

*（已截断，请参见原文）*
