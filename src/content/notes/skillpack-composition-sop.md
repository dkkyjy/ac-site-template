---
title: "Skillpack - 四包编排"
category: "专项工具 / 科研"
description: "grill/openspec/superpowers/compound-engineering 四包有机编排。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["composition", "skills", "workflow"]
---

# skillpack_composition_sop — grill / openspec / superpowers / compound-engineering 四包有机编排

> 来源：本机 `~/.claude/skills` 四包 + 真实重构实测（temp/wtcp hooks.py round，2026-08-08）。与 build_chain_sop(全链路) 互补：本 SOP 管**多包共存时的角色分配与接缝**。

## 0. 四包身份（职责唯一，防重叠）
| 包 | 唯一身份 | 独有工件 | 角色 |
|---|---|---|---|
| grill 家族 | 对话式逼出非显性决策 | 澄清+（可选 CONTEXT/ADR） | 前置澄清师 |
| openspec | 长期增量真值源 | `specs/*` + project/tasks | 规格固化 |
| superpowers | 自主执行纪律堆栈 | writing-plans / subagent-driven / TDD / verification | 执行引擎 |
| compound-engineering | 主工作流 + **知识复利** | ce-* 六步环 + `docs/solutions` learnings | 总主干 + 复盘 |

## 1. 触发选择（哪条路）
- 大而自主长跑 → superpowers `subagent-driven-development`
- 日常迭代+要知识户 → compound ce-* 环（默认）
- 小修单任务 → 单 step 快过，别跑全套

## 2. 流水线（compound 为主干）
```
ce-ideate→ce-strategy → grill澄清 → openspec spec-delta →
plan(ce-plan/superpowers writing-plans) → 执行(TDD，单 context) →
双轴 review(Standards+Spec) → ce-compound 落 docs/solutions → 回灌下一轮
```

## 3. 接缝纪律（实测踩坑）
- **dirty 工作区先隔离**：`git worktree` 或复制副本到 temp/，重构别在原处改。
- **契约术语三方一致**：spec / docstring / 测试断言共用同一稳定英文键（如 `overwrite-not-merge`），中文概论易漂移——实测由此抓出。
- **写代码前先锁行为**：现有模块重构前先写"行为基线测试"钉死真实语义（别信注释）；`trigger` 类折叠可能实际是 overwrite-not-merge。
- **monkeypatch 只打属性不改模块名**：`mod.load=lambda...`+用后还原，别 `mod=lambda`。
- **目录与文档唯一归属**：openspec 管 specs/，grill 管 ADR/词表，ce-compound 管 docs/solutions，实现计划不长期存。

## 4. 唯一入口路由（CLAUDE.md 顶部）
```markdown
# 主路由
主工作流：compound-engineering ce 六步环。
工具插槽：grill 任澄清；openspec 任规格；superpowers 任计划+执行纪律。
禁止 grill⇄ce-brainstorm、openspec-pr⇄ce-plan 同目标并发；无凭据不造需求。
```

## 坑位汇总
常量覆盖 hook 语义 / 契约术语漂移 / dirty 工作区污染 / docs 实体归属重叠。