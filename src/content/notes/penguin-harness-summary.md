---
title: "PenguinHarness (Prism-Shadow) - RSI 引擎研究报告"
category: "技能库 / 框架"
description: "PenguinHarness 深度研究报告：hiyouga（LlamaFactory 作者）的 RSI 多智能体平台，OmniMessage 协议、ReAct 循环、自我进化闭环（Builder→Benchmark→Evaluator→Optimizer→Snapshot），以及本地安装实测记录。"
pubDate: "2026-09-10"
badge: "reference"
tags: ["penguin-harness", "RSI", "self-improving", "hiyouga", "Prism-Shadow"]
---
> 研究日期：2026-09-10 | 材料：`temp/penguin-harness/`（浅克隆 @ main，32MB）+ GitHub API
> 仓库：https://github.com/Prism-Shadow/penguin-harness

---

## 1. 项目档案

| 项 | 值 |
|---|---|
| 全名 | Prism-Shadow/penguin-harness |
| 一句话 | 🐧 Harness for RSI. Let AI Build AI. Multi-Agent Auto-Dev Platform. Everything is Transparent. |
| Star / Fork | ⭐ 2054 / 216 |
| 语言 | TypeScript（monorepo，pnpm workspace） |
| License | Apache-2.0 |
| 创建 | 2026-07-19（约 2 个月前，极新、极活跃） |
| 最近推送 | 2026-09-10（研究当天仍有更新） |
| 作者 | **Yaowei Zheng（hiyouga）—— LlamaFactory 作者**；团队 PrismShadow AI Team + Fable 5（Anthropic 新模型） |
| 官网 | penguin.ooo |
| 运行时 | Node ≥ 24；支持 Linux / macOS / Windows 10+ / x64 / arm64 |

**定位**：开源、本地优先的多智能体自动开发平台，核心目标是 **RSI（Recursive Self-Improvement，递归自我改进）**——让 AI 构建 AI。

---

## 2. 三大支柱（README 原话）

1. **🏆 以几十分之一的成本跑出优异效果** —— 刻意精简工具集 + 干净底层接口：更少工具调用、更少 Token，对 DeepSeek 等开放模型深度适配。基准对比：数据分析任务与 Claude Code 效果相当，成本约 1/70；用 DeepSeek V4 Pro 生成整个 RAG 应用仅消耗 0.2 元（$0.02）。
2. **🔍 一切透明** —— 每次请求有完整轨迹（Trace），可回放、可审计。
3. **🧬 原生 Agent 自进化引擎** —— Agent 自己评估、自己优化：跑 Benchmark、找失分点、发布 N+1 版；每轮之前自动快照，每个请求可在轨迹观测中回放。

---

## 3. 系统架构

pnpm monorepo，7 个包。核心是 `@prismshadow/penguin-core` 中的执行引擎；CLI / Server / Web App 都只是这同一引擎的不同「Human 实现」。

```
┌─────────────┐  ┌─────────────────────────────┐
│   CLI       │  │  Web App / Desktop (Electron) │
└──────┬──────┘  └──────────────┬──────────────┘
       │         @prismshadow/penguin-server (多用户鉴权/SSE会话/用量统计)
       └──────────────┬──────────────┘
              @prismshadow/penguin-core（执行引擎）
               ├─ context_engine  （ReAct 循环）
               ├─ omnimessage     （OmniMessage 协议 + markers）
               ├─ llm / environment(mcp/tools) / state / trace
               └─ plugins / hooks / extension
```

| 包 | 说明 |
|---|---|
| `core` | 核心 SDK：context_engine、OmniMessage 协议、LLM/Environment/Agent State 接口 |
| `cli` | 交互式 REPL + 单任务运行器 |
| `server` | 无界面服务：多用户认证授权、SSE 流式会话、用量统计 |
| `web` | React + Vite + Tailwind SPA（会话流、Agent/模型配置、用量、Traces） |
| `desktop` | Electron 桌面壳（内嵌 server，双击即用） |
| `docs` / `landing` | 文档站 / 落地页 |

**三接口边界**：LLMInterface / EnvironmentInterface / Agent State——三者的边界把 SDK、CLI、Server、Web 组织成一个系统。Agent State 含 `AGENTS.md`、Skills、配置、Benchmark 目录等，是自我进化的"基因组"。

---

## 4. 核心执行机制（agent-loop）

- **context_engine**：ReAct 循环（Reason → Act → Observe），每轮工具调用前**审批**（ApproveFn；策略禁止时返回 `Tool call denied by policy.`）。
- **OmniMessage 协议**：统一消息流（text / tool_call / tool_call_output / complete 等），web 端 SSE 直接渲染该协议。
- **Trace**：追加式 JSONL，完整记录每轮；**可恢复（resume）**——中断后可续跑。
- **中断与补发（carry-over）**：用户 signal 触发中断 → 产出 `abort` 事件；场景 A（模型输出已完成）：已完成的工具结果按结构化 `tool_call_output` 补发，未执行的补 `[interrupted: tool aborted by user]` 占位，保证 tool_call 与输出严格配对；场景 B（未完成）：整轮压平为 `[turn_aborted]` 用户文本。
- **自动重连**：≤ 5 次。
- **上下文压缩**：长会话自动 compact（CLI 有 `/compact` 命令）。

---

## 5. 插件系统（Skill + 钩子）

**插件** = 目录 + `plugin.json` 清单 + 携带内容。每个插件是独立 npm 包 `@penguinharness/<name>`。

```
plugins/<plugin>/
├── plugin.json                # 唯一元数据载体（描述/版本/分类/钩子命令）
├── icon.svg
├── skills/<name>/SKILL.md     # 零个或多个 Skill（reference/… 随行）
└── hooks/*.mjs                # 至多一个钩子包：纯 Node 脚本
```

- **Skill 渐进式加载**：系统 Prompt 只注入 `{{SKILL_METADATA}}`（name + description），模型在任务匹配时**先用 Shell 完整读 SKILL.md 再执行**；不设专门的 Skill 工具。
- **钩子点**：`stop` / `pre_tool_use` / `user_prompt`——harness 在循环固定点运行脚本（如 goal 模式的 start.mjs / stop.mjs）。
- 版本号 = `YYYY-MM-DD.N`（日期+当日序号），更新检查读已装 frontmatter 的 version。
- 对话中可用 `[use_skills]` 块显式指定 Skill。

**内置插件**：data-analysis / use-bento-slides / humanizer / goal / continual-learning / software-development / use-claude-code / agent-development / model-development / skill-porting / agent-tuning。

---

## 6. RSI 自进化闭环（self-improvement）

由 Skill 编排普通 Agent 机制完成——**评测是普通 Session，优化是普通文件编辑**。四个角色：

| 角色 | 职责 |
|---|---|
| **Builder** | 顶层 Agent：`agent-initialization` → `benchmark-design`（构建多 Case Benchmark） |
| **Target Agent** | 被改进的 Agent，只在隔离 Workspace 执行评测任务 |
| **Evaluator** | `run_subagent` 创建的叶子 Worker，执行并评分一次 Case 运行 |
| **Optimizer** | 新顶层 Agent：`agent-optimization` |

优化循环（Optimizer）：
1. `run_subagent` 并行编排 Evaluator，覆盖 Case × runs 矩阵；
2. 根据得分 + 关联 Trace 提出**有界 Candidate**；
3. 编辑 Target Agent 的可编辑状态——`AGENTS.md`、Skills、配置 → 版本 N+1；
4. **分数严格提升才保留，否则回滚**；
5. 达到期望提前结束，否则保留最高分 Reference。

工程保障：
- **Benchmark 存储** `benchmarks/<id>/`：`benchmark_config.toml` + 每 Case 的 `statement/`（题面）与 `rubric/`（**私有评分标准，对 Target Agent 隔离**）+ `scoreboard.yaml`。
- **快照与回滚**：每轮优化前把 Agent State 打包为 `snapshots/v<version>.tar.gz`（**Vault 密钥永不进快照**）；`system_config.yaml` 的 version 优化成功后自增；Web UI 可导出/导入快照。
- **全程可审计**：每个 Evaluator 运行都是普通 Session 留完整 Trace，scoreboard 通过 `session_id` 链接回这些 Session。**分数不是黑盒：任何数字都可回溯到产生它的那次运行。**

---

## 7. 示例代码证据（examples/）

| 示例 | 证明 |
|---|---|
| `build-agent-with-agent/build-agent.ts` | **AI 构建 AI**：用 `agent-initialization` skill 从自然语言需求生成全新 agent（commit-helper），再让它干活验证生成的 AGENTS.md 真的塑造了行为 |
| `self-improving-agent/self-improve.ts` | 最简**评分循环**（evaluate→edit→re-evaluate→keep/rollback），EDIT 步骤由脚本硬编码——刻意注明"这不算自进化" |
| `self-improving-agent/self-evolve.ts` | **真·自进化**：agent 自己诊断失败（空白 AGENTS.md 下稳定失分）、对比通过/未通过的两份报告、**自己推理出规范并写进自己的 AGENTS.md**，重测只保留分数提升的版本；脚本从不写规则，只给失败信号 + 一个正例 |
| `self-improving-agent/self-evolve-recursive.ts` | **递归自进化**：多轮构建。第 1 轮只见 1 个正例只能推断结构（稳定 ~7/10）；第 2 轮给多个共享同一常量标记的正例后，agent 读取自己的第 1 轮 AGENTS.md 并精化锁定常量 → 逼近满分。`state_{n+1} = agent.reflect(state_n, new_evidence)` |

示例全部可本地跑（Ollama 服务 qwen3.6:35b 即可，不依赖闭源模型）。

---

## 8. 安装与使用

```bash
# 一行安装（或 desktop app 双击）
curl -fsSL https://penguin.ooo/install.sh | sh
# 或 npm
npm install -g @prismshadow/penguin-cli   # 需 Node ≥ 24

penguin config model add --provider deepseek --model-id deepseek-v4-flash-vision-exp --api-key sk-... --set-default
penguin run -m "Create hello.txt containing Hello, Penguin"   # 单次任务
penguin chat       # 交互式 REPL（/compact /clear /exit、Ctrl-C 中断）
penguin web        # 启动服务并打开 http://127.0.0.1:7364（Web UI：会话/Agent/Skill/模型/用量/Trace/评测中心）
```

- 数据根：`~/.penguin/data`（Desktop 与 CLI 共用，可混用）
- 模型：1000+ 模型 / 100+ 供应商；DeepSeek V4 支持 DeepSeek/OpenRouter/Fireworks/SiliconFlow/TokenDance/Qwen Token Plan/Qwen PAYG；也支持 Kimi K3、GLM 5.3、Hunyuan 3、Qwen 3.8 Max 等
- SDK：`createAgent` → `createSession` → `run([userText(...)])`（异步生成器流式输出 OmniMessage）

---

## 9. 与 deepseek-harness / GDPevo 的关系

同属 **Prism-Shadow 组织**（成员：penguin-harness、GDPevo、awesome-rsi、deepseek-harness-book、learn-deepseek-harness、dsh-omni-desktop、travel-agent、agent-provider-verifier、penguin-extensions、penguin-harness-leaderboard、self-evolving-agent-daily）：

- **deepseek-harness**（DeepSeek 官方，本机已装）→ 更底层/研究向的 harness；penguin-harness 是它的**工程化产品化版本**：对 DeepSeek 等开放模型深度适配，同一批任务正面比拼 Claude Code，成本 1/70。
- **GDPevo**（⭐63, Python）→ "A Benchmark for Evaluating Agent Self-Evolution on Real Business Tasks"——**Agent 自进化的评测基准**，与 penguin-harness 的自进化引擎互补（一个提供引擎，一个提供度量）。
- **penguin-harness-leaderboard** → 自进化结果排行榜。

---

## 10. 总体评价

**亮点（值得借鉴的设计）：**
1. **RSI 落地为工程闭环**而非概念：Benchmark 存储格式、私有 rubric 隔离、快照回滚、scoreboard 全可审计——把"自我改进"做成了可复现、可回滚、可追溯的流水线。
2. **Skill 渐进式加载**：元数据进系统 Prompt、正文按需读取——省 Token 且不污染上下文。
3. **"评测是普通 Session，优化是普通文件编辑"**：不发明新机制，复用自身原语，架构极简。
4. **一切透明**：任何分数可回溯到一次具体运行（session_id → Trace）。
5. **真·自进化示例对"自进化"的诚实区分**：明确标注"脚本写死编辑=评分循环≠自进化"，方法论严谨。
6. 与 LlamaFactory 同作者，质量背书强；Apache-2.0 开源。

**注意点 / 风险：**
- 项目极新（2 个月），API 与存储格式仍在快速变化（CHANGELOG 高频更新，旧 scoreboard 不迁移）。
- Node ≥ 24 门槛较高；本地跑 RSI 对硬件/模型质量有要求（示例用 qwen3.6:35b）。
- 自我进化的**安全边界**：快照回滚机制已考虑（Vault 密钥隔离），但让 agent 改自己的 AGENTS.md 仍需人工把关（Builder/Optimizer 分两个独立 Session、用户确认步骤一后才进入步骤二）。

**与我们（GenericAgent）的关联**：我们的 goal_hive / goal_mode / skills 体系与它的 Skill+钩子、goal 插件思路同源；它的 self-improvement 闭环（benchmark→evaluate→optimize→snapshot）可作为我们自进化能力的参考蓝本。
