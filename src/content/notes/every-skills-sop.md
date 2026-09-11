---
title: "技能库总索引（2026-09-08）"
category: "技能库"
description: "本机全部技能库导航：10 个 Claude 插件 + 目录 skill（openspec/speckit/wiki 系）+ 统一架构说明。"
pubDate: "2026-09-08"
badge: "reference"
tags: ["skills", "index", "reference"]
---

# 技能库总索引（2026-09-08）

## 统一架构

- **唯一实体层**：`~/.agents/skills/`（197 个实体，0 转发链接）
- **各 Agent 链接层**：`~/.claude/skills`（99）· `~/.codex/skills`（109）· `~/.cursor/skills`（81）全部 symlink 指向 `.agents`
- **Claude 插件层**：10 个插件提供 104+ skills + 斜杠命令，优先于目录层（同名项已从目录删除，见下方各库）
- 新装技能：`npx skills add <源> -g -y`（装到 `.agents` 并自动建链接）

## Claude Code 插件（10 个）

| 插件 | 版本 | 提供 | 详细文档 |
|---|---|---|---|
| agent-skills@addy-agent-skills | 0.6.9 | 25 skills + 9 斜杠命令 + 4 人格（Google 工程实践） | [agent-skills-summary](/notes/agent-skills-summary) |
| mattpocock-skills@mattpocock | 1.2.3 | 25 skills（grill-me / to-spec / to-tickets / tdd 等） | [mattpocock-skills-summary](/notes/mattpocock-skills-summary) |
| understand-anything | 2.9.6 | 9 skills（代码库图谱分析 / 问答 / 仪表盘） | [understand-anything](/notes/understand-anything) |
| compound-engineering@compound-engineering-plugin | 2.39.0 | 37 skills + `/lfg` `/slfg` 命令（全自动化流水线） | [compound-engineer](/notes/compound-engineer) |
| compound-knowledge | 0.2.0 | 6 skills（kw-* 知识工作流） | — |
| superpowers@claude-plugins-official | 6.3.0 | 15+ skills（brainstorming / TDD / systematic-debugging 等） | [superpower](/notes/superpower) |
| code-review@claude-plugins-official | — | 代码审查 | — |
| code-simplifier@claude-plugins-official | 1.0.0 | 代码简化 | — |
| commit-commands@claude-plugins-official | — | Git 提交命令 | — |
| claude-md-management@claude-plugins-official | 1.0.0 | 文档管理 | — |

> 插件同名 skill 已从 `~/.claude/skills` 删除（2026-09-08），插件机制承担同名项，无重复加载。

## 目录 skill 库（.claude/skills 99 个，无插件版）

| 库 | 数量 | 说明 | 文档 |
|---|---|---|---|
| openspec 官方 | 16 | 规格驱动开发（new/propose/apply/verify/archive…） | [openspec-sop](/notes/openspec-sop) |
| speckit 中文 | 8 | 规格驱动开发中文版（plan/specify/tasks/implement…） | — |
| wiki 系 | 10 | wiki 摄入/查询/lint/进化 + llm-wiki | [llm-wiki](/notes/llm-wiki) |
| GA 自有核心 | ~35 | huashu-design / bento-slides / omc-* / academic-* / deep-* 等 | 对应各 note |
| 其他第三方 | ~30 | ultragoal / officecli / fanbox-agent / darwin-skill 等 | [ultrarag-summary](/notes/ultrarag-summary) 等 |

## 历史库去向（旧 every 索引，2026-08-10 → 2026-09-08）

| 旧库 | 现状 |
|---|---|
| superpowers (15) | ✅ 插件 superpowers@claude-plugins-official 6.3.0 |
| compound-engineering (40) | ✅ 插件 compound-engineering 2.39.0（ce-* 26 实体保留于 .agents） |
| compound-knowledge (6 kw-*) | ✅ 插件 compound-knowledge 0.2.0 |
| understand-anything (8) | ✅ 插件 understand-anything 2.9.6（9 skills） |
| open-slide (8) | ⚠️ 未单独安装（vercel-* / slide-* 不再单独提供） |
| coding-tutor (1) | ⚠️ 未单独安装 |
| llm-wiki (1) | ✅ .agents/skills/llm-wiki 实体（wiki 系） |

## 快速检索

- 需求→交付全链路: [build-chain](/notes/build-chain-sop) | grilling（需求追问）| to-spec → to-tickets → tdd
- 开发流程: superpowers/* | ce-brainstorm | ce-plan | ce-work | ce-code-review | /lfg
- 代码质量: ce-simplify-code | ce-optimize | systematic-debugging | code-review 插件
- 规格驱动: openspec-*（16）| speckit-*（8 中文）
- PR/Git: ce-commit | ce-commit-push-pr | ce-resolve-pr-feedback | ce-worktree | using-git-worktrees
- 知识管理: wiki 系 | llm-wiki | kw-*（compound-knowledge 插件）| understand-* | ce-slack-research
- 知识图谱: understand | understand-knowledge | understand-domain
- 前端设计: huashu-design | frontend-design | web-design-guidelines
- 测试: test-driven-development | ce-test-browser | ce-test-xcode
- 全自动化: /lfg（plan→work→review→test→commit→push→PR→CI→fix）
- 浏览器: huashu-chrome（真实 Chrome 带登录态）
