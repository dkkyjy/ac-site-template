---
title: "Compound Engineering 使用示例大全"
description: "Compound Engineering 使用示例大全。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["compound", "工程"]
---

# Compound Engineering 使用示例大全

## 核心哲学

> **每个工程任务应当让下一个任务更简单，而不是更难。**

80% 在规划和审查，20% 在执行。每个任务都产出知识，让后续工作越来越快。

---

## 安装

```bash
# Claude Code
/plugin marketplace add EveryInc/compound-engineering-plugin
/plugin install compound-engineering

# Cursor
/add-plugin compound-engineering

# Codex (3步)
codex plugin marketplace add EveryInc/compound-engineering-plugin
bunx @every-env/compound-plugin install compound-engineering --to codex
# 然后在Codex内 /plugins → 安装Compound Engineering
```

安装后先运行：
```text
/ce-setup
```

---

## 1. 核心工作流（完整链条）

Compound Engineering 的推荐流程是四个步骤的链条：

```
/ce-ideate → /ce-brainstorm → /ce-plan → /ce-work
```

### 场景示例：开发"用户通知中心"功能

#### ① `/ce-strategy` — 战略锚点（可选，在最上游）

```text
/ce-strategy
```

创建/更新 `STRATEGY.md`：定义产品的目标问题、方法、用户画像、关键指标。后续 `/ce-ideate`、`/ce-brainstorm`、`/ce-plan` 都会读取它作为上下文。

#### ② `/ce-ideate` — 构思探索（可选）

```text
/ce-ideate "我们想做一个团队通知中心，有什么好思路？"
```

生成和批判性评估多个大方向，选出最强的那个输出给 brainstorm。产出：排序后的构思文档。

#### ③ `/ce-brainstorm` — 需求定义

```text
/ce-brainstorm "设计用户通知中心"
```

逐轮提问互动，压力测试前提假设，探索 2-3 种方案后推荐一个，产出包含：

| 标识符 | 含义 |
|--------|------|
| `R-IDs` | 需求编号 |
| `A-IDs` | 角色编号 |
| `F-IDs` | 核心流程编号 |
| `AE-IDs` | 验收示例编号 |

**关键特性**：Brainstorm 不写实现方案（HOW），只写行为需求（WHAT）。最终输出三段式 Summary：
- **Stated**（明确决定）
- **Inferred**（Agent 的推断假设）
- **Out**（已排除的范围）

#### ④ `/ce-plan` — 规划

```text
/ce-plan "执行通知中心的规划"
```

产出 **决策文档 + 执行护栏**，包含：

| 组件 | 说明 |
|------|------|
| `U-IDs` | 稳定标识符（跨分支、跨 session 不变） |
| 范围边界 | 什么在内 / 在外 |
| 原子工作单元 | 每个单元的依赖顺序 |
| 文件映射 | 每个单元修改哪些文件 |
| 测试场景 | 必须通过的测试 |
| 风险缓解 | 已知风险和应对 |

`ce-plan` **不写** 具体代码、API 签名或 shell 命令序列——这些是 implementer 的事。

自动派发最多 **5 个研究 Agent** 并行调查：
- 仓库分析（技术栈、架构、模式）
- 机构记忆（`docs/solutions/` 历史方案）
- 框架文档（版本锁定）
- 最佳实践（高风险主题）
- Spec 流分析（边界用例）

同样的引擎也适用于非软件场景（研究计划、旅行规划、热水器维护等）。

#### ⑤ `/ce-work` — 执行

```text
/ce-work "实现通知中心"
```

**关键原则**：Plan 决定了 WHAT，`ce-work` 自己决定 HOW。

自动执行以下流程：

| 步骤 | 说明 |
|------|------|
| 🧠 **读取计划** | 解析 U-IDs、文件映射、测试要求 |
| 🔍 **并行安全检查** | 检测文件冲突 → 无冲突则在独立 worktree 并行执行 |
| 🚀 **派发子 Agent** | 每个单元在独立分支上工作 |
| 🔄 **合并** | 按依赖顺序合并，每步跑测试 |
| ✅ **幂等性检查** | 已经完成的工作标记为 done，不重复执行 |
| 🧪 **质量门** | 自动运行测试套件 |
| 📝 **Commit + PR** | 自动生成 PR 摘要、测试说明、操作手册 |

```text
# 小改动可以简化（不强制完整链）
/ce-work "把按钮颜色从蓝色改成绿色"
```
小改动直接进入实现；中等工作建任务列表；大/敏感工作建议先走 `/ce-brainstorm` 或 `/ce-plan`。

---

#### ⑥ `/ce-code-review` — 代码审查

```text
/ce-code-review          # 审查当前分支/PR
/ce-code-review quick    # 快速审查（跳过多agent流水线）
```

核心机制：

| 特性 | 说明 |
|------|------|
| **Tiered Persona** | 50+ 专业审查 Agent，根据 diff 内容自动选择 |
| **并行派发** | 多个审查 Agent 同时运行 |
| **置信度门控** | 跨 Agent 交叉验证提升/降级发现 |
| **去重合并** | 同一个问题被多个 Agent 发现 → 提升优先级 |
| **Autofix** | `safe_auto` 级别的修复自动应用 |

审查 Agent 类型（部分）：
- 正确性审查、性能审查、安全审查、可靠性审查
- Ruby on Rails 专家、Python 专家、TypeScript 专家
- Swift/iOS 审查、Schema 漂移检测、前端竞态条件审查
- 对抗性审查（模拟破坏场景）

#### ⑦ `/ce-debug` — 调试

```text
/ce-debug "用户登录后偶尔看到空白页"
```

系统化的根因分析流程：
1. 追踪因果链条
2. 形成可测试的假设
3. 测试优先修复

#### ⑧ `/ce-compound` — 知识沉淀（闭环）

```text
# 解决问题后自动触发（说了"搞定了"、"修复了"等）
/ce-compound

# 或手动调用
/ce-compound "记录N+1查询的修复过程"
```

产出文件到 `docs/solutions/[category]/[filename].md`：

```markdown
---
title: "修复通知中心的 N+1 查询"
category: performance
date: 2026-06-26
tags: [rails, activerecord, n+1]
---

## 症状
用户通知列表加载超过5秒

## 根因
User -> Notification 循环查询，每条通知独立查用户

## 失败尝试
- 批量预加载 (`includes`) 被旧查询覆盖
- raw SQL 方案不可维护

## 解决方案
在 User 模型上新增 `has_many :notifications` 关联 + `includes(:user)`
```

可选专业 Agent 后审：
- `ce-performance-oracle` → 性能问题
- `ce-security-sentinel` → 安全问题
- `ce-data-integrity-guardian` → 数据库问题
- `ce-code-simplicity-reviewer` → 代码简洁性（始终执行）

---

## 2. 其他常用技能

| 技能 | 用法 | 说明 |
|------|------|------|
| **`/ce-optimize`** | `/ce-optimize "优化渲染性能"` | 迭代优化循环：并行实验 + 测量门控 + LLM评判 |
| **`/ce-compound-refresh`** | `/ce-compound-refresh` | 刷新过时/漂移的学习文档，决定保留/更新/归档 |
| **`/ce-product-pulse`** | `/ce-product-pulse --since 7d` | 生成产品脉搏报告：用量、性能、错误、跟进 |
| **`/ce-sessions`** | `/ce-sessions "之前讨论过这个吗？"` | 跨 Claude Code / Codex / Cursor 搜索历史会话 |
| **`/ce-slack-research`** | `/ce-slack-research "关于通知的决策"` | 搜索 Slack 组织结构上下文 |
| **`/ce-ideate`** | `/ce-ideate "什么值得探索？"` | 生成并批判性评估多种方案 |
| **`/ce-resolve-pr-feedback`** | `/ce-resolve-pr-feedback` | 处理 PR 评论意见 |
| **`/ce-simplify-code`** | `/ce-simplify-code` | 简化复杂代码 |

---

## 3. 完整场景串联示例

```text
# 1. 探索方向
/ce-ideate "我们想做更好的用户通知体验"

# 2. 定义需求
/ce-brainstorm "设计用户通知偏好设置"

# 3. 制定规划
/ce-plan "实现通知偏好设置"

# 4. 执行
/ce-work "构建通知偏好设置"

# 5. 审查
/ce-code-review

# 6. 沉淀知识
/ce-compound (自动触发于"搞定了")
```

---

## 4. 50+ 审查 Agent 列表（精选）

| 类别 | Agents |
|------|--------|
| **正确性** | `ce-correctness-reviewer`, `ce-adversarial-reviewer`, `ce-api-contract-reviewer` |
| **性能** | `ce-performance-reviewer`, `ce-performance-oracle`, `ce-reliability-reviewer` |
| **安全** | `ce-security-reviewer`, `ce-security-sentinel`, `ce-security-lens-reviewer` |
| **语言专家** | `ce-kieran-rails-reviewer`, `ce-kieran-python-reviewer`, `ce-kieran-typescript-reviewer`, `ce-swift-ios-reviewer` |
| **设计** | `ce-design-iterator`, `ce-design-implementation-reviewer`, `ce-figma-design-sync` |
| **文档** | `ce-coherence-reviewer`, `ce-ankane-readme-writer` |
| **项目** | `ce-scope-guardian-reviewer`, `ce-product-lens-reviewer`, `ce-feasibility-reviewer` |
| **数据** | `ce-data-integrity-guardian`, `ce-data-migration-expert`, `ce-schema-drift-detector` |

---

## 心智模型

> **Compound Engineering = 每次完成任务都留下知识**

```
/ce-ideate ──→ /ce-brainstorm ──→ /ce-plan ──→ /ce-work ──→ /ce-compound
    ↑                                                              │
    └────────────────────── 下次更快 ──────────────────────────────┘
```
