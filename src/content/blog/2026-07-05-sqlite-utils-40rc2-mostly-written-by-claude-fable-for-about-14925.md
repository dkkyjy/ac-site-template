---
title: "sqlite-utils 4.0rc2，主要由Claude Fable编写（花费约149.25美元）"
description: "sqlite-utils 4.0rc2，主要由Claude Fable编写（花费约149.25美元）"
pubDate: "2026-07-05"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/5/sqlite-utils-fable"
---

> *Feed摘要：几周前我写了关于sqlite-utils 4.0rc1发布的文章。由于我们的Max订阅中Claude Fable仅剩几天可用，我决定看看它是否能帮助我完成4.0的稳定版发布。*

## sqlite-utils 4.0rc2，主要由Claude Fable编写（花费约149.25美元）

2026年7月5日

几周前我写了关于[sqlite-utils 4.0rc1](https://simonwillison.net/2026/Jun/21/sqlite-utils-40rc1/)发布的文章。由于我们的Max订阅中Claude Fable仅剩几天可用，我决定看看它是否能帮助我完成一个我真正放心的4.0稳定版发布——因为我尽量遵循[SemVer](https://semver.org)，并且希望不兼容的主版本尽可能少。

我在iPhone上通过Claude Code for web 开始了以下提示：

> `在发布稳定版4.0之前的最终审查——非常重要的是要发现任何如果以后修复会变成破坏性变更的问题`

这是它为我创建的[那份初始报告](https://github.com/simonw/sqlite-utils/blob/0c369a447eeaf39084f0d14a45b3eeb7eacb631b/fable-review-4.0rc1.md)。有一些我自己尚未遇到的**严重**问题——Fable将其中的5个归类为“发布阻塞问题”。这是最糟糕的一个：

> **1. `delete_where()`永不提交并毒化连接（数据丢失）**
>
> `Table.delete_where()` (`sqlite_utils/db.py:2948`) 通过裸调用 `self.db.execute()` 来执行DELETE，没有使用 `atomic()` 包装——对比 `Table.delete()` 在 `db.py:2944`，后者正确包装了。连接保持 `in_transaction=True`，因此每次后续的 `atomic()` 调用都会走保存点分支（`db.py:430-440`），也永远不会提交。
>
> 端到端复现：
>
> ```
> db = sqlite_utils.Database("dw.db")
> db["t"].insert_all([{"id": i} for i in range(3)], pk="id")
> db["t"].delete_where("id = ?", [0])   # conn.in_transaction 现在为 True
> db["t"].insert({"id": 50})
> db["u"].insert({"a": 1})
> db.close()
> # 重新打开：行是 [0, 1, 2] —— 删除、行50 以及表 u 全部丢失。
> ```

这是一个非常糟糕的 bug！我很庆幸没有发布这个版本，尽管至少这算是一个可以在 4.0.1 补丁发布中修复的 bug，而不是一个会迫使 5.0 的设计缺陷。

经过 37 个提示、34 次提交以及 30 个独立文件中 +1,321 -190 的代码更改，我们逐一处理了所有反馈，并沿途做了若干其他设计改进。

编码代理的一个奇怪之处在于，像这样较难的任务实际上为你提供了更多同时做其他事情的机会，因为代理有时需要 10-15 分钟来处理新任务。我出去享受了半月湾的国庆日游行，偶尔查看手机并提示 Fable 下一步操作。

详细信息见[PR](https://github.com/simonw/sqlite-utils/pull/767)和[这份共享对话记录](https://claude.ai/code/session_01UnLnhsH25Nnv7LHhekUfPd)。最终审查我切换到了笔记本电脑，并通过 GitHub 的 PR 界面进行。

最重要的变更与事务处理有关，这是早期 RC 版本的标志性新功能。新的 RC 现在包含关于新事务模型的[全面文档](https://sqlite-utils.datasette.io/en/latest/python-api.html#transactions-and-saving-your-changes)，其引言部分我在此完整引用：

> 本库中每个写入数据库的方法——`insert()`、`upsert()`、`update()`、`delete()`、`delete_where()`、`transform()`、`create_table()`、`create_index()`、`enable_fts()` 等——都在自己的事务内运行，并在返回前提交。方法调用完成后，您的更改将保存到磁盘：
>
> ```
> db = Database("data.db")
> db.table("news").insert({"headline": "Dog wins award"})
> # 新行已经保存——无需 commit()
> ```
>
> 同样的规则适用于使用 [db.execute()](https://sqlite-utils.datasette.io/en/latest/python-api.html#python-api-transactions-execute) 执行的原始 SQL——写入语句在运行后立即提交。
>
> 您永远不需要调用 `commit()`，也不需要关闭数据库来持久化更改。只有两种情况需要您考虑事务：
>
> 1. 您希望将多个写入操作分组，以便它们全部成功或全部失败——使用 [db.atomic()](https://sqlite-utils.datasette.io/en/latest/python-api.html#python-api-atomic)。
> 2. 您使用 `db.begin()` [自行管理事务](https://sqlite-utils.datasette.io/en/latest/python-api.html#python-api-transactions-manual)，在这种情况下，直到您提交之前没有任何内容被提交——该库永远不会提交您打开的事务。

在审查 Fable 的文档时——我发现先审查文档编辑是建立对变更初步理解的绝佳方式——我注意到了[这个细节](https://github.com/simonw/sqlite-utils/blob/6c88067ab76b9597fb1c538c53164632526a2891/docs/python-api.rst?plain=1#L386)：

> `db.atomic()` 和自动的每个方法事务是为 Python 默认事务处理模式下的连接设计的。使用 Python 3.12+ `sqlite3.connect(..., autocommit=True)` 或 `autocommit=False` 选项创建的连接不受支持，因为 `commit()` 和 `rollback()` 在这些连接上的行为不同。

我承认我之前没有考虑过 `sqlite-utils` 对更新的 [autocommit 设置](https://docs.python.org/3/library/sqlite3.html#sqlite3.Connection.autocommit) 会如何反应（该设置添加于 Python 3.12）。结果“这些连接上的行为不同”意味着几乎整个测试套件都会失败，于是我与该模型合作[确保这种差异](https://github.com/simonw/sqlite-utils/commit/f7ff3e2027aefb9905ebb2e611e5bbb0a62382c5)不会破坏库的工作方式。

#### 以及 GPT-5.5 的最终审查

我曾经认为让一个模型审查另一个模型的工作有些荒谬——感觉像是迷信。问题是**这确实有效**——我已经习惯性地让 Anthropic 最好的模型审查 OpenAI 的工作，反之亦然，因为我经常发现这能带来有趣的结果。

我使用以下提示启动了 Codex Desktop 和 GPT-5.5 xhigh：

> `审查自上一个 RC 以来的变更。同时确认变更日志是最新的。`

这足以发现两个值得调查的问题：

> **发现**
>
> * [P1] [sqlite\_utils/db.py:663](https://github.com/simonw/sqlite-utils/blob/04f8971546418962aaf6579d4028c7117d6c3a20/sqlite_utils/db.py#L663) `db.query()` 现在仅在调用 `db.execute()` 后才拒绝非行语句，而 [sqlite\_utils/db.py:705](https://github.com/simonw/sqlite-utils/blob/04f8971546418962aaf6579d4028c7117d6c3a20/sqlite_utils/db.py#L705) 会先自动提交这些写入。因此 `db.query("update ...")` 会引发 `ValueError`，但更新已经提交。对于文档中描述为“只能用于返回行的 SQL”的方法来说，这是一个令人惊讶的副作用。
> * [P1] [sqlite\_utils/db.py:672](https://github.com/simonw/sqlite-utils/blob/04f8971546418962aaf6579d4028c7117d6c3a20/sqlite_utils/db.py#L672) 通过 `db.query()` 执行的 `INSERT ... RETURNING` 仅在返回的生成器完全耗尽后才提交。使用 `db.query("insert ... returning ...")` 而不迭代，或者常见的 `next(db.query(...))` 用法，会使事务保持打开状态，写入可能在关闭时回滚。这与 [docs/changelog.rst:15](https://github.com/simonw/sqlite-utils/blob/04f8971546418962aaf6579d4028c7117d6c3a20/docs/changelog.rst?plain=1#L15) 和 [docs/python-api.rst:232](https://github.com/simonw/sqlite-utils/blob/04f8971546418962aaf6579d4028c7117d6c3a20/docs/python-api.rst?plain=1#L232) 相矛盾，后者表示无需迭代即可生效。

我将这些粘贴到一个新的 Fable 会话中，该会话进行了一些实验来确认问题：

> 两个发现均已确认。`db.query()` 首先调用 `self.execute()`，该调用会自动提交写入，然后才检查 `cursor.description`——因此 `db.query("update ...")` 在引发 `ValueError` 之前就已经提交了更新。而 `INSERT ... RETURNING` 的提交位于返回的生成器末尾，因此除非迭代器被耗尽，否则它不会触发——`next(db.query(...))` 或未迭代的调用会使事务保持打开状态，这与变更日志和文档的承诺相悖。

这是包含修复的 [PR](https://github.com/simonw/sqlite-utils/pull/768) 以及完整的 [Claude Code 对话记录](https://claude.ai/code/session_012U3iRfJoTZ5vd22cBSF2nJ)。审查这段代码帮助我建立了对 SQLite 事务语义边缘情况的更好心智模型！

#### 预估（未补贴）成本为 149.25 美元

我将 Claude Max 订阅升级到了 200 美元/月（之前是 100 美元/月），以增加我在剩余时间内（直到 7 月 7 日的 Fable 末日）的 Fable 配额，届时即使 Claude Max 订阅者也需要为该模型支付完整的 API 成本。

我很好奇如果直接支付这些费用会花多少钱。起初我以为这些数字无法获取，因为我使用 Claude Code for web 远程完成了工作，然后我意识到我可以运行 [AgentsView](https://www.agentsview.io) 来分析现有会话，从而获得成本估算！

> `运行 "uvx agentsview --help"​ 然后使用该工具计算此会话的成本`

Claude 学会了如何使用 `session list --include-children` 命令，并得出以下结果：

| 对话记录 | 模型 | 成本 |
| --- | --- | --- |
| 主会话 | claude-fable-5 | $141.02 |
| API 表面扫描代理 | claude-fable-5 | $2.40 |
| 事务/原子性审查代理 | claude-fable-5 | $2.39 |
| rc1 后提交审查代理 | claude-fable-5 | $1.72 |
| 迁移审查代理 | claude-fable-5 | $1.40 |
| 提示计数代理 | claude-opus-4-8 | $0.32 |
| **总计** | **$149.25** | |

我很高兴我订阅了！我真的应该[听从自己的建议](https://simonwillison.net/2026/Jul/3/judgement/)，更多地依赖更便宜模型的子代理。

这是 [claude.ai/settings/usage](https://claude.ai/settings/usage) 目前显示的内容：

我现在还在进行其他几个由 Fable 驱动的重要项目，目标是在 Fable 价格上调前，正好用满那个 Fable 进度条。

#### sqlite-utils 4.0rc2 的完整发布说明

以下是该 RC 版本的[完整发布说明](https://sqlite-utils.datasette.io/en/latest/changelog.html#rc2-2026-07-04)。我让 Fable 在每次变更落地时将其添加到变更日志的“未发布”部分，并随它一起审查。这产生了一个很好的附带效果：[变更日志的提交历史](https://github.com/simonw/sqlite-utils/commits/4.0rc2/docs/changelog.rst) 成为了发布中每个变更的简洁总结。

过去我坚持手动编写发布说明，但老实说，这些比我自己的更好。发布说明是一个很好的例子，说明我愿意外包给代理来编写，因为它们需要枯燥、可预测且准确。

> 破坏性变更：
>
> * 使用 `db.execute()` 执行的写入语句现在会自动提交，除非已打开一个事务，在这种情况下它们会加入该事务。以前它们会打开一个隐式事务，保持打开状态直到某些内容提交它——在同一连接上读取时写入似乎有效，但在连接关闭时会静默回滚。依赖回滚未提交 `db.execute()` 写入的代码应使用新的 `db.begin()` 方法先打开显式事务。事务模型在[事务与保存更改](https://sqlite-utils.datasette.io/en/latest/python-api.html#python-api-transactions)中有完整文档。
> * `db.query()` 现在在调用时立即执行其 SQL，而不是等到返回的生成器首次迭代。行仍然在迭代期间惰性获取。SQL 错误现在在调用点引发，诸如 `INSERT ... RETURNING` 之类的语句会立即执行并提交，无需迭代其结果，而传递一个不返回行的语句——以前是静默无操作——现在会引发 `ValueError`，建议改用 `db.execute()`。以这种方式拒绝的语句会在错误引发前回滚，因此对数据库没有影响。
> * Python API 验证错误现在引发 `ValueError` 而不是 `AssertionError`。以前无效参数——例如没有列的 `create_table()`，对不存在的表进行 `transform()`，或同时传递 `ignore=True` 和 `replace=True`——使用裸 `assert` 语句拒绝，当 Python 使用 `-O` 标志运行时这些语句会被静默跳过。捕获了这些情况的 `AssertionError` 的代码应改为捕获 `ValueError`。
> * `table.upsert()` 和 `table.upsert_all()` 现在如果记录缺少任何主键列的值，或者某列的值为 `None`，则会引发 `PrimaryKeyRequired`。以前这样的记录——永远无法匹配现有行——会被静默地作为全新行插入，或者在进行插入后引发令人困惑的 `KeyError`。
> * `db.enable_wal()` 和 `db.disable_wal()` 现在如果在事务打开时调用会引发 `sqlite_utils.db.TransactionError`。以前它们会通过更改日志模式静默提交打开的事务，破坏了 `db.atomic()` 和用户管理事务的回滚保证。
> * `View` 类不再具有 `enable_fts()` 方法。它之前存在只是为了引发 `NotImplementedError`，因为视图不支持全文搜索——现在调用它会引发 `AttributeError`，并且该方法不再出现在 API 参考中。`sqlite-utils enable-fts` 命令在指向视图时会显示清晰的错误。
> * 从 `insert` 和 `upsert` 命令中移除了无操作的 `-d/--detect-types` 标志。自 4.0a1 起，类型检测已成为 CSV/TSV 数据的默认行为，因此该标志什么都不做——使用它的调用应直接删除该标志。`--no-detect-types` 仍然可用于禁用检测。
> * `Database()` 现在如果传入了使用 Python 3.12+ `sqlite3.connect(..., autocommit=True)` 或 `autocommit=False` 选项创建的连接，会引发 `sqlite_utils.db.TransactionError`。`commit()` 和 `rollback()` 在这些连接上的行为不同，以前这会导致库所做的每次写入在连接关闭时被静默丢弃。
>
> 其他所有内容：
>
> * 修复了 `table.delete_where()`、`table.optimize()` 和 `table.rebuild_fts()` 不提交其更改，使连接保持在打开事务内的错误。它们的工作——以及后续的任何写入——可能在连接关闭时被静默回滚。现在这三个方法都使用 `db.atomic()`，与其他写入方法一致。
> * `sqlite-utils drop-table` 命令现在拒绝删除视图，而 `drop-view` 拒绝删除表。以前如果名称匹配，每个命令都会静默删除错误类型的对象。现在两者都会显示错误并建议使用正确的命令。
> * 由新的[迁移系统](https://sqlite-utils.datasette.io/en/latest/migrations.html#migrations)应用的迁移现在在事务内运行，同时记录迁移已应用。如果迁移引发异常，其更改会被回滚，并且它保持待定状态，因此在修复错误后可以安全地重新应用。无法在事务内运行的迁移，例如执行 `VACUUM` 的迁移，可以使用 `@migrations(transactional=False)` 选择退出——请参阅[迁移与事务](https://sqlite-utils.datasette.io/en/latest/migrations.html#migrations-transactions)。
> * `table.upsert()` 和 `table.upsert_all()` 现在会自动检测现有表的主键或复合主键，因此在向已有主键的表进行 upsert 时不再需要 `pk=` 参数。
> * `db.table(table_name).insert({})` 现在可用于向现有表插入完全由默认值组成的行，使用 `INSERT INTO ... DEFAULT VALUES`。（[#759](https://github.com/simonw/sqlite-utils/issues/759)）
> * 对 `sqlite-utils migrate` 命令的改进：不匹配任何已知迁移的 `--stop-before` 值现在会引发错误，而不是静默忽略；`--stop-before` 现在能正确处理仍使用旧版 `sqlite_migrate.Migrations` 类的迁移文件；`--list` 现在是只读操作，不再创建数据库文件或迁移跟踪表。`migrations.applied()` 现在按应用顺序返回迁移。
> * 新增 `db.begin()`、`db.commit()` 和 `db.rollback()` 方法，用于手动控制事务，作为 `db.atomic()` 上下文管理器的替代方案。
> * 新文档：[事务与保存更改](https://sqlite-utils.datasette.io/en/latest/python-api.html#python-api-transactions) 描述了事务的工作原理以及何时提交更改；新增的[升级](https://sqlite-utils.datasette.io/en/latest/upgrading.html#upgrading)页面详细说明了在主版本之间迁移所需的更改。

发布于[2026年7月5日](/2026/Jul/5/) 凌晨1点 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
