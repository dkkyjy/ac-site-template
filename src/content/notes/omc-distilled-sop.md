---
title: "OMC - 蒸馏笔记"
category: "多智能体编排"
description: "oh-my-claudecode Teams-first 多 Agent 编排层蒸馏与决策表。"
pubDate: "2026-08-10"
badge: "distilled"
tags: ["omc", "orchestration", "distilled"]
---

# OMC Distilled — oh-my-claudecode 蒸馏存档（编排能力提取）

## 定位
oh-my-claudecode (OMC v4.15.7, Yeachan-Heo，38k★) 是 Claude Code 的 Teams-first 多 Agent 编排层，与已蒸馏的 OMX(oh-my-codex) 同族但更强。定位与 GA 高度重叠（同为 agent 编排 harness）。本次按用户选择 **B：聚焦编排能力提取**——吸收其差异化编排方法论转 GA 可用，不重写 TS 实现、不实跑 SWE-bench。

## 蒸馏源
GitHub: `Yeachan-Heo/oh-my-claudecode` (npm: oh-my-claude-sisyphus)

## 归档位置（对齐 OMX 形式：轻量源码，跳 dist/tests/node_modules）
`memory/OMC/oh-my-claudecode/`（16.7MB / 970 文件）
含 agents/(19)、commands/(28)、skills/(42)、docs/(29)、src/(测试已剔)、hooks/scripts/templates/bridge/benchmark/benchmarks/geobench/tests-顶层源码。临时源 temp/omc_meta 会被沙箱清空，需时从本归档取回。

## 核心组件决策表（调用/重写/舍弃）
| 组件 | 决策 | 理由 |
|------|------|------|
| 19 专家 Agent 角色契约(agents/*.md) | **调用-规范化** | Markdown 可读；其"职责/非职责"分离与模型分层建模可直接提炼为 GA 角色方法论 |
| autopilot 管线状态机(TS) | **重写方法** | 深度绑定 Claude Code hooks/tmux/TS，GA 不复刻代码；提炼阶段机设计模式 |
| team/tmux 并行执行 | **舍弃** | GA 已有 goal_hive_sop 多 worker BBS 调度，无需重造 |
| 治理 hooks(生命周期) | **观察** | 概念映射 GA 的事件感，不重写 TS |
| State 管理(.omc/ 档案) | **观察增强** | GA 已有 L1/L2/L3 记忆体系；借鉴其 artifact-descriptor/边界移交思路 |
| SWE-bench harness(benchmark/) | **舍弃** | 需 Claude API+docker，超出 GA 复用范围 |
| MCP/hud/bridge 等外围 | **舍弃** | 平台绑定重，不搬运 |

## 提炼出的编排方法论（GA 可直接借鉴）

### 1. Agent 角色契约：「职责/非职责」双向约束
每个角色 = frontmatter(name/description/model/level/disallowedTools) + `<Role>/<Mission>/<Responsible::>/<Not_Responsible::>`。
**关键点在显式"不负责清单"**——防止 executor 顺手去 review、architect 抢划 spec。GA 在做 checklist/subagent 分解时应为每个角色强制写清"非职责边界"。
19 agents 分 4 lanes：
- **构建/分析 lane**：explore(haiku)→analyst(opus)→planner(opus)→architect(opus)→debugger(sonnet)→executor(sonnet)→verifier(sonnet)→tracer(sonnet)
- **评审 lane**：security-reviewer, code-reviewer（最终质量闸门）
- **领域 lane**：test-engineer/designer/writer/qa-tester/scientist/git-master/document-specialist/code-simplifier
- **协调 lane**：critic(opus)——专门挑战他人计划，无缝隙才放行
典型流水线：`explore→analyst→planner→critic→executor→verifier`。

### 2. 模型分层路由（成本/质量分档）
按角色定档：explore/writer=haiku(快), executor/debugger/test=sonnet(平衡), architect/planner/critic/code-reviewer=opus(复分)。委托时 Enforcer 自动注入模型，显式指定则保留。

### 3. Autopilot 阶段式管线状态机
可配置序列 `RALPLAN→EXECUTION→RALPH→QA`，每段可选/可跳，状态未来穿越压缩保留。默认 execution=solo，验算 engine=ralph。
- **ralplan**：Planner+Architect+Critic 分歧迭代直至共识（计划闸门）。
- **execution**：solo 或 team(并行多个 worker)。
- **ralph**：不停循环，verifier 基于新鲜证据确认完成才退出（maxIterations，如 100）。
- **qa**：build/lint/test 回环。
阶段间以**显式阶段信号 + 事务性 transition + 证据边界**推进：每完成一个阶段的 evidence 落盘，任务描述/结果的大 payload 用 descriptor(口参 {kind,path,creator,sizeBytes,retention}) + 摘要去引用而非内联（边界移交）。

### 4. 技能三层公式
`[执行层 skill] + [0..N 增强层] + [可选保证层]`。如 `ultrawork(并行) + default(构建) + git-master(提交)`；`ralph(保证):不能停，直到近似完成`。可配置 magic keyword 自动触发 (如 autopilot/ultrawork/tdd/ccg)。

### 5. State 持久化模式
compile-resistant notepad(.omc/notepad.md, PreCompact 时段存/后重新注入) + project-memory.json(跨 session) + 每个执行计划的独立随记夹(learnings/decisions/issues/problems)。QA evidence 要求**fresh(≤5min) 真实命令输出**，非断言。

## 与 GA 现有能力映射
- 角色边界方法论 → 增强 GA 的 checklist/subagent role 定义（**新价值**）
- autopilot 验证闸门/阶段状态机 → 增强 GA 的 goal_mode/自主/持续优化流程
- skill 三层公式 → GA 有同类 skill/SOP 组合；公式可借鉴
- control/data plane 分离+descriptor → GA memory L1/L2/L3 已实现同思想，验证增强
- team 并行 → goal_hive 已有，不重写
- 模型分层 → GA 单 LLM 无显式分档，仅归档方法论

## 结论
omc 与 GA 定位重叠大。策略同 omx：**排除/吸收核心方法论为主，不重写 TS**。GA 首要受益点：①严格的"非职责"角色契约 ②证据新鲜+验证循环的完成闸门 ③阶段化管线状态机 + descriptor 边界移交。需要时 read 蒸馏原码 `temp/omc_meta/oh-my-claudecode-main/`（需重新拉取，环境沙箱可能清空）。