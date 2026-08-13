---
title: "sqlite-utils 4.0，现支持数据库模式迁移"
description: "sqlite-utils 4.0，现支持数据库模式迁移"
pubDate: "2026-07-07"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/7/sqlite-utils-4"
---

> *提要：今天早上我发布了 sqlite-utils 4.0，这是该项目的第 124 个版本，也是自 2020 年 11 月 3.0 以来的第一个主版本号提升。除了一些微小但重要的破坏性变更（详见……）*

## sqlite-utils 4.0，现支持数据库模式迁移

2026 年 7 月 7 日

今天早上我发布了 [sqlite-utils 4.0](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0)，这是该项目的第 124 个版本，也是自 2020 年 11 月 [3.0](https://sqlite-utils.datasette.io/en/stable/changelog.html#v3-0) 以来的第一个主版本号提升。除了一些微小但重要的破坏性变更（详见[此升级指南](https://sqlite-utils.datasette.io/en/stable/upgrading.html)），该版本引入了三大主要功能：**数据库迁移**、**嵌套事务**（通过新的 `db.atomic()` 方法），以及对**复合外键**的支持。

#### 使用 sqlite-utils 进行数据库模式迁移

模式迁移定义了对 SQLite 数据库进行的一系列更改，以及一种跟踪哪些迁移已被应用、并应用任何待定迁移的机制。

迁移使用 [sqlite-utils Python 库](https://sqlite-utils.datasette.io/en/stable/python-api.html) 在 Python 文件中定义，该库包含一个强大的 `table.transform()` 方法，提供了 SQLite 的 `ALTER TABLE` 语句所不支持的[增强型表修改能力](https://sqlite-utils.datasette.io/en/stable/python-api.html#transforming-a-table)。

（`table.transform()` 实现了 [SQLite 文档推荐](https://www.sqlite.org/lang_altertable.html#otheralter) 的模式——创建一个具有新模式的临时表，复制数据，然后删除旧表并将临时表重命名替换。）

以下是一个示例迁移文件，它首先创建了一个名为 `creatures` 的表，在第二步中添加了一个额外的列，然后在第三步中更改了其中两列的数据类型：

```
from sqlite_utils import Migrations

migrations = Migrations("creatures")

@migrations()
def create_table(db):
    db["creatures"].create(
        {"id": int, "name": str, "species": str},
        pk="id",
    )

@migrations()
def add_weight(db):
    db["creatures"].add_column("weight", float)

@migrations()
def change_column_types(db):
    db["creatures"].transform(types={"species": int, "weight": str})
```

将其保存为 `migrations.py` 并对一个空数据库运行如下命令：

```
uvx sqlite-utils migrate data.db migrations.py
```

然后检查该数据库的模式：

```
uvx sqlite-utils schema data.db
```

你将看到以下 SQL：

```
CREATE TABLE "_sqlite_migrations" (
   "id" INTEGER PRIMARY KEY,
   "migration_set" TEXT,
   "name" TEXT,
   "applied_at" TEXT
);
CREATE UNIQUE INDEX "idx__sqlite_migrations_migration_set_name"
    ON "_sqlite_migrations" ("migration_set", "name");
CREATE TABLE "creatures" (
   "id" INTEGER PRIMARY KEY,
   "name" TEXT,
   "species" INTEGER,
   "weight" TEXT
);
```

`_sqlite_migrations` 表用于跟踪哪些迁移函数已被运行。上面的 `creatures` 表是所有三个迁移应用后的模式。

要查看待定和已应用的迁移列表，运行：

```
uvx sqlite-utils migrate data.db migrations.py --list
```

输出：

```
Migrations for: creatures

  Applied:
    create_table - 2026-07-07 17:58:41.360051+00:00
    add_weight - 2026-07-07 17:58:41.360608+00:00
    change_column_types - 2026-07-07 18:01:15.802000+00:00

  Pending:
    (none)
```

如果你不指定迁移文件，`sqlite-utils migrate data.db` 命令将扫描当前目录及其子目录中名为 `migrations.py` 的文件，并应用其中找到的任何 `Migrations()` 实例。

你还可以[通过 Python 代码](https://sqlite-utils.datasette.io/en/stable/migrations.html#applying-migrations-in-python)执行迁移，使用 `migrations.apply(db)` 方法，这对于构建管理自身数据库模式（跨越多个版本）的工具非常有用。我自己的 [LLM 工具](https://llm.datasette.io/) 已经使用这种模式好几年了，如 [llm/embeddings\_migrations.py](https://github.com/simonw/llm/blob/0.31/llm/embeddings_migrations.py) 所示。

#### 先例

我最喜欢的这种模式的实现仍然是 [Django 的迁移](https://docs.djangoproject.com/en/6.0/topics/migrations/)，由 Andrew Godwin 基于他早期的项目 [South](https://github.com/andrewgodwin/south) 开发。有趣的事实：在 2008 年的首届 DjangoCon 上，Andrew、Russ Keith-Magee 和我在[模式演进小组讨论](https://www.youtube.com/watch?v=VSq8m00p1FM)中展示了我们各自针对 Django 的模式迁移方法！我的尝试叫做 [dmigrations](https://simonwillison.net/2008/Sep/3/dmigrations/），是与伦敦 Global Radio 的一个团队一起开发的。

Django 的迁移可以从模型定义自动生成，并包含回滚到之前版本的能力。`sqlite-utils` 的方法特意更简单：与 Django 不同，`sqlite-utils` 鼓励程序化的表创建，而非模型定义 ORM，因此我们没有可用来自动生成迁移的东西。

我决定跳过回滚，因为根据我的经验，这是一个很少使用的功能。对于 SQLite 项目，实现回滚的一个简单方法是在应用迁移之前创建数据库文件的副本！

#### 从 sqlite-migrate 迁移

`sqlite-utils` 迁移的设计已有三年历史——我最初将它作为一个独立的包发布，名为 [sqlite-migrate](https://github.com/simonw/sqlite-migrate)，它从未真正超越 beta 版本。

我已经在许多地方使用了该包，因此对设计充满信心。我决定将其提升为 `sqlite-utils` 的一个功能，使其默认可供所有其他不断增长的 sqlite-utils/Datasette/LLM 生态系统工具使用。

我发布了 `sqlite-migrate` 的[最后一个版本](https://github.com/simonw/sqlite-migrate/releases/tag/0.2)，它将依赖关系切换为 `sqlite-utils>=4`，并将 `__init__.py` 文件替换为以下内容：

```
from sqlite_utils import Migrations

__all__ = ["Migrations"]
```

任何依赖 `sqlite-migrate` 的现有项目应继续正常工作，无需修改。

#### sqlite-utils 4.0 中的其他所有内容

以下是该版本的发布说明，附带一些内联注释：

> 4.0 版本包含一些微小的向后不兼容修复（因此主版本号提升），并引入了三大主要新功能：
>
> * [数据库迁移](https://sqlite-utils.datasette.io/en/stable/migrations.html#migrations)，提供了一种结构化机制来随着时间演进项目模式。（[#752](https://github.com/simonw/sqlite-utils/issues/752)）

我认为迁移是标志性的新功能，因此有了这篇博文。

> * [嵌套事务支持](https://sqlite-utils.datasette.io/en/stable/python-api.html#python-api-atomic)，通过 `db.atomic()` 实现，以及对事务在库中工作方式的诸多改进。（[#755](https://github.com/simonw/sqlite-utils/issues/755)）

`sqlite-utils` 长期以来与数据库事务的关系一直很混乱，部分原因是我在 2018 年开始设计该库时，对 SQLite 中事务的工作方式还没有很好的理解。

将迁移添加到核心库让我下定决心最终解决这个问题，因为事务使迁移系统更加安全且易于推理。

我最终围绕一个 `db.atomic()` 上下文管理器构建了这个功能，如下所示：

```
with db.atomic():
    db.table("dogs").insert({"id": 1, "name": "Cleo"}, pk="id")
    db.table("dogs").insert({"id": 2, "name": "Pancakes"})
```

SQLite 支持[保存点](https://sqlite.org/lang_savepoint.html)，因此 `db.atomic()` 可以嵌套，在事务内部执行子事务。这非常棒！

> * 支持[复合外键](https://sqlite-utils.datasette.io/en/stable/python-api.html#python-api-compound-foreign-keys)，包括创建、转换和通过 [table.foreign\_keys](https://sqlite-utils.datasette.io/en/stable/python-api.html#python-api-introspection-foreign-keys) 进行内省。（[#594](https://github.com/simonw/sqlite-utils/issues/594)）

这是因为当我让一个编程代理审查所有开放的 issue 和 PR，看看哪些应该包含在 4.0 版本中（因为它们如果以后添加会构成破坏性变更），它正确识别出复合外键正是那种功能。

我从对 [table.foreign\_keys](https://sqlite-utils.datasette.io/en/stable/python-api.html#python-api-introspection-foreign-keys) 内省方法的一个破坏性变更开始，然后决定看看 Claude Fable 5 是否能处理将复合外键创建集成到库中的更繁琐工作。它帮助创建的 API 设计对我来说[感觉完全正确](https://sqlite-utils.datasette.io/en/stable/python-api.html#compound-foreign-keys)——与库其他部分的工作方式一致。

> 其他值得注意的变更包括：
>
> * Upsert 现在使用 SQLite 的 `INSERT ... ON CONFLICT ... DO UPDATE SET` 语法，自动检测现有表的主键，并拒绝缺少必需主键值的记录。（[#652](https://github.com/simonw/sqlite-utils/issues/652)）

这是促使我考虑进行 4.0 破坏性变更版本提升的第一次变更。我构建它是为了支持 [sqlite-chronicle](https://github.com/simonw/sqlite-chronicle)，后者使用触发器来跟踪表中被插入、更新或删除的行。

> * `db.query()` 现在立即执行，并拒绝不返回行的语句；对于写入和 DDL，请使用 `db.execute()`。

可能是[最具破坏性的变更](https://sqlite-utils.datasette.io/en/stable/upgrading.html#python-api-changes)——我不得不更新自己代码中的几个地方，从 `db.query()` 切换到 `db.execute()`。

> * CSV 和 TSV 导入现在默认检测列类型，而插入到现有表时会保留这些表的列类型。（[#679](https://github.com/simonw/sqlite-utils/issues/679)）

`sqlite-utils insert data.db creatures creatures.csv --detect-types` 标志是后来添加的，允许根据 CSV 中的数据自动检测列类型（文本、整数、实数）。它应该成为默认行为，而发布 4.0 使我能够做到这一点。

> * `table.extract()` 和 `extracts=` 不再为全 `null` 值创建查找表记录。（[#186](https://github.com/simonw/sqlite-utils/issues/186)）

此版本解决的最早的 issue——底层 bug 是在 2020 年 10 月（由我）打开的。

> 有关向后不兼容变更的详细信息，请参阅 [从 3.x 升级到 4.0](https://sqlite-utils.datasette.io/en/stable/upgrading.html#upgrading-3-to-4)。
>
> 在 4.0 预发布周期中发布的详细功能和修复说明，可在 [4.0a0](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0a0)、[4.0a1](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0a1)、[4.0rc1](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0rc1)、[4.0rc2](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0rc2)、[4.0rc3](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0rc3) 和 [4.0rc4](https://sqlite-utils.datasette.io/en/stable/changelog.html#v4-0rc4) 中查看。

升级指南完全由 Claude Fable 5、Claude Opus 4.8 和 GPT-5.5 编写。发布说明也是如此。

这是我逐渐习惯外包给机器人的那种文档。它不需要说服任何人任何事情，也不需要表达任何观点——它的任务是尽可能准确和详细。我已经仔细审查了发布说明，可以确认它们是准确且全面的。

#### Claude Fable 5 提供了很大帮助

我在[一年多前](https://sqlite-utils.datasette.io/en/stable/changelog.html#a0-2025-05-08)发布了 sqlite-utils 4.0 的第一个 alpha 版本。我在稳定发布上拖延了很长时间，因为需要花费大量工作来追踪和清理许多其他小的设计缺陷——主版本号允许我处理这些缺陷。

来自 Claude Fable 5（以及在较小程度上来自 Opus 4.8 和 GPT-5.5）的帮助给了我克服惰性所需的动力，让我充分利用我能投入到这个库上的时间。

Fable 在 API 设计方面*品味非常好*，而且如果你给它一个更开放的目标，它会[非常主动](https://simonwillison.net/2026/Jun/11/fable-is-relentlessly-proactively/)。我最成功的提示是我针对我认为的最后一个候选版本发布的一个审查任务：

> `审查自上次标记的 3.x 版本以来 main 分支上的更改 — 我即将将它们作为 sqlite-utils 4.0 发布，这是一个承诺在很长一段时间内不会再有向后不兼容修复的稳定版本。`
>
> `审查变更日志和升级指南，并为自己编写脚本，尝试 v4 中的所有新功能 — 保存这些脚本，但不要提交它们`

我在 Codex Desktop 中尝试了 GPT-5.5 xhigh 和在 Claude Code 中尝试了 Fable 5。

GPT-5.5 [编写了 5 个 Python 脚本](https://gist.github.com/simonw/823fdecc031371d56dce39537adc0096)，没有发现特别有趣的内容——它的[最终报告在这里](https://github.com/simonw/sqlite-utils/issues/769#issuecomment-4899982463)。

Fable 5 [编写了 12 个脚本](https://gist.github.com/simonw/95800bf584f8e437f1cf0d48d9ef81e6)，在其[报告](https://github.com/simonw/sqlite-utils/issues/769#issuecomment-4900034150)中识别出 4 个发布阻塞问题和 10 个其他问题，并构建了一个整洁的[组合复现脚本](https://gist.githubusercontent.com/simonw/95800bf584f8e437f1cf0d48d9ef81e6/raw/c43918b36a129bba1d2f2a129117aa11c85146c0/12_bug_repros.py)，运行后输出如下：

```
=== 1. 失败的 db.execute() 写入留下了一个隐式打开的事务 ===
  失败写入后 in_transaction: True
  BUG: 当连接关闭时，表 'other' 被静默丢失

=== 2. 开头的 ';' 绕过了 query() 的第一个令牌扫描器 ===
  BUG: 引发 OperationalError: no such savepoint: sqlite_utils_query
  BUG: 尽管回滚，行仍然持久化 (count=1)

=== 3. 通过 query() 被拒绝的写入 PRAGMA 仍然生效 ===
  BUG: 在 '被拒绝' 语句后 user_version=5 (文档说无效)

=== 4. 隐式复合 FK 按表顺序解析主键列，而不是主键顺序 ===
  BUG: other_columns 报告为 ('b', 'a'), 应为 ('a', 'b')
  BUG: 对有效数据的转换引发 IntegrityError: FOREIGN KEY constraint failed

=== 5. ForeignKey (现在是一个数据类) 不再可哈希 ===
  BUG: 无法使用 'sqlite_utils.db.ForeignKey' 作为集合元素 (不可哈希类型: 'ForeignKey')

=== 6. foreign_keys= 中混合的 ForeignKey 对象和元组被拒绝 ===
  BUG: foreign_keys= 应该是一个元组列表

=== 7. insert --csv 到已有表会转换其列类型 ===
  BUG: 已有的 zip '01234' 现在变成了 1234 (列类型: int)

=== 8. insert(pk=, alter=True) 回归: 在 alter 运行之前 InvalidColumns ===
  BUG: InvalidColumns: 对于表 t 的列 ['a'], 无效的主键列 ['id']

=== 9. migrate --stop-before 一个已应用的迁移会应用所有内容 ===
  BUG: 尽管指定了 --stop-before m1 (m1 已应用)，m2 仍然被应用

=== 10. ensure_autocommit_on() 静默提交一个打开的事务 ===
  BUG: 行在回滚后仍然存在 (count=1) - 事务已被提交
```

我发现自己几乎同意所有这些问题。这是[包含 16 次提交的 PR](https://github.com/simonw/sqlite-utils/pull/779)，我们逐一解决了这些问题。

毫无疑问，如果没有最新前沿模型的帮助，sqlite-utils 4.0 的发布质量不会这么高。

发布于 [2026 年 7 月 7 日](/2026/Jul/7/) 晚上 7:32 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或者[订阅我的 newsletter](https://simonwillison.net/about/#subscribe)
