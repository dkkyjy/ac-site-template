---
title: "Hermes Agent - 文档总结"
category: "技能库 / 框架"
description: "Nous Research 的 Hermes Agent 文档全景总结:自进化 AI 代理的定位与两条设计铁律、六种运行入口、66 个顶层 CLI 子命令、技能/toolsets/插件三大扩展面、profile 路由与多网关、安全运维与仓库内工程文档索引。"
pubDate: "2026-09-11"
badge: "guide"
tags: ["hermes", "agent", "NousResearch", "AI工具", "agent-framework"]
---

# Hermes Agent 文档总结

> 仓库：[NousResearch/hermes-agent](https://github.com/NousResearch/hermes-agent) · MIT · 文档站 [hermes-agent.nousresearch.com/docs](https://hermes-agent.nousresearch.com/docs/)
> 本地实装：`hermes` v0.20.5（2026.8.19），完整 git checkout 在 `~/.hermes/hermes-agent`（upstream `7c973439`，1.9G），配置 `~/.hermes/`，二进制 `~/.local/bin/hermes`。
> 本文梳理 **README（含中文版）/ AGENTS.md 开发指南 / CLI 帮助 / 仓库内 `docs/` 工程文档** 四类来源。

---

## 1. 是什么

**由 Nous Research 构建的"自进化"个人 AI 代理**：同一套 agent core 同时驱动 CLI、消息网关、TUI 与 Electron 桌面端；跨会话学习（记忆 + 技能）、委派子代理、跑定时任务、驱动真实的终端与浏览器。定位强调**不绑定你的电脑**——可以在 5 美元 VPS、GPU 集群或近零成本的 Serverless 基础设施上运行，在 Telegram 上和它说话、它在云端 VM 上干活。

它**主要通过插件与技能扩展，而不是把核心做大**。两条铁律决定了几乎所有设计决策与代码评审口径：

1. **逐会话 prompt 缓存神圣不可侵犯。** 长会话每轮复用缓存前缀；任何改写历史上下文、中途换 toolset、重建 system prompt 的行为都会让缓存失效、成倍推高用户成本——**唯一例外是上下文压缩**。
2. **核心是"细腰"，能力在边缘。** 每个模型工具都会在每次 API 调用中被发送，因此新增 *核心* 工具的门槛极高；新能力应当以「CLI 命令 + 技能」「服务门控工具」或「插件」的形式到来。

配套还有两条工程哲学（AGENTS.md）：**Footprint Ladder**（新能力先走最小足迹那一级）与 **能力面属于 SESSION，而非进程环境**（禁止用进程级全局/环境变量偷带能力）。

---

## 2. 安装与运行形态

```bash
curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash   # Linux/macOS/WSL2/Termux
iex (irm https://hermes-agent.nousresearch.com/install.ps1)          # Windows PowerShell
```

安装器会创建**托管式完整 git checkout**（`HERMES_HOME/hermes-agent`，默认 `~/.hermes/hermes-agent`）与托管 venv；Termux 上只装 `.[termux]` 扩展集（`.[all]` 会拉入 Android 不兼容的语音依赖）。开发者路径：在托管 checkout 里 `uv pip install -e ".[all,dev]"` + `scripts/run_tests.sh`。

**六种入口**（同一 agent core）：

| 入口 | 说明 |
|---|---|
| **CLI** | `hermes` 进入交互式对话；`hermes model` / `tools` / `config set` / `setup` / `doctor` |
| **TUI** | `hermes --tui`，Ink(React) 终端界面（`ui-tui/` + Python JSON-RPC 后端 `tui_gateway/`） |
| **消息网关** | `hermes gateway setup` + `start`，一个网关进程同跑 Telegram、Discord、Slack、WhatsApp、Signal、Email 等约 20 个平台 |
| **桌面端** | Electron 应用（`apps/desktop/`）；`hermes dashboard` 的 `/chat` 也提供 TUI |
| **编辑器 / ACP** | `hermes acp` 提供 ACP 服务，接入 VS Code / Zed / JetBrains（`acp_adapter/`） |
| **Web / API** | `hermes serve`、`hermes web`、webhook、api_server 平台通道 |

---

## 3. 核心能力（README 七大特性）

| 能力 | 要点 |
|---|---|
| **真正的终端界面** | 完整 TUI：多行编辑、斜杠命令自动补全、会话历史、中断重定向、流式工具输出 |
| **随你所在** | Telegram / Discord / Slack / WhatsApp / Signal / CLI 全由一个网关进程服务；语音备忘录转写、跨平台会话连续性 |
| **闭环学习** | 代理自管记忆并定期自省；复杂任务后**自动创建技能**，技能在使用中自我改进；FTS5 会话搜索 + LLM 摘要实现跨会话回溯；Honcho 辩证式用户建模；兼容 agentskills.io 开放标准 |
| **定时自动化** | 内置 cron 调度器，可投递到任意平台；日报、夜间备份、周审计都用自然语言描述，无人值守 |
| **委派与并行** | 生成隔离子代理处理并行工作流；可写 Python 脚本通过 RPC 调工具，把多步管道压缩成零上下文开销的轮次 |
| **随处运行** | **七种终端后端**：本地 / Docker / SSH / Singularity / Modal / Daytona / Vercel Sandbox（`tools/environments/`）；Daytona 与 Modal 提供 Serverless 持久化，空闲休眠近零成本 |
| **研究就绪** | 批量轨迹生成、轨迹压缩，用于训练下一代工具调用模型 |
| **多模型** | 任意 provider，`hermes model` 切换，无锁定 |

---

## 4. 模型与 Provider

支持 Nous Portal、OpenRouter（200+ 模型）、NVIDIA NIM（Nemotron）、小米 MiMo、z.ai/GLM、Kimi/Moonshot、MiniMax、Hugging Face、OpenAI，以及自定义端点。

**Nous Portal**（`hermes setup --portal`，OAuth 登录）解决"到处收集 API Key"的问题：一个订阅覆盖 300+ 模型 + **Tool Gateway**（网页搜索 Firecrawl、图像生成 FAL、TTS OpenAI、云浏览器 Browser Use），全部托管、无需额外注册；Gateway 按**工具粒度**生效，可单独切回自己的 Key（`hermes portal info` 查看路由）。

另有 `hermes moa`（Mixture of Agents 槽位配置）、`hermes fallback`（主模型失败时的回退 provider）、`hermes proxy`（把 OAuth provider 暴露成本地 OpenAI 兼容代理）、`hermes auth`（凭据池）。

---

## 5. CLI 命令地图

顶层 `hermes --help` 列出 **66 个顶层子命令**（每个下还有子子命令与参数），按用途归类：

| 组 | 命令 |
|---|---|
| **会话与交互** | `chat`、`console`（安全命令控制台）、`sessions`、`send`、`completion`（补全脚本）、`prompt-size`、`insights`（用量分析） |
| **图形与后台** | `dashboard`（Web UI，默认端口 9119）、`serve`（headless 后端，供桌面端/远程）、`desktop`（别名 `gui`）、`acp`（Agent Client Protocol 服务） |
| **模型与路由** | `model`、`moa`、`fallback`、`proxy`、`portal`（Nous Portal / Tool Gateway）、`auth`、`logout` |
| **扩展** | `skills`、`bundles`（技能包/别名）、`curator`（后台技能维护）、`plugins`、`tools`、`mcp`、`hooks`、`lsp`、`computer-use`、`memory`、`pets`（petdex 动态宠物） |
| **消息平台** | `gateway`、`whatsapp`、`whatsapp-cloud`、`slack`、`pairing`（DM 配对码）、`webhook`、`peer`（跨机 bot-to-bot DM） |
| **自动化与协作** | `cron`、`kanban`、`project`（多文件夹命名工作区）、`worktree`（清理累积的 git worktree）、`sync`（Skill Sync 跨设备/仓库同步）、`checkpoints`、`pause` / `resume`（紧急停止 cron/kanban 派发与新网关轮次） |
| **安全与合规** | `approvals`（把历史审批挖成 allowlist 建议）、`security`（OSV.dev 供应链审计 venv/插件/MCP）、`secrets`（Bitwarden / 1Password）、`egress`（iron-proxy 出口凭证注入） |
| **诊断运维** | `doctor`、`verify`（探测项目运行配方并冒烟）、`status`、`logs`、`monitoring`、`dump`、`debug`、`config`、`skin`、`profile`、`backup`、`import`（从 zip 还原备份）、`import-agent` |
| **迁移与更新** | `claw`（OpenClaw 迁移）、`migrate`、`update`、`uninstall` |

---

## 6. 技能系统（Hermes 的灵魂）

两个并行面：

- **`skills/`** —— 默认加载的内置技能，按类目目录组织（如 `skills/github/`、`skills/mlops/`），本地实装 **15 个类目目录**。
- **`optional-skills/`** —— 重依赖或小众技能，随仓库分发但**默认不激活**，用 `hermes skills install official/<category>/<skill>` 显式安装（本地 **21 个**类目目录）。类目含 autonomous-ai-agents、blockchain、communication、creative、devops、email、health、mcp、migration、mlops、productivity、research、security、web-development。

**SKILL.md frontmatter**：`name`、`description`、`version`、`author`、`license`、`platforms`（OS 门控，如 `[linux, macos]`）、`metadata.hermes.{tags,category,related_skills,config}`；顶层 `tags:` / `category:` 也被接受（加载器会镜像到 `metadata.hermes.*`）。

**撰写红线（HARDLINE，违反即拒收）**：`description` ≤ 60 字符、单句、以句号结尾；只陈述能力而非实现；禁止营销词（powerful / comprehensive / seamless / advanced）；不复述技能名。技能所需的 config.yaml 设置走 `skills.config.<key>`，在 setup 期提示、加载期注入。

---

## 7. 工具与工具集

`toolsets.py` 中以单个 `TOOLSETS` 字典定义，每个平台适配器选一个**基础 toolset**（如 Telegram 用 `messaging`），`_HERMES_CORE_TOOLS` 是多数平台继承的默认包。文档所列 toolset 键 30 个：

`browser`、`clarify`、`code_execution`、`cronjob`、`debugging`、`delegation`、`discord`、`discord_admin`、`feishu_doc`、`feishu_drive`、`file`、`homeassistant`、`image_gen`、`kanban`、`memory`、`messaging`、`moa`、`rl`、`safe`、`search`、`session_search`、`skills`、`spotify`、`terminal`、`todo`、`tts`、`video`、`vision`、`web`、`yuanbao`。

用 `hermes tools`（curses UI）或 config.yaml 的 `tools.<platform>.enabled/disabled` 逐平台启停。委派走 `tools/delegate_tool.py`：子代理拥有隔离的上下文与终端会话，父代理默认等摘要继续，`background=true` 则后台跑。

---

## 8. 插件体系

扩展面在 `plugins/` 下，本地实装目录：`memory`（记忆 provider：honcho / mem0 / supermemory 等）、`context_engine`、`model-providers`（openrouter / anthropic / gmi 等）、`kanban`（多代理看板调度 + worker）、`observability`、`image_gen`、`video_gen`、`cron_providers`、`security-guidance`、`dashboard_auth`、`platforms`、`web`、`browser`、`spotify`、`google_meet`、`teams_pipeline`、`disk-cleanup`、`hermes-achievements`。

文档还给出 **Middleware 契约**（`docs/middleware/README.md`）：observer hook 只报告"发生了什么"，**middleware 能改变"会发生什么"**——在执行前重写请求，或包裹执行回调本身，契约刻意做成后端无关。

---

## 9. 记忆、会话与上下文

- **SessionDB**（`hermes_state.py`）：SQLite 会话存储 + FTS5 搜索，支撑跨会话回溯。
- **Micro-compaction**（`docs/micro-compaction.md`）：会话越过阈值时"停下 → 单次调用摘要中段 → 继续对话"的压缩路径，是 prompt 缓存铁律的唯一例外。
- **记忆 provider 插件化**：`hermes memory`（子命令 `setup` / `status` / `off` / `reset`）管理外部记忆 provider，可选 **honcho、openviking、mem0、hindsight、holographic、retaindb、byterover**，同一时间只有一个外部 provider 生效；**内置记忆（MEMORY.md / USER.md）始终有效**。Honcho 提供辩证式用户建模。
- 相关命令：`hermes sessions`、`insights`、`memory`。

---

## 10. 多实例：profile 路由与多网关

Hermes 支持 **profile**（如 default / writer / admin / coder / researcher）多实例并行与**按来源路由**（`gateway/profile_routing.py`、`docs/profile-routing.md`）：

- 每个 profile 有自己的记忆、人格、工具集；`get_hermes_home()`（`hermes_constants.py`）使所有路径 profile 化——AGENTS.md 明令**禁止硬编码 `~/.hermes` 路径**。
- **多网关部署**（`docs/kanban/multi-gateway.md`）：多个网关进程并发，各连自己的平台 API；但**只有一个网关拥有 kanban dispatcher**（single-dispatcher posture）。
- **Kanban** 是多 profile 协作看板（任务/链接/评论），配套 `docs/hermes-kanban-v1-spec.pdf` 与 `docs/design/kanban-dialogs/`。
- **Peer**：跨机器的 bot-to-bot DM（peer Hermes 网关）。

---

## 11. 安全与运维

| 面 | 机制 |
|---|---|
| **审批** | 命令白名单/审批模式；`hermes approvals` 可从历史挖出 allowlist 建议 |
| **出口隔离** | `docs/security/network-egress-isolation.md`：Docker 下默认 `network_mode: host` 会给代理无限制外连，文档给出分段方案——主要防御**提示注入**导致的数据外泄；配 `hermes egress`（iron-proxy 凭证注入防火墙） |
| **供应链** | `hermes security` 基于 OSV.dev 审计 venv / 插件 / MCP server |
| **可观测** | `docs/observability/monitoring.md`：网关健康 + 结构化诊断经 OTLP/HTTP 导出（OTel Collector / DataDog），**按构造不含内容**（content-free），只出生命周期状态、连接器健康与无内容的告警 |
| **紧急停止** | `hermes pause` 暂停 cron/kanban 派发与新网关轮次，`hermes resume` 解除 |
| **自检** | `hermes doctor`、`verify`、`status`、`dump`（含 `docs/rca-ssl-cacert-post-git-pull.md` 这类故障复盘文档） |
| **凭据** | `hermes secrets` 接 Bitwarden / 1Password；密钥只进 `~/.hermes/.env`，config.yaml 只放设置 |

---

## 12. 从 OpenClaw 迁移

`hermes claw migrate`（`--dry-run` 预览、`--preset user-data` 只迁用户数据不含密钥、`--overwrite` 覆盖冲突）。安装向导会自动检测 `~/.openclaw` 并在配置前提供迁移选项。导入内容：**SOUL.md 人格**、**记忆**（MEMORY.md / USER.md）、**技能**（落到 `~/.hermes/skills/openclaw-imports/`）、命令白名单、消息设置、白名单内 API 密钥、TTS 资产、工作区指令 AGENTS.md（`--workspace-target`）。

社区还有 **HermesClaw** 微信桥（同一微信账号同时跑 Hermes 与 OpenClaw）。

---

## 13. 开发与架构（AGENTS.md）

**核心文件**（`~/.hermes/hermes-agent/`）：

| 文件 | 职责 |
|---|---|
| `run_agent.py` | `AIAgent` 类——核心对话循环（约 12k 行） |
| `cli.py` | `HermesCLI`——交互式 CLI 编排（约 11k 行） |
| `model_tools.py` | 工具编排：`discover_builtin_tools()`、`handle_function_call()` |
| `toolsets.py` | toolset 定义与 `_HERMES_CORE_TOOLS` |
| `hermes_state.py` | `SessionDB`——SQLite 会话存储（FTS5 搜索） |
| `hermes_constants.py` / `hermes_logging.py` | profile 化路径 `get_hermes_home()` / 日志 `agent.log`、`errors.log`、`gateway.log` |
| `batch_runner.py` | 并行批处理（轨迹生成） |

目录：`agent/`（provider 适配、记忆、缓存、压缩）、`hermes_cli/`（子命令、setup 向导、插件加载、皮肤引擎）、`tools/`（自动发现，含 `environments/` 七种终端后端）、`gateway/platforms/`（每平台一个适配器）、`plugins/`、`skills/` + `optional-skills/`、`ui-tui/` + `tui_gateway/`、`acp_adapter/`、`cron/`、`website/`（Docusaurus 文档站）、`tests/`（**约 1.7 万个测试 / 约 900 个文件**，截至 2026-05）。

**贡献口径**：主分支合并的大多是真实报障的修复，产品面（平台、通道、provider、模型、桌面/TUI/看板功能）**刻意激进扩张**；克制只针对**核心 agent + 模型工具 schema**——"最小足迹"约束的是*能力如何接入核心*，不是产品能否长大（"expansive at the edges, conservative at the waist"）。已知坑位（AGENTS.md 明文）：**禁止硬编码 `~/.hermes` / `Path.home()/".hermes"`**（破坏 profile，PR #3575 修过 5 个此类 bug）、**所有 CLI 菜单选择器必须用 curses**（`hermes_cli/curses_ui.py`）、spinner/显示代码**禁用 ANSI `\033[K` 擦除**（会被 prompt_toolkit 泄漏成字面 `?[K`）、工具 schema 描述**不得跨 toolset 指名其他工具**（工具可能未启用，诱导模型幻觉调用）、测试不得硬编码 `~/.hermes/`（conftest 的 `_isolate_hermes_home` 会把 HERMES_HOME 重定向到临时目录）。自动化 triage 只允许以 `implemented_on_main` / `cannot_reproduce` / `incoherent` 三个理由关闭 PR，**口味型"我们不要这个/超范围"必须留给人类维护者**。

---

## 14. 文档地图

**外部文档站**（`hermes-agent.nousresearch.com/docs`，源码在 `website/`）：

| 章节 | 内容 |
|---|---|
| 快速开始 | 安装 → 设置 → 2 分钟首次对话；Termux 指南 |
| CLI / 配置 / 消息网关 | 命令、快捷键、人格、会话；配置文件与全部选项；六平台接入 |
| 安全 | 命令审批、DM 配对、容器隔离 |
| 功能 | 工具与工具集（40+ 工具、终端后端）、技能系统、记忆、MCP 集成、cron、上下文文件、Tool Gateway |
| 开发者指南 | 架构（项目结构、代理循环、关键类）、贡献 |
| 参考 | 全部 CLI 命令与标志、环境变量 |
| 迁移 | OpenClaw → Hermes |

**仓库内工程文档**（`docs/`，偏内部契约）：`ADR.md`（架构决策记录）、`session-lifecycle.md`、`micro-compaction.md`、`profile-routing.md`、`kanban/multi-gateway.md`、`observability/monitoring.md`、`security/network-egress-isolation.md`、`relay-connector-contract.md`（v1 实验性契约）、`chronos-managed-cron-contract.md`（托管 cron，网关可缩容到零仍能触发任务）、`billing-lifecycle.md`、`middleware/README.md`、`streaming-tts.md`、`rfcs/`、`design/`、`hermes-kanban-v1-spec.pdf`。

社区：Discord、技能中心 [agentskills.io](https://agentskills.io)、GitHub Issues/Discussions。许可证 MIT（由 Nous Research 构建）。
