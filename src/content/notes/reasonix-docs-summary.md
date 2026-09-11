---
title: "Reasonix - 文档总结"
category: "专项工具 / 科研"
description: "Reasonix(esengine/DeepSeek-Reasonix)文档全景总结:Go 单二进制编码 Agent 的设计原则、四个入口、CLI 命令地图、配置体系、权限与沙盒、Checkpoints、子智能体、Goal 模式、扩展协议与文档索引。"
pubDate: "2026-09-11"
badge: "guide"
tags: ["reasonix", "coding-agent", "DeepSeek", "AI工具", "agent"]
---

# Reasonix 文档总结

> 仓库：[esengine/DeepSeek-Reasonix](https://github.com/esengine/DeepSeek-Reasonix) · MIT · 官网 [reasonix.io](https://reasonix.io/) · 文档站 esengine.github.io/DeepSeek-Reasonix · Discord 双语社区
> 本文对项目 `docs/` 下约 50 组中英双语文档做全局梳理。**本地实测版本：`reasonix desktop-v1.30.0`**（二进制 `~/go/bin/reasonix`，配置目录 `~/.reasonix/`）。

---

## 1. 是什么

**Reasonix 是一个"可以一直开着跑"的编码 Agent**：一套本地引擎，四个入口（终端 / 桌面端 / 浏览器 / 通过 ACP 接入编辑器）。计划模式、权限、工作区沙箱与逐轮 checkpoint，让长时间自治运行始终**可读、可撤销**。

| 项 | 值 |
|---|---|
| 语言 / 分发 | Go（1.0 起为 Go 重写版），`CGO_ENABLED=0` 单个静态二进制，一条命令交叉编译 6 个目标平台 |
| 许可 | MIT（代码签名：Windows 安装器由 SignPath 基金会免费提供证书） |
| 设计内核 | 极薄 harness 驱动多个模型，**所有能力由配置和插件提供** |
| 模型预设 | DeepSeek 内置；任何 OpenAI 兼容端点只是一条配置 |
| 分支 | `main-v2` 活跃开发（Go 1.0+）；`v1` 为旧 TypeScript `0.x` 只维护分支 |

四大特性（README）：

- **配置驱动** —— provider、agent、启用的工具、插件全部在 `reasonix.toml` 声明，内核无硬编码模型。
- **多模型 · 可组合** —— 可选"执行器 + 规划器"双模型协同，各自独立、缓存稳定的 session。
- **插件驱动** —— MCP server 提供工具/提示词/资源；Extension Protocol Sidecar 可拦截运行时事件、提供 Provider 与结构化 UI。
- **缓存友好的上下文维护** —— 启动注入稳定环境摘要；旧工具输出先 snip/prune 再 compaction；工具 schema 合约有文档与回归测试保护。

---

## 2. 工程规格（SPEC.md）

`SPEC.zh-CN.md` 是**规范性工程契约**：代码须遵循；要改行为先改契约再改代码。章节共 9 节。

**设计原则（前 5 条）**：

1. **配置与插件驱动** —— 核心只依赖接口；模型/工具通过 registry 按名解析，不硬编码 `switch model`。
2. **单一静态二进制** —— `CGO_ENABLED=0`，跨平台一条命令，CLI 开箱即用。
3. **精简依赖** —— 默认标准库；第三方必须纯 Go 且不破坏单二进制分发；**TOML parser 是当前唯一接受的基础依赖**。
4. **两级扩展** —— 编译期 built-in 通过 `init()` 自注册；运行时外部插件以 stdio JSON-RPC 子进程或 MCP 兼容传输接入。
5. **接口优先、registry 驱动** —— `Provider` 与 `Tool` 都是接口。

结构：`§2 目录与依赖方向` → `§3 核心抽象`（最大章节）→ `§4 数据类型` → `§5 配置` → `§6 错误处理` → `§7 代码风格` → `§8 分发` → `§9 路线图（当前范围之外）`。

---

## 3. 安装与四个入口

| 路径 | 安装方式 | 说明 |
|---|---|---|
| **A. CLI / TUI** | `npm i -g reasonix`（自动拉对应平台原生二进制）；macOS 亦可 `brew install esengine/reasonix/reasonix` | 预编译归档 + `SHA256SUMS` 见 GitHub Releases |
| **B. 桌面端** | 官网下载：macOS 通用 `.dmg`/`.zip`、Windows `.exe`/便携 `.zip`、Linux `.deb`/`.tar.gz` | 应用内配置 provider，无需跑 CLI 命令 |
| **C. VS Code 扩展** | Marketplace / Open VSX，扩展 ID `SivanLiu.reasonix-agent` | **不内置 CLI**，而是启动本机 `reasonix acp` 后端，提供原生聊天、编辑器上下文、工具审批、模型选择 |
| **D. 从源码构建** | CLI 需 Go 1.25+（`make build` / `make cross`）；桌面端另需 Node 24+、pnpm 10、与 `.wails-version` 一致的 Wails CLI（`cd desktop && wails build`） | 桌面端为 Wails 应用 |

CLI 与桌面端**共用同一个本地引擎**与同一个 Reasonix home。

---

## 4. CLI 命令地图

### 4.1 会话与一次性运行

```sh
reasonix                                  # 交互式会话（TUI）
reasonix --continue                       # 续接上次会话
reasonix --resume [QUERY]                 # 恢复（可按关键词查）
reasonix -p|--print <task>                # 一次性输出，支持 --output-format text|json|stream-json
reasonix run [--max-steps N] [--copy] <task>   # 子进程/自动化友好；支持 stdin 管道
reasonix run --events-jsonl <task>        # 输出脱敏结构化事件 JSONL
reasonix review [--base BRANCH|--commit SHA]   # 基于本地 diff 的 AI 代码审查
reasonix web [--addr HOST:PORT] [--no-open]     # 本地 Web UI 并打开浏览器
reasonix serve [--auth none|token|password]     # HTTP+SSE 服务（可带认证）
reasonix acp                              # 以 stdio 提供 Agent Client Protocol
```

参数：`--model NAME`、`--effort LEVEL`、`--permission-mode MODE`、`--add-dir PATH`、`--allowed-tools RULES`。

### 4.2 管理与运维

| 命令 | 用途 |
|---|---|
| `reasonix setup [path]` | 交互式配置向导，生成 `reasonix.toml`（及 `.env`）；统一管理 provider、模型列表、凭据、连接测试与默认模型（改动"保存并退出"生效） |
| `reasonix config reasoning-language [auto\|zh\|en]` / `compact-ratio [65..85]` / `telemetry [auto\|on\|off]` | 可见思考语言 / 自动压缩阈值 / 不含内容的 CLI 统计 |
| `reasonix mcp add\|remove\|list\|import` | 管理 `reasonix.toml` 里的 MCP 服务器 |
| `reasonix subagent list\|create\|edit\|delete\|try\|run` | 管理与运行隔离子智能体 profile |
| `reasonix init` | 查看如何生成项目记忆（`AGENTS.md`） |
| `reasonix doctor [--json]` | 输出脱敏本地诊断信息；`doctor session <branch-id>` 导出会话冲突诊断 zip |
| `reasonix session list\|show\|status\|recovery --json` | 面向机器客户端的脱敏会话接口 |
| `reasonix hook list\|status --json` | 查看脱敏 Hook 状态 |
| `reasonix task list\|show\|status\|events\|stop\|cancel\|monitor\|tmux --json` | 查看/控制脱敏 Task |
| `reasonix bot start\|doctor\|weixin-login` | 多渠道 IM bot 网关 |
| `reasonix report list\|show\|send\|delete` | 审阅并**明确发送**本地 CLI 崩溃报告 |
| `reasonix upgrade [--check] [--force]` / `completion bash\|zsh\|fish` / `version [--verbose\|--json]` | 更新 / 补全脚本 / 版本 |

---

## 5. 配置体系

**优先级：** `flag` > `./reasonix.toml`（项目级）> `<Reasonix home>/config.toml` > 内置默认值。

**Reasonix Home**（v1.8.1 起，CLI 与桌面端共用）：

| 平台 | 路径 |
|---|---|
| macOS / Linux | `~/.reasonix` |
| Windows | `%APPDATA%\reasonix` |

`REASONIX_HOME` 可覆盖（主要用于测试、CI 或便携安装）；设置后运行时会变成**完整自包含模式**——配置、状态、缓存、数据都在该目录树下。

**密钥**一律通过 `api_key_env` 从环境变量注入（如 `DEEPSEEK_API_KEY` / `MIMO_API_KEY` / `ANTHROPIC_API_KEY`），不写进 toml。

主要的配置块：

```toml
default_model = "deepseek"

[[providers]]                    # 任意 OpenAI 兼容端点 = 一条配置
name = "deepseek"
kind = "openai"
base_url = "https://api.deepseek.com"
models = ["deepseek-v4-flash", "deepseek-v4-pro"]
api_key_env = "DEEPSEEK_API_KEY"

[agent]
compact_ratio = 0.80             # 唯一的自动维护阈值（预设 0.70 活跃 / 0.80 推荐默认 / 0.85 较晚）
# planner_model = "deepseek-pro" # 可选：开启双模型协同（低频规划器）
# subagent_model = "deepseek-pro"                                    # runAs=subagent 的 skill 默认模型
# subagent_models = { review = "deepseek-pro", security_review = "deepseek-pro" }  # 按 skill 覆盖
# max_subagent_depth = 2         # 子代理嵌套委派深度
# max_subagent_concurrency = 6   # 会话级子代理总并发（task/fleet/skills）
# max_parallel_writers = 3       # 互不重叠 write_paths 时的并行写入上限
# max_output_tokens = 32768      # 0=provider 自动；可选控费上限
# goal_token_budget = 20000000
# task_cost_budget = 5.0
# task_time_budget_minutes = 60

[sandbox]
workspace_root = ""              # 文件写工具被限制在此目录；留空 = 当前目录
allow_write = ["/tmp"]           # 额外可写目录
forbid_read = ["${HOME}/.ssh"]   # agent 不可读/不可列的路径（用绝对路径或 ${VAR}）

[serve]
# token = ""  password_hash = ""  behind_proxy = false
```

GUI 侧还有 `reasoning_language`（可见思考语言 auto/zh/en，不改最终回答语言）、`cursor_shape`、`shortcut_layout` 等。GUIDE 另含 **远程 SSH**、**自定义 OpenAI-compatible provider**、**快捷键（桌面端 GUI / CLI-TUI 两套）**、**Web 前端**、**内置文档检索**等章节。

---

## 6. 三条正交的"轴"（最容易被混淆的设计）

文档反复强调：**协作方式 / 工具权限 / 沙盒** 是三条互相独立的轴。

### 6.1 协作方式（怎么推进）

| 模式 | 行为 |
|---|---|
| 普通 | 直达 executor |
| **计划模式** | 先产出计划、确认后再实施；**不是权限边界**，规划期工具调用仍受 Ask/Auto/Yolo、权限规则与沙盒约束 |
| **目标模式（Goal）** | 给一个目标持续推进到完成/阻塞/暂停/清除 |

没有"自动任务模式"。唯一的会话角色是**质量底线**：`standard`（默认）或 `delivery`。普通请求一律进 executor；Planner 只响应显式 Plan、批准边界与 Goal 启动。

### 6.2 工具权限（要不要问）

三种模式始终可见、可直接切换：**询问 / 自动 / Yolo**。

- 规则按**权限规则**匹配而非按钮文案：`Bash(npm run build)`、`Bash(npm run test:*)`、`Edit(docs/**)`，优先级 `deny > ask > allow > 兜底`。
- 只读工具一般不需要审批；Bash 与文件修改都要审核。
- 参数展开、赋值、裸 heredoc、重定向、glob **不能**复用裸 `Bash`/前缀/glob Allow；命令替换、动态命令名、`eval`、`source`、`shell -c`、运行时内联代码**默认强制人工**。高级用户可 `[permissions] allow_dynamic_bash = true`（显式 ask/deny 仍优先）。
- 无头运行没有审批界面：默认 Ask 对普通 writer fallback 与显式 ask 规则 **fail closed**；无人值守需 `reasonix run --auto ...` / `-y` / `--permission-mode auto`。

### 6.3 沙盒（能力的强制边界）

**权限是策略，沙盒是强制**——已放行的调用仍不能写出已批准根目录。

- 文件写工具（`write_file`/`edit_file`/`multi_edit`/`move_file`）拒绝 `workspace_root` 之外的任何路径，并解析符号链接与 `..`，链接无法打洞越界。
- 出工作区走交互式"扩展写入范围"审批（本次/本会话/写入项目 toml/拒绝），不退化成无沙箱执行。
- Bash 必须用 `additional_write_dirs` + `justification` 声明目录，**宿主不从命令文本猜路径**；无头 `reasonix run` 需 `--add-dir` 或 `[sandbox].allow_write`。
- 文件系统根与 Reasonix 会话/状态目录**不能**通过动态流程批准。
- `bash` 默认进 **OS 沙盒**：macOS 用 **Seatbelt**、Linux 用 **bubblewrap**；`[sandbox] network` 为真才可联网。
- 凭据隔离：工具子进程环境中移除已保存的 provider/bot 凭据变量，全局凭据 `.env` 自动加入运行时禁读边界。
- **会话私有临时目录**：同一逻辑会话的多条 Bash 共享一个私有 TMPDIR（`/new`、`/clear`、切分支时旋转），跨进程 resume 不恢复其中内容。

---

## 7. 上下文、记忆与检索

- **缓存稳定**优先：系统前缀与工具 schema 保持字节稳定；`max_output_tokens` 与 `compact_ratio` 解耦（0 表示 provider 自动值，服务端 384K）。
- **Context Engine v2 / 记忆检索**（`SESSION_MEMORY_RETRIEVAL`）：把"**常驻指令**"（定义 agent 必须怎样工作，如 `AGENTS.md` / `REASONIX.md` / `CLAUDE.md`）与"**背景记忆**"（可能过时的事实）分层——**一个事实不应静默升级成命令，一条长期规则不应依赖检索是否命中**。
- **内置文档检索**与 **`@` 引用**：`@path/to/file` / `@dir` 注入本地文件内容或目录清单，`@<server>:<uri>` 注入 MCP 资源；仅真实存在的路径才当引用，普通 `@mention` 保持原文。输入 `/` 或 `@` 弹出补全（斜杠命令 / 逐层文件导航 / MCP 资源）。
- **历史搜索 Catalog**：`<cache root>/history-search/v1.sqlite`，FTS5 只存规范化 token（英文小写+代码符号语义，CJK 重叠 bigram），snippet 再从权威 JSONL 读取——**删库不删会话**。

---

## 8. Checkpoints 与 Rewind（编辑安全网）

状态：**Phase 1 + 2 已实现**。

- 机制是**文件快照，不是 git**：不提交、不暂存、不改 `.git/`，**非 git 目录同样可用**。
- 只跟踪可预览的编辑工具变更：`write_file` / `edit_file` / `multi_edit`（`move_file` 暂不进预览）。
- 入口：CLI `Esc-Esc` 与 `/rewind` 选择器、桌面端悬停 rewind；菜单对齐 Claude Code：恢复代码 / 恢复会话 / 同时恢复 / 从此处分叉 / 从此处开始摘要 / 摘要到此处。
- 对话回溯改为**显式分叉，父会话永不截断**（见 `SESSION_OWNERSHIP`）。

**会话所有权**：同一会话文件同一时刻只有一个跨进程写者，用 session lease 文件；生产路径上 Controller 绑定带 generation 的 `SessionWriter`，重新绑定令旧 generation 立即失效。

---

## 9. 子智能体（Subagent）

- 形态：**带 `runAs: subagent` 的手动 Skill**——复用现有 Skill 文件格式与目录，不引入独立数据库；启动隔离子智能体，只把最终答案返回父智能体。
- 桌面端、交互式 CLI、Headless CLI **共用同一套 Profile**；CLI 侧 `reasonix subagent list|create|edit|delete|try|run`。
- 运行形态：`task`、`read_only_task`、`parallel_tasks`、`fleet`。
- **进度展示零配置**：进度卡片显示阶段、耗时、最近活动；桌面可展开查看受限 reasoning/回答/notice 预览，CLI 在 `/verbose` 下同样可见，且**子 Agent 的 reasoning/正文不进入父对话**（复用保留的 `reasonix.subagent.*` 进度事件名）。
- 委派控制：`max_subagent_depth`（默认 2，设 1 恢复单层边界）、`max_subagent_concurrency`（默认 6）、`max_parallel_writers`（默认 3）。

---

## 10. Goal 模式与"结构化完成协议"

`/goal` 是**唯一的跨 turn 调度器**，与 Delivery（纯质量门禁）、权限/沙盒保持正交。

- **默认不设上限**：模型轮数、跨 Run turn 数、墙钟时长、无进展数字阈值都不设限——持续执行直到完成、仅用户/外部条件可解阻塞、用户暂停/停止、不可恢复外部错误或耗尽显式预算。
- **结构化完成协议**：每轮结束模型通过 `update_goal` 工具报告 `continue` / `complete` / `blocked`（含 reason 与 next_action），取代旧的 `[goal:*]` 文本标记。
- **完成校验与对账**：`complete` 可附自述（`verified` / `unverified` / `risks`）；`verified` 命令逐条与本会话真实 receipt 对账，**没跑过 / 跑失败 / 早于最后一次改动**都记为 unbacked claim。
- **独立评审**：模型没报告时，宿主调用一次有界 evaluator 判定；评审不可用/出错/不确定时**安全暂停，绝不默认继续**。
- 进展按 **Goal 范围的新颖性**计算：新的读取/搜索结果、mutation、verification、todo 变化与 review 推进目标；**完全相同的工具+参数+结果重复不推进**。停滞只注入纠偏提示，不暂停 Goal。
- 预算：`goal_token_budget`（默认 0=关闭），超限先生成总结再进入可恢复的 `budget_spend` 暂停，`/goal resume` 授予新预算切片但累计统计不清零。
- 批量签收：宿主可在同一轮处理多个 `complete_step`，但**必须严格遵循 canonical Todo 顺序**，跳过/待办/乱序会被拒绝。

配套：**任务合约（Task Contract）**模板 —— `Context / Request / Output format / Constraints / Pause policy`（Reasonix 用 "Pause policy" 而非 "Checkpoint"，避免与快照混淆）。

---

## 11. 双模型协同

```toml
[agent]
planner_model = "deepseek-pro"   # 作为低频规划器
```

- Planner 能看到已加载的 `REASONIX.md` / `AGENTS.md` 记忆，并拿到一小组**只读研究工具**，先检查文件再把计划交给执行器；**写入类/流程类工具只给执行器**。
- 路由是**确定性规则，不再调用额外 classifier 模型**：普通请求直达 Executor；Planner 只响应显式「先规划 / plan first」、显式等待批准、显式「只规划 / 不要执行」，或 Goal 启动。"复杂重构""修复登录"这类措辞**不会**自动启动 Planner。
- Planner 使用同一稳定 system prompt，单轮只追加很小的 `<planner-turn>`，除 prompt 升级的一次性缓存未命中外不持续破坏 prefix cache。

---

## 12. 扩展体系（三层）

| 层 | 内容 | 信任 |
|---|---|---|
| **声明式**（任意插件包） | skills、agents、commands、prompts、hooks、MCP servers、主题 —— 都是文件与配置 | 按宿主正常权限运行 |
| **代码型 Runtime**（Manifest v2 `runtime` 块） | 通过 Extension Protocol 驱动的 Sidecar 进程：拦截运行时事件、持有替换策略、提供流式模型 Provider、发布结构化 UI | **完全信任（full trust）**，安装前务必读安全章节 |
| **MCP** | stdio JSON-RPC 子进程或 MCP 兼容传输；提供工具、提示词、资源 | 需显式启动第三方服务器 |

- **Extension Protocol v2**：协议 ID `reasonix.extension.v2`；机器可读 schema 在 `internal/extension/protocol/schema.generated.json`；方法/事件/限额索引 `docs/EXTENSION_PROTOCOL.generated.md`（CI 防漂移）；官方 **Go SDK** 在 `sdk/go`。
- **插件包**：`reasonix plugin install git:github.com/obra/superpowers`（或 https URL），当前为**全局范围**安装，写入 Reasonix home；把 skills/hooks/MCP/prompts/主题/代码扩展组织成一个可安装单元。
- **Hooks**：在会话、用户输入、工具调用、模型返回、压缩上下文等节点执行本地 shell；桌面端"设置 → Hooks"图形编辑，本质读写同一份 `settings.json`（全局与项目 hooks 自动加载）。
- **主题包 v2**：**受控皮肤**——语义颜色令牌、密度/圆角配方、本地背景图；**不能**执行 CSS/JS、加载字体、远程 URL 或 SVG 脚本。八款官方主题图片全部由 `scripts/official-theme-art/` 用 numpy + Pillow 固定种子程序化生成（原创、无第三方像素）。

---

## 13. 其他能力

- **IM Bot**：连接飞书 / Lark / 微信 / QQ，在 IM 里使用 Reasonix（审批、问答、YOLO 与常用命令的交互方式见 `BOT_GUIDE`）；`[bot].max_steps` 中 `0` 表示自动持续执行。
- **Web / serve**：`reasonix web` 启动本地 UI；`reasonix serve` 提供 HTTP+SSE 并可 `--auth none|token|password`（`--hash-password` 生成 `password_hash`）。
- **ACP 编辑接入**：实现 Agent Client Protocol v1，通过 stdio 提供 NDJSON JSON-RPC 2.0 agent；host 负责启动进程与工作区会话。
- **能力诊断**（`CAPABILITY_DIAGNOSTICS`）：CLI 与桌面端"设置 → 诊断"共用的**只读**诊断模型，覆盖 Skills、Commands、Hooks、插件包、MCP 服务器与指令文件；静态模式无网络、不启动 MCP 子进程，`--live` 才探测。
- **可观测与隐私**：`reasonix doctor --json` 脱敏诊断；`session/task/hook` 系列 `--json` 脱敏机器接口；崩溃报告需用户**明确发送**；telemetry 为**不含内容**的 CLI 使用统计。
- **数据存储**：`usage-catalog/v1.sqlite`、`history-search/v1.sqlite`、`task-catalog/v1.sqlite` 都是**可丢弃投影**——JSONL / snapshot / event log 才是权威数据，删库不丢会话、不丢任务。
- **计费**：把 `original`（原币估算）/ `valuations`（调用时 identity 与官方区域价表）/ 钱包余额（供应商返回事实）三类事实分开；运行时不下载、不缓存 FX。
- **恢复**：`RECOVERY` 记录恢复与安全模式；`production_checklist` 为 v5 稳定版发布门禁（运行时安全、控制平面确定性等）。

---

## 14. 文档地图（docs/ 约 50 组中英双语）

| 文档 | 内容 |
|---|---|
| `GUIDE` | 使用指南（88K，全量配置/权限/沙盒/插件/斜杠命令/Goal/@引用/双模型） |
| `CLI` | CLI 命令参考（会话、一次性自动化、恢复、权限、会话内命令） |
| `SPEC` | 工程规格（设计原则、目录与依赖方向、核心抽象、数据类型、配置、错误、风格、分发、路线图） |
| `CONFIG_PATHS` | Reasonix Home 与配置路径 |
| `ACP` | 编辑器接入（ACP v1） |
| `TOOL_CONTRACT` / `TOOL_APPROVAL_MODES` | 内置工具的 provider-visible 合约 / 询问·自动·Yolo |
| `TASK_CONTRACT` | 任务合约与暂停策略模板 |
| `COLLABORATION_MODES` / `GOAL_ENFORCEMENT` | 计划·目标模式 / Goal 连续执行与完成协议 |
| `CHECKPOINTS` / `SESSION_OWNERSHIP` | 快照与 rewind / 会话写者与回溯 |
| `SESSION_MEMORY_RETRIEVAL` | Context Engine v2 记忆分层 |
| `SESSION_CATALOG` / `SESSION_REFERENCE_ARCHITECTURE` | 会话目录 / `@` 引用其他会话（Issue #3185） |
| `SUBAGENT_PROFILES` / `SUBAGENT_PROGRESS` | 子智能体 Profile / 本地子 Agent 进度展示 |
| `EXTENSIONS` / `EXTENSION_PROTOCOL` / `EXTENSION_RUNTIME_V2` / `PLUGIN_PACKAGES` | 扩展概览 / 协议 v2 / 时空可组合性 / 插件包 |
| `DESKTOP_HOOKS` / `THEME_PACK` / `THEME_ASSETS` | 桌面 Hooks / 主题包 v2 / 官方主题素材与许可 |
| `BOT_GUIDE` | 飞书·Lark·微信·QQ 机器人 |
| `BILLING` / `USAGE_CATALOG` / `HISTORY_SEARCH_CATALOG` / `TASK_CATALOG` | 计费币种 / 用量 / 历史搜索 / 任务投影 |
| `CAPABILITY_DIAGNOSTICS` / `RECOVERY` / `MIGRATING` / `production_checklist` | 能力诊断 / 恢复与安全模式 / 从 0.x 迁移 / 发布门禁 |
| `DESKTOP_CRASH_DIAGNOSTICS_RUNBOOK` | Windows/Linux 桌面崩溃诊断运行手册（旧 Win LTSC 手册已并入） |
| `research/` `superpowers/` `themes/` `assets/` | 专题目录与素材 |

---

## 15. 迁移：0.x (TypeScript) → 1.0 (Go)

| | v1（旧） | 1.0+（v2） |
|---|---|---|
| 语言 | TypeScript / Node.js | **Go** |
| 分支 | `v1`（仅维护） | `main-v2`（默认、活跃） |
| 版本 | `0.x`（最高 v0.54.x） | `1.0.0`+ |

这不是增量升级而是**从零重写**：配置目录统一到用户可见的 Reasonix home（v1.8.1 起），旧的简单/写入/研究参数与 `[agent].max_steps` 只作兼容元数据解析（值被忽略并一次性迁移移除），显式预算改用 CLI `--max-steps`。旧 `.reasonix/autoresearch/<task-id>/` 保持只读。

近期变更重点（CHANGELOG "Unreleased"）：**事实驱动执行** —— 取消自动 simple/light/full 任务模式与逐轮 `TaskPolicy` 分类；宿主根据真实工具动作与 receipt 建立验证义务；Plan / Goal / 权限 / 沙箱四者独立；工具 schema 与 executor 系统前缀保持字节稳定。

---

## 16. 本机实测速查

```bash
reasonix --version        # → reasonix desktop-v1.30.0
reasonix --help           # 全部子命令
reasonix doctor --json    # 诊断（含 config.source_path）
ls ~/.reasonix            # config.toml / agents/ / .env / 状态与缓存
```

| 项 | 值 |
|---|---|
| 二进制 | `~/go/bin/reasonix` |
| 生效配置 | `~/.reasonix/config.toml`（优先级高于仓库内 `reasonix.toml`） |
| 源码 | `~/storage/github/DeepSeek-Reasonix`（`main-v2`） |
| 版本 | `desktop-v1.30.0` |

> **排障提示**：provider 配置**不支持** `retry`/`timeout`/`no_proxy` 字段；重试由内建 `internal/provider/retry.go` 决定（`MaxRetries=10`，退避 500ms→2^n→上限 15s，除 `ctx.Canceled/DeadlineExceeded` 外都判可重试）。`dial tcp <IP>:443: i/o timeout` 属连接阶段网络瞬断（非 DNS、非配置 bug）——先 `curl <base_url>/chat/completions` 实测，返回 200 即已自愈，直接重跑即可。

---

## 17. 参考

- [README（中）](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/README.zh-CN.md) · [使用指南](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/GUIDE.zh-CN.md) · [CLI 参考](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/CLI.zh-CN.md) · [工程规格](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/SPEC.zh-CN.md)
- 扩展开发：[扩展概览](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/EXTENSIONS.zh-CN.md) · [Extension Protocol](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/EXTENSION_PROTOCOL.zh-CN.md) · [插件包](https://github.com/esengine/DeepSeek-Reasonix/blob/main-v2/docs/PLUGIN_PACKAGES.zh-CN.md) · Go SDK `sdk/go`
- [官方网站](https://reasonix.io/) · [文档站](https://esengine.github.io/DeepSeek-Reasonix/) · [Discord](https://discord.gg/XF78rEME2D)

> 本文为文档梳理（2026-09-11，基于 `desktop-v1.30.0` 本地源码）。实现细节以仓库规范版本（英文）为准。
