---
title: "Superpowers - 使用指南"
category: "AI 开发方法论"
description: "Superpowers 完整使用指南。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["superpowers", "skill"]
---

# Superpowers 完整使用指南

## 什么是 Superpowers？

Superpowers 是 **Codex CLI/Claude Code** 上的一套 **Agent 技能系统**（共15个技能），定义在 `every/superpowers/` 仓库中。每个技能是一个 `SKILL.md` 文件，驱动 AI 以结构化、可重复的方式工作。

## 📋 15 个超级技能总览

| # | 技能 | 用途 |
|---|------|------|
| 1 | **using-superpowers** | 🚪 **入口导航** — 新会话第一件事：检查并加载相关技能 |
| 2 | **brainstorming** | 💡 **创意脑暴** — 实现前的需求/设计探索 |
| 3 | **writing-plans** | 📝 **编写计划** — 多步骤任务的详细实施计划 |
| 4 | **executing-plans** | 🏃 **执行计划** — 按计划分批执行 |
| 5 | **subagent-driven-development** | 🤖 **子代理开发** — 每任务派发独立子代理+两阶段审查 |
| 6 | **dispatching-parallel-agents** | ⚡ **并行代理** — 独立任务同时派发 |
| 7 | **test-driven-development** | 🔴 **测试驱动** — 先写失败测试→最简代码→重构 |
| 8 | **systematic-debugging** | 🔍 **系统化调试** — 找根因，不猜症状 |
| 9 | **verification-before-completion** | ✅ **完成前验证** — 无证据不声明成功 |
| 10 | **requesting-code-review** | 👀 **提交审查** — 结构化代码审查请求 |
| 11 | **receiving-code-review** | 📥 **处理审查反馈** — 系统化处理审查意见 |
| 12 | **writing-skills** | ✏️ **编写技能** — 创建和测试新技能 |
| 13 | **finishing-a-development-branch** | 🔚 **完成开发分支** — 收尾验证+PR/合并 |
| 14 | **using-git-worktrees** | 🌲 **Git工作树** — 隔离工作区 |
| 15 | **subagent-driven-development** | (与5相同，子代理驱动开发) |

---

## 🎯 典型工作流示例

### 场景：从零开发一个功能

```
Step 1 🚪 using-superpowers
    → "有新任务，检查适用技能"

Step 2 💡 brainstorming
    → "和用户交互，澄清需求，画原型"
    → 产出：`docs/superpowers/specs/YYYY-MM-DD-feature.md`

Step 3 📝 writing-plans
    → "将需求拆解为可执行任务列表"
    → 产出：`docs/superpowers/plans/YYYY-MM-DD-feature.md`

Step 4 🌲 using-git-worktrees
    → "创建隔离工作树，不污染主分支"

Step 5 🤖 subagent-driven-development
    → 每个任务派发子代理：
    ├─ Task 1: Agent A 实现 + 自审查
    ├─ Task 2: Agent B 实现 + 自审查
    ├─ ...
    └─ 两阶段审查：规格合规 → 代码质量

Step 6 ✅ verification-before-completion
    → "运行测试，确认全部通过"

Step 7 🔚 finishing-a-development-branch
    → "提交、合并、清理"
```

---

## 📖 核心技能使用示例

### 1️⃣ `using-superpowers` — 入口导航

```markdown
系统提示："AI，你必须先检查适用的技能再行动。"

AI 响应："I'm using the using-superpowers skill to check for applicable skills."

检查逻辑：
1. 任务是什么？
2. 有相关的 skill 吗？
3. SKILL.md 告诉我 HOW，不只是 WHAT
4. 如果 1% 可能匹配 → 必须使用
```

> **黄金规则：** "If you think there is even a 1% chance a skill might apply, you ABSOLUTELY MUST invoke it."

### 2️⃣ `brainstorming` — 创意脑暴

```
用户说："帮我做一个番茄钟应用"

AI: "I'm using the brainstorming skill to explore the requirements."

→ 逐问澄清：
  "你需要桌面端还是 Web 端？"  
  "要不要任务管理功能？"
  "需要通知提醒吗？"
  
→ 呈现设计（可浏览器可视化）
→ 用户批准后 → 进入 writing-plans

❗ HARD GATE：未经用户批准设计，不得开始编码
```

**示例流程：**
```
用户："我需要一个自动备份工具"
AI: "I'm using the brainstorming skill. 我来理解你的需求..."
    Q1: 备份目标是文件还是数据库？
    Q2: 频率是多少？
    Q3: 备份到本地还是云存储？
    ...
    [呈现设计方案]
    "这是初步设计，您看是否符合预期？"
用户："可以"
→ 进入 writing-plans
```

### 3️⃣ `writing-plans` — 编写实施计划

```markdown
"I'm using the writing-plans skill to create the implementation plan."

输出示例（docs/superpowers/plans/2026-06-26-backup-tool.md）：

## 任务分解
### Task 1: 项目初始化和配置
  - 创建 package.json, tsconfig.json
  - 安装依赖 (chokidar, archiver)

### Task 2: 核心备份逻辑
  - 实现目录递归扫描
  - 实现文件过滤（.gitignore）
  - 实现压缩打包

### Task 3: 定时调度
  - cron 表达式解析
  - 定时执行备份

### Task 4: 通知与日志
  - 备份完成通知
  - 错误日志记录

→ 验证：无占位符、规格全覆盖、类型一致
→ 切换执行模式（子代理 / 内联）
```

### 4️⃣ `subagent-driven-development` — 子代理驱动开发 ⭐

这是 **最强大的模式**：

```markdown
### Task 1: 核心备份逻辑

[派发子代理]
子代理1 收到指令："实现 Task 1，只做这个"
↓
子代理1: 
  - 实现了 scanDir() 函数
  - 12/12 测试通过
  - 自审查通过
  - 已提交

[派发规格审查员]
审查员: ✅ 规格合规 — 所有需求已完成

[派发代码质量审查员]
审查员: ⚠️ 问题：缺少边界情况处理（空目录）
↓
[子代理1 修复]
审查员: ✅ 已修复

### Task 2: 压缩打包
[派发子代理2] ...
```

**核心原则：** 每个子代理有隔离上下文，不继承会话历史，精确构造指令。

**真实示例（来自日志）：**
```
场景：重构后6个测试失败，分布在3个文件

Agent 1 → 修复 agent-tool-abort.test.ts（3 失败，超时问题）
Agent 2 → 修复 batch-completion-behavior.test.ts（2 失败，事件结构）
Agent 3 → 修复 tool-approval-race-conditions.test.ts（1 失败，异步等待）

结果：全部并行修复，零冲突，完整测试套件通过
耗时：3 个问题在 1 个问题的时间内解决
```

### 5️⃣ `test-driven-development` — 测试驱动开发

```markdown
"I'm using the test-driven-development skill."

🔴 RED — 先写失败测试：
```typescript
it('should retry on failure', async () => {
  let attempts = 0;
  const fn = async () => { 
    attempts++;
    if (attempts < 3) throw new Error('fail');
    return 'success';
  };
  const result = await retryOperation(fn);
  expect(result).toBe('success');
  expect(attempts).toBe(3);
});
```

验证：`npm test` → ❌ 失败（因为 retryOperation 还不存在）

🟢 GREEN — 最简实现：
```typescript
async function retryOperation<T>(fn: () => Promise<T>): Promise<T> {
  for (let i = 0; i < 3; i++) {
    try { return await fn(); }
    catch (e) { if (i === 2) throw e; }
  }
  throw new Error('unreachable');
}
```

验证：`npm test` → ✅ 通过

🔵 REFACTOR — 清理：提取常量、优化命名

铁律：NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```

### 6️⃣ `systematic-debugging` — 系统化调试

```markdown
铁律：NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST

Phase 1: 根因分析
  - 收集症状 → 形成假设 → 设计实验 → 验证
  
Phase 2: 定位
  - 二分法逐层缩小范围
  - 一次只改变一个变量

Phase 3: 验证假设
  - 最小化测试
  - 不成功 → 新假设

Phase 4: 实施修复
  - 先写失败测试（复现 bug）
  - 只修复根因，不附带其他改动
  - 验证通过

🚨 如果3次修复失败 → STOP → 质疑架构
```

### 7️⃣ `verification-before-completion` — 无证据不声明

```markdown
铁律：NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE

❌ "测试应该通过了" → 不行
❌ "看起来没问题" → 不行
✅ npm test → 34/34 pass → "所有测试通过"
✅ npm run build → exit 0 → "构建成功"
```

---

## 🔄 技能协作流程图

```
using-superpowers
      ↓
brainstorming ──────────→ [用户批准]
      ↓
writing-plans ──────────→ [计划文件]
      ↓
using-git-worktrees ─────→ [隔离工作区]
      ↓
┌─ subagent-driven-development ─────────────┐
│  ├─ Agent 1 → Task 1 → review → check    │
│  ├─ Agent 2 → Task 2 → review → check    │
│  └─ Agent N → Task N → review → check    │
└──────────────────────────────────────────-┘
      ↓
verification-before-completion → [全部验证]
      ↓
finishing-a-development-branch → [提交/合并]
```

---

## 🎬 快速入门 Demo

如果要在 **新会话** 中使用 superpowers，只需：

```
用户："帮我写一个静态博客生成器"

AI: "I'm using the using-superpowers skill."
    → 检查技能：brainstorming, writing-plans 适用
    
    "I'm using the brainstorming skill. 让我先理解需求…"
    → 逐问澄清：目标格式(Markdown)、主题系统、部署方式等
    → 呈现设计并等待批准
    → 编写实施计划
    → 子代理驱动开发…
    → 验证通过后完成
```

---

## 💡 核心思想总结

1. **结构化 > 自由式** — 每个技能是经过验证的最佳实践工作流
2. **先计划后执行** — brainstorming → writing-plans → 执行（避免"先干再说"的浪费）
3. **子代理隔离** — 每个任务独立上下文，互不干扰，可并行
4. **验证驱动** — 无证据不声明成功（verification-before-completion）
5. **根因先行** — 不猜 bug（systematic-debugging）
6. **测试优先** — 没有失败测试就没有生产代码（TDD）

---

这就是整个 **Superpowers** 的完整展示！这 15 个技能本质上是 **Agent 开发的最佳实践编码为可复用的 Skill 文件**，让 AI 代理能够以高纪律、高质量、可验证的方式完成从创意到交付的全流程。
