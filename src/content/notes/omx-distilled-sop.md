---
title: "OMX - 蒸馏笔记"
category: "OMX / Codex 工作流"
description: "oh-my-codex 多 Agent 编排层蒸馏存档：结构、接入方式。"
pubDate: "2026-08-10"
badge: "distilled"
tags: ["omx", "orchestration", "distilled"]
---

# OMX Distilled — oh-my-codex 蒸馏存档

## 定位
OMX (oh-my-codex v0.18.15) 是 OpenAI Codex CLI 的多 Agent 编排层。本 SOP 记录其蒸馏产物的位置、结构和接入方式。

## 蒸馏范围（轻量版）
- **skills/**: 本地技能文件（50+ skills，每项含 SKILL.md）
- **oh-my-codex/**: npm 包源码（跳过 node_modules/ 和 dist/）
  - src/ — TypeScript 源码
  - crates/ — Rust 源码（omx-runtime, omx-mux, omx-sparkshell 等）
  - prompts/ — 提示词模板
  - plugins/ — 插件定义
  - templates/ — 模板
  - .agents/ — Agent 定义
  - package.json, README.md, Cargo.toml 等元数据

## 路径
- `memory/OMX/skills/` — 本地技能（~/.codex/skills/ 的副本）
- `memory/OMX/oh-my-codex/` — npm 包源码

## Skills 概览

### 本地独有的技能（17个，来自 every 等第三方插件）
```
api-and-interface-design, browser-testing-with-devtools, ci-cd-and-automation,
code-review-and-quality, code-simplification, compound-engineering-plugin,
compound-knowledge-plugin, context-engineering, debugging-and-error-recovery,
deprecation-and-migration, documentation-and-adrs, doubt-driven-development,
frontend-ui-engineering, git-workflow-and-versioning, idea-refine,
incremental-implementation, llm-wiki, performance-optimization,
planning-and-task-breakdown, security-and-hardening, shipping-and-launch,
skills, source-driven-development, spec-driven-development, superpowers,
test-driven-development, understand, understand-anything-plugin,
understand-chat, understand-dashboard, understand-diff, understand-domain,
understand-explain, understand-knowledge, understand-onboard,
using-agent-skills
```

### npm 包自带的独有技能（17个，新版本新增）
```
ask-claude, ask-gemini, build-fix, deepsearch, ecomode,
frontend-ui-ux, git-master, help, note, ralph-init, review,
security-review, swarm, tdd, trace, visual-verdict, web-clone
```

### 共享技能（29个）
```
ai-slop-cleaner, analyze, ask, autopilot, autoresearch, autoresearch-goal,
best-practice-research, cancel, code-review, configure-notifications,
deep-interview, design, doctor, hud, omx-setup, performance-goal, pipeline,
plan, prometheus-strict, ralph, ralplan, skill, team, ultragoal, ultraqa,
ultrawork, visual-ralph, wiki, worker
```

## Skill 文件格式
每个 skill 目录包含 `SKILL.md`（YAML frontmatter + Markdown）：
```yaml
---
name: <skill-name>
description: "[OMX] <简要描述>"
---
<Purpose>...</Purpose>
<Use_When>...</Use_When>
<Do_Not_Use_When>...</Do_Not_Use_When>
<Execution_Policy>...</Execution_Policy>
```
部分 skill 含 README.md / .system/ 子目录。

## 接入方式
1. **直接调用**：通过 `omx` 命令使用（需安装 oh-my-codex npm 包）
2. **SOP 重写**：将特定 skill 的核心逻辑重构为本系统的 L3 SOP（参考 morphling_sop）
3. **能力索引**：L1 索引中引用需要的 skill（本 SOP 作为桥梁）

---

## Morphling 分析记录 (2026-06-26)

**目标**: https://github.com/Yeachan-Heo/oh-my-codex
**版本**: v0.18.15 (npm全局安装)
**语言**: TypeScript 93.6%, Rust 3.9%, JavaScript 2.0%
**架构**: 38个src模块 + 51个skills + Rust crates (蒸馏存档: 335文件, ~4.5MB)

### 核心架构组件
| 层级 | 组件 | 说明 |
|------|------|------|
| CLI | src/cli/, src/compat/ | OMX CLI入口，Codex兼容层 |
| 编排 | src/autopilot/, src/pipeline/, src/planning/, src/ultragoal/, src/ralplan/ | 多Agent编排核心 |
| Agent | src/agents/, src/subagents/, src/team/ | 子Agent系统，原生Native Agent |
| Skills | skills/ (51个) | Codex CLI skill文件 |
| 运行时 | src/exec/, src/runtime/, src/hooks/, src/state/, src/sidecar/ | 执行引擎 |
| Rust | crates/ | omx-runtime, omx-mux等原生模块 |
| 视觉 | src/visual/, src/imagegen/ | 视觉Ralph循环 |

### 组件决策
| 组件 | 决策 | 理由 |
|------|------|------|
| Skills (Markdown) | **调用** | 可直接被Codex CLI读取，格式与GA SOP不兼容 |
| 编排引擎 | **舍弃** | 深度绑定Codex CLI/tmux/TS，无法在GA复用 |
| Agent系统 | **观察** | "原生Native Agent"概念与GA subagent类似 |
| Rust模块 | **舍弃** | 语言不兼容 |
| 整体源码 | **保留蒸馏存档** | 架构参考之用 |

### 结论
omx是Agent Harness，与GA系统定位重叠但架构差异大。策略：保留存档作架构参考，不重写；按需提取个别skill逻辑为L3 SOP。