---
title: "OMC - 多智能体编排演示"
description: "Oh-My-ClaudeCode (OMC) 多智能体编排演讲资料（中译版）。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["omc", "多智能体", "演示"]
---

# Oh-My-ClaudeCode

**面向自主开发的多智能体编排**

---

# 🎭 让我们从现场演示开始

**你告诉我想构建什么，我用 10 分钟把它做出来。**

你需要什么?
- 待办应用?
- 天气仪表盘?
- 实时投票?
- 小游戏?

*把你的想法发到弹幕/聊天里吧！*

---

# oh-my-claudecode：为 Claude Code 打造的多智能体编排

## 零学习曲线。最强的能力。

**[演讲者姓名]**

版本号 3.6.3

---

## 议程

| 时间 | 主题 |
|------|-------|
| 0:00 | 什么是 OMC？ |
| 0:10 | 五大核心执行模式 |
| 0:30 | Agent 系统 |
| 0:40 | 现场演示场景 |
| 0:48 | 开发者体验 |
| 0:54 | 上手入门 |
| 0:58 | 问答环节 |

注：这是一场 60 分钟的研讨会，覆盖完整的 oh-my-claudecode 系统。我们将聚焦于实际使用模式。

---

## 痛点问题

**今天开发人员面临的困境：**

- 手动协调复杂多步任务 <!-- .element: class="fragment" -->
- 在不同关注点之间频繁切换上下文 <!-- .element: class="fragment" -->
- 单线程的 AI 交互无法扩展 <!-- .element: class="fragment" -->
- 无持久性——任务一难，AI 就放弃 <!-- .element: class="fragment" -->
- 令牌浪费——用昂贵的模型处理简单任务 <!-- .element: class="fragment" -->

注：这些都是我用 Claude Code 构建生产应用时遇到的实际问题。OMC 诞生于对手动编排 AI 辅助开发的不满。

---

<!-- .slide: data-background="#1a1a2e" -->

# 第一部分
## 什么是 OMC？

---

## 什么是 oh-my-claudecode？

**一个为 Claude Code 打造的多智能体编排系统**

```
                    +------------------+
                    |     You (User)   |
                    +--------+---------+
                             |
                             v
                    +------------------+
                    |  Claude (Conductor)  |
                    +--------+---------+
                             |
              +--------------+--------------+
              |              |              |
              v              v              v
        +---------+    +---------+    +---------+
        | Skill 1 |    | Skill 2 |    | Skill N |
        +---------+    +---------+    +---------+
              |              |              |
              v              v              v
        +---------+    +---------+    +---------+
        | Agent A |    | Agent B |    | Agent C |
        +---------+    +---------+    +---------+
```

- 28 个专精 agent <!-- .element: class="fragment" -->
- 37 个技能 <!-- .element: class="fragment" -->
- 零配置即可使用 <!-- .element: class="fragment" -->

注：OMC 将 Claude 从一个独奏者，转变为指挥一支专精 AI agent 交响乐团的指挥家。

---

## 核心理念

> "你是指挥，而不是演奏者。"

**传统 AI 工作流：**
```
User -> Claude -> [Does everything itself]
```

**OMC 工作流：**
```
User -> Claude (指挥) -> [把任务分派给专家]
                             |
             +---------------+------------+
             |               |            |
        architect        executor      designer
        (分析)          (实施)        (UI/UX)
```

**Claude 变身为智能编排者**，为每个任务分派最合适的专家。

注：这是核心心智模型。Claude 不再是一个试图包揽一切的通用型选手，而成为一个聪明的协调者。

---

## 使用 OMC 前后对比

| 方面 | 使用前 | 使用后 |
|--------|-----------|-----------|
| **任务执行** | 单线程 | 多 agent 并行 |
| **复杂任务** | 手动拆分 | 自动分解 |
| **模型选择** | 永远同一个模型 | 智能路由（Haiku/Sonnet/Opus）|
| **持久性** | 容易放弃 | 持续直到验证通过 |
| **成本** | 昂贵 | 节省 30-50% |
| **学习曲线** | 记忆命令 | 自然语言 |

**示例——「修复所有 TypeScript 错误」：**

使用前：你手动逐个查找并逐一修复

使用后：5 个并行 agent 同时认领并修复错误

注：成本节省来自用 Haiku（$0.25/1M tokens）处理简单任务，而不是用 Opus（$15/1M tokens）。

---

## 关键统计

| 指标 | 数值 |
|--------|-------|
| 专精 Agent | 32 |
| 技能 | 35+ |
| 执行模式 | 8 |
| 生命周期钩子 | 19 |
| 模型层级 | 3（Haiku、Sonnet、Opus）|
| 许可证 | MIT |

**Token 成本对比：**

| 模型 | 输入 | 输出 |
|-------|-------|--------|
| Haiku | $0.25/1M | $1.25/1M |
| Sonnet | $3/1M | $15/1M |
| Opus | $15/1M | $75/1M |

注：智能模型路由意味着，选择能胜任任务的最便宜模型。

---

## 架构概览

```
+--------------------------------------------------------------------+
||                           USER INPUT                                ||
||                    "autopilot: build a REST API"                    ||
+------------------------------------+-------------------------------+
                                     |
                                     v
+--------------------------------------------------------------------+
||                      CLAUDE CODE (CONDUCTOR)                      ||
||  +----------------+  +----------------+  +---------------------+    ||
||  | 关键词         |  | 技能          |  | Agent              |    ||
|   | 检测            |  ->| 解析          | ->| 分派            |    ||
||  +----------------+  +----------------+  +---------------------+    ||
+------------------------------------+-------------------------------+
                                     |
              +----------------------+----------------------+
              |                      |                      |
              v                      v                      v
     +---------------+      +---------------+      +---------------+
     | 技能层        |      | 技能层        |      | 技能层        |
     | autopilot     |      | ultrawork     |      | ralph         |
     +-------+-------+      +-------+-------+      +-------+-------+
             |                      |                      |
             v                      v                      v
     +---------------+      +---------------+      +---------------+
     | AGENT 层      |      | AGENT 层      |      | AGENT 层      |
     | analyst       |      | executor      |      | architect     |
     | architect     |      | executor-low  |      | critic        |
     | executor      |      | build-fixer   |      | executor      |
     +---------------+      +---------------+      +---------------+
```

注：架构分三层——关键词触发技能，技能协调 agent，agent 做实际工作。

---

<!-- .slide: data-background="#1a1a2e" -->

# 第二部分
## 五大核心执行模式

---

## 模式一：Autopilot — 是什么？

**从想法到可用代码的全自主执行**

```
"autopilot: build a REST API for a bookstore"
```

**5 个阶段：**

1. **扩展（Expansion）**— 把模糊想法转成详细规格
2. **规划（Planning）**— 创建带验证的实施计划
3. **执行（Execution）**— 用并行 agent 构建（Ralph + Ultrawork）
4. **质量保证（QA）**— 反复测试直到全部通过（最多 5 轮）
5. **验证（Validation）**— 多评审人批准（架构 + 安全 + 代码评审）

注：Autopilot 是旗舰体验。把想法交给它，离开，回来就看到可运行的代码。

---

## 模式 1：Autopilot — 工作原理

```
Phase 0: 扩展 EXPANSION
    |
    +-> 分析师 Analyst (Opus) 提取需求
    +-> 架构师 Architect (Opus) 创建技术规格
    |
    v
Phase 1: 规划 PLANNING
    |
    +-> 架构师创建实施计划（直接模式）
    +-> 评审 Critic 验证计划
    |
    v
Phase 2: 执行 EXECUTION
    |
    +-> 激活 Ralph + Ultrawork
    +-> Executor-low （简单任务）
    +-> Executor （标准任务）
    +-> Executor-high （复杂任务）
    |
    v
Phase 3: 质量保障 QA （最多 5 轮）
    |
    +-> 构建 -> 静态检查 -> 测试 -> 修复
    |
    v
Phase 4: 验证 VALIDATION
    |
    +-> 架构师 （功能完整性）
    +-> 安全评审员 （漏洞检查）
    +-> 代码评审员 （质量审查）
```

注：每个阶段都有明确的进入和退出标准。只有在阶段验证完成后，Autopilot 才会继续。

---

## 模式 1：Autopilot — 何时使用

**最适合：**
- 从零开始的新项目
- 完整功能实现
- 端到端工作流

**触发关键词：**
```
autopilot, auto pilot, autonomous
build me, create me, make me
full auto, handle it all
I want a/an...
```

**示例命令：**
```
autopilot: build a REST API with CRUD for inventory

/oh-my-claudecode:autopilot Add OAuth2 authentication

autopilot: create a CLI tool that tracks daily habits
```

注：Autopilot 结合了所有最佳能力——规划、持久性、并行、验证。

---

## 模式 2：Ultrapilot — 是什么？

**多 P2 并行的 autopilot，最多 5 个并发 worker**

对合适的任务，比标准 autopilot 快 3-5 倍。

```
"ultrapilot: build a full-stack todo app"
```

**核心创新：文件所有权分区**

- 每个 worker 获得独占的文件集合
- worker 之间无冲突
- 共享文件由协调者处理

注：当你需要 autopilot 级别的自主性，又想通过并行获得最大速度时，用 Ultrapilot。

---

## 模式 2：Ultrapilot — 工作原理

```
用户输入: "构建一个全栈待办应用"
                    |
                    v
          [ULTRAPILOT 协调器]
                    |
        任务分解 + 文件分区
                    |
        +-----------+-----------+-----------+-----------+-----------+
        |           |           |           |           |           |
        v           v           v           v           v
    [Worker-1]  [Worker-2]  [Worker-3]  [Worker-4]  [Worker-5]
     后端         前端        数据库       API 文档      测试
    (src/api/)  (src/ui/)   (src/db/)   (docs/)    (tests/)
        |           |           |           |           |
        +-----------+-----------+-----------+-----------+
                              |
                              v
                   [集成阶段]
           （共享文件：package.json, tsconfig.json）
                              |
                              v
                   [验证阶段]
                    （全系统测试）
```

注：分解阶段至关重要——它使用架构师 agent 识别可并行的安全的子任务。

---

## 模式 2：Ultrapilot — 何时使用

**最适合：**
- 多组件系统（前端 + 后端 + 数据库）
- 具有清晰模块边界的大型重构
- 多服务架构
- 并行测试生成

**速度对比：**

| 任务 | Autopilot | Ultrapilot |
|------|-----------|------------|
| 全栈应用 | 约 75 分钟 | 约 15 分钟 |
| 多服务重构 | 约 32 分钟 | 约 8 分钟 |
| 测试覆盖 | 约 50 分钟 | 约 10 分钟 |

**触发：**
```
ultrapilot, parallel build, swarm build
```

注：如果你的任务有 3 个以上独立组件，ultrapilot 很可能比 autopilot 更快。

---

## 模式 3：Swarm — 是什么？

**N 个协调的 agent，原子式认领任务**

```
/swarm 5:executor "fix all TypeScript errors"
```

**架构：**
- 基于 SQLite 的任务池
- 通过事务实现原子认领
- 5 分钟租约超时，自动释放
- 心跳监控实现容错

注：Swarm 犹如一支开发团队分工一份共享任务清单，谁都能抢下一个任务。

---

## 模式 3：Swarm — 工作原理

```
/swarm 5:executor "fix all TypeScript errors"
              |
              v
              [SWARM 编排器]
              |
        +--+--+--+--+--+
        |  |  |  |  |
        v  v  v  v  v
        E1 E2 E3 E4 E5    <-- 5 个 executor agent
        |  |  |  |  |
        +--+--+--+--+
              |
          [SQLITE 数据库]
    +---------------------+
    | tasks 表            |
    |---------------------|
    | id, description     |
    | status: pending,    |
    |   claimed, done,    |
    |   failed            |
    | claimed_by          |
    | heartbeat 跟踪      |
    +---------------------+
```

**认领协议：**
1. Agent 调用 `claimTask()`
2. SQLite 事务原子地更新状态
3. Agent 完成任务
4. Agent 调用 `completeTask()` 或 `failTask()`

注：SQLite 事务保证不会有两个 agent 认领同一任务——真正的原子性。

---

## 模式 3：Swarm — 何时使用

**最适合：**
- 大量相互独立的任务
- 逐文件的操作
- 批处理

**使用场景：**
```
# 修复所有 TypeScript 错误
/swarm 5:executor "fix all TypeScript errors"

# 为所有 UI 组件添加 Material-UI 样式
/swarm 3:designer "implement Material-UI styling for all components"

# 安全审计所有端点
/swarm 4:security-reviewer "review all API endpoints"

# 添加文档
/swarm 2:writer "add JSDoc comments to all exported functions"
```

注：当你有很多互不依赖的独立任务时，Swarm 表现出色。

---

## 模式 4：Pipeline — 是什么？

**带数据传递的顺序 agent 链**

就像 Unix 管道，但用于 AI agent。

```
/pipeline explore -> architect -> executor "add authentication"
```

**一个 agent 的输出成为下一个 agent 的输入：**

```
[探索 findings] -> [架构师分析] -> [执行者实现]
```

注：Pipeline 用于必须按特定顺序执行的工作流，其中每一步都需要上一步的上下文。

---

## 模式 4：Pipeline — 内置预设

| 预设 | 阶段 | 用途 |
|--------|--------|---------|
| `review` | explore -> architect -> critic -> executor | 主要功能、重构 |
| `implement` | planner -> executor -> tdd-guide | 带测试的新功能 |
| `debug` | explore -> architect -> build-fixer | Bug、构建错误 |
| `research` | parallel(researcher, explore) -> architect -> writer | 技术决策 |
| `refactor` | explore -> architect-medium -> executor-high -> qa-tester | 安全重构 |
| `security` | explore -> security-reviewer -> executor -> security-reviewer-low | 安全修复 |

**用法：**
```
/pipeline review "add rate limiting to api"
/pipeline debug "login fails with OAuth"
/pipeline security "audit user authentication"
```

注：这些预设为常见工作流内置了最佳实践。在创建自定义 pipeline 之前，先从这些预设开始。

---

## 模式 4：Pipeline — 何时使用

**最适合：**
- 多阶段处理工作流
- 代码评审流程
- 从研究到实施的流程

**自定义 Pipeline 语法：**
```
# 基本顺序
/pipeline agent1 -> agent2 -> agent3 "task"

# 带模型规格
/pipeline explore:haiku -> architect:opus -> executor:sonnet "task"

# 带并行阶段
/pipeline [explore, researcher] -> architect -> executor "task"
```

**数据流：**
```json
{
  "pipeline_context": {
    "original_task": "用户请求",
    "previous_stages": [
      {"agent": "explore", "findings": "..."}
    ],
    "current_stage": "architect"
  }
}
```

注：数据传递协议确保每个 agent 拥有来自前序阶段的完整上下文。

---

## 模式 5：Ecomode — 是什么？

**节能高效 token 的并行执行**

比标准执行便宜 30-50%。

```
eco: implement new feature
```

**策略：**
- 所有任务优先用 Haiku（最便宜）
- 需要时才升级到 Sonnet
- 除非绝对必要，否则避免 Opus

注：Ecomode 适合追求成本控制，或做探索性的工作。

---

## 模式 5：Ecomode — 工作原理

**路由规则：**

| 任务类型 | 标准模式 | Ecomode |
|-----------|---------------|---------|
| 简单查询 | architect-low | architect-low |
| 标准实现 | executor | executor-low (首次尝试) |
| 复杂分析 | architect | architect-medium |
| 规划 | planner (Opus) | 尽量避免 |

Agent 路由表：（下表指 Haiku 优先，Sonnet 兜底，避免 Opus）

| 领域 | 首选 (Haiku) | 回退 (Sonnet) | 避免 (Opus) |
|--------|-------------------|-------------------|--------------|
| 分析 | architect-low | architect-medium | ~~architect~~ |
| 执行 | executor-low | executor | ~~executor-high~~ |
| 搜索 | explore | - | ~~explore-high~~ |
| 前端 | designer-low | designer | ~~designer-high~~ |

注：Ecomode 先尝试最便宜的选项，只有失败时才升级。

---

## 模式 5：Ecomode — 何时使用

**最适合：**
- 预算敏感的项目
- 迭代式开发（许多小的改动）
- 探索性工作
- 个人项目

**节省成本示例：**

| 任务 | 标准成本 | Ecomode 成本 | 节省 |
|------|--------------|--------------|---------|
| 100 个简单修复 | 约$3.00 | 约$0.50 | 83% |
| 功能实现 | 约$1.50 | 约$0.75 | 50% |
| 完整构建 | 约$10.00 | 约$5.00 | 50% |

**触发关键词：**
```
eco, efficient, save-tokens, budget
```

注：关键洞见是 80% 的任务可以由 Haiku 完成——只有真正复杂的推理才需要 Opus。

---

<!-- .slide: data-background="#1a1a2e" -->

# 第三部分
## Agent 系统

---

## 28 个专精 Agent

| 领域 | Agent |
|--------|--------|
| **分析** | architect, architect-medium, architect-low |
| **执行** | executor, executor-high, executor-low |
| **搜索** | explore, explore-high |
| **研究** | researcher |
| **前端** | designer, designer-high, designer-low |
| **文档** | writer |
| **视觉** | vision |
| **规划** | planner, analyst |
| **评审** | critic |
| **测试** | qa-tester |
| **安全** | security-reviewer, security-reviewer-low |
| **构建** | build-fixer |
| **TDD** | tdd-guide, tdd-guide-low |
| **代码审查** | code-reviewer |
| **数据科学** | scientist, scientist-high |

注：每个 agent 都有针对其领域优化的专精提示与工具集。

---

## 三层模型路由

```
+------------------+------------------+------------------+
|   LOW (Haiku)    |  MEDIUM (Sonnet) |   HIGH (Opus)    |
|------------------|------------------|------------------|
| $0.25/$1.25/1M   | $3/$15/1M        | $15/$75/1M       |
|------------------|------------------|------------------|
| 简单查询         | 标准工作         | 复杂推理         |
| 快速搜索         | 功能实现         | 架构             |
| 基础修复         | 中等调试         | 深度调试         |
| 文档            | UI 组件         | 安全审计        |
+------------------+------------------+------------------+
       ^                  ^                  ^
       |                  |                  |
  默认使用          当 LOW 失败时升级    只有真正需要
                                      才使用
```

**成本示例：**
- 1000 个简单问题：Haiku = $0.25，而 Opus = $15（便宜 60 倍！）

注：分层体系构成 OMC 成本效率的核心。始终低位开始，需要时再升级。

---

## 智能分派

**OMC 自动选择合适的 agent：**

| 任务 | 所选 Agent | 模型 |
|------|---------------|-------|
| "这个函数返回什么？" | architect-low | Haiku |
| "找到 UserService 定义位置" | explore | Haiku |
| "给登录表单加校验" | executor-低 | Haiku |
| "实现 OAuth2 流程" | executor | Sonnet |
| "调试权限里的竞态" | architect | Opus |
| "重构整个认证模块" | executor-high | Opus |

**分派代码：**
```javascript
Task(
  subagent_type="oh-my-claudecode:executor-low",
  model="haiku",
  prompt="Add validation to the login form"
)
```

注：model 参数始终显式传递——Claude Code 不会自动从 agent 定义应用模型。

---

## Agent 组合

**技能 + Agent 结合出强大工作流：**

```
"ralph ultrawork: migrate database"
   |          |
   |          +-> 并行执行 (ultrawork)
   +--------------> 持久性 (ralph)
```

**真实示例：**
```
ralph ultrawork git-master: refactor authentication
  |       |                    |
  |       |                    +-> Git 专业知识（原子提交）
  |       +---------------------> 最大并行
  +-----------------------------> 持续直到验证通过
```

**结果：** 持久、并行、git 感知的重构

注：组合恰恰是 OMC 的闪光点——把行为组合成你需要的精确工作流。

---

## 分派分类

**带自动检测的语义任务分类：**

| 分类 | 层级 | 温度 | 思考 | 自动检测的关键词 |
|----------|------|------|----------|---------------------------|
| `visual-engineering` | HIGH | 0.7 | `high` | "UI", "component", "style" |
| `ultrabrain` | HIGH | 0.3 | `max` | "debug", "architecture" |
| `artistry` | MEDIUM | 0.9 | `medium` | "creative", "brainstorm" |
| `quick` | LOW | 0.1 | `low` | "find", "what is", "where" |
| `writing` | MEDIUM | 0.5 | `medium` | "document", "explain" |

**工作原理：**
```
用户: "在认证模块调试竞态条件"
            |
            v
     检测到： "debug" 关键词
            |
            v
     分类: ultrabrain
            |
            v
     设置: HIGH 层级, temp=0.3, 最大化思考
```

注：分类会自动调整模型参数，针对不同任务类型实现最优表现。

---

<!-- .slide: data-background="#1a1a2e" -->

# 第四部分
## 现场演示场景

---

## 演示 1：Autopilot

**命令：**
```
autopilot: build a REST API for a bookstore with CRUD operations
```

**会发生什么：**

1. **扩展阶段**（约 2 分钟）
   - 分析师提取：实体（Book, Author）、操作（CRUD）、约束
   - 架构师创建：技术规格、数据库 schema、API 设计
2. **规划阶段**（约 1 分钟）
   - 架构师创建实施计划
   - 评审者验证完整性
3. **执行阶段**（约 10-15 分钟）
   - 执行者并行实现路由、模型、测试
4. **质量保障阶段**（约 3-5 分钟）
   - 构建、静态检查、测试循环直到全绿
5. **验证阶段**（约 2 分钟）
   - 架构、安全、代码审查批准

注：现场演示会显示 HUD，展示通过各个阶段的进度。

---

## 演示 2：Ultrawork

**命令：**
```
ulw fix all TypeScript errors
```

**会发生什么：**
```
[ULTRAWORK 已激活]

正在扫描 TypeScript 错误...
在 8 个文件中找到 23 个错误。

启动并行 agent:
  [executor-low:1] -> src/api/routes.ts    (5 个错误)
  [executor-low:2] -> src/api/handlers.ts  (3 个错误)
  [executor-low:3] -> src/ui/App.tsx       (4 个错误)
  [executor-low:4] -> src/db/models.ts     (6 个错误)
  [executor-low:5] -> src/utils/helpers.ts  (5 个错误)

进度: [====================] 100%

在 2分34秒内修复全部 23 个错误
构建: 通过
```

注：Ultrawork 是保留并行模式——没有规划开销，只有并行执行。

---

## 演示 3：Pipeline

**命令：**
```
/pipeline review "为 API 添加频率控制 rate limiting"
```

**会发生什么：**

```
PIPELINE: review
Stages: explore -> architect -> critic -> executor "add..."

[Stage 1/4: explore] RUNNING
  在代码库中寻找限流模式...
  找到: src/middleware/ 中的中间件模式
  找到: 依赖中的 express-rate-limit
  输出: 识别出 3 个相关文件

[Stage 2/4: architect] RUNNING
  分析: 现有中间件模式
  建议: 使用 express-rate-limit 搭配 Redis 存储
  设计: 每路由配置
  输出: 创建了实施计划

[Stage 3/4: critic] RUNNING
  审查计划...
  已批准并附注: 考虑处理突发流量

[Stage 4/4: executor] RUNNING
  实现限流...
  已创建: src/middleware/rateLimiter.ts
  已修改: src/api/routes.ts
  输出: 实现完成

PIPELINE 完成
```

注：Pipeline 确保每个阶段的输出都建立在上一个阶段之上。

---

## 演示 4：Planning

**演示**：
```
plan the authentication system
```

**:
```
[PLAN 技能已激活]

我将帮助你规划认证系统。让我先问几个问题:

Q1: 需要哪些认证方式?
  [ ] 用户名/密码
  [ ] OAuth2 (Google, GitHub)
  [ ] 魔术链接（邮箱）
  [ ] API 密钥

Q2: 采用什么会话管理方式?
  [ ] JWT 令牌
  [ ] 服务端会话
  [ ] 混合

Q3: 安全需求有哪些?
  [ ] 需要双因素认证(2FA)
  [ ] 密码复杂度规则
  [ ] 认证端点的限流
  [ ] 失败尝试后的账户锁定

[在用户回答之后...]

创建实施计划...
已保存计划到: .omc/plans/auth-system.md
```

注:Planning 模式采用交互式访谈收集需求，然后创建详细计划。

---

## 演示 5：Ralph

**演示:**
```
ralph: refactor the auth module to use dependency injection
```

**会发生什么**:

```
[RALPH 已激活 - 不会停止，直到验证完成]

迭代 1/10:
  分析认证模块结构...
  创建重构计划...
  执行更改...
  错误: 在 auth.test 中测试失败

迭代 2/10:
  分析失败: Mock 没有为新的依赖注入模式更新
  修复测试 mock...
  重新运行测试...
  错误: UserService 中的类型错误

迭代 3/10:
  修复 UserService 类型...
  所有测试通过...
  启动架构师进行验证...

.注意:
[ARCHITECT 验证]
  检查: 依赖注入模式是否正确应用
  检查: 所有测试通过
  检查: 没有类型错误
  结果: 已批准

[RALPH 完成]
重构验证完成，共 3 次迭代。
```

注：Ralph 是持久模式——自纠错并持续运行，直到架构师验证完成。

---

# 第五部分
## 开发者体验

---

## 魔法关键词

**面向资深用户的快捷方式：**

| 关键词 | 效果 | 示例 |
|---------|-------------------|---------|
| `autopilot` | 全自动执行 | `autopilot:` |
| `ralph` | 持续直到完成 | `ralph:` |
| `ulw` | 最大并行 | `ulw fix all errors` |
| `eco` | 节省 token 执行 | `eco: add validation` |
| `plan` | 交互式规划 | `plan the api` |
| `ralplan` | 迭代式规划共识 | `ralplan new feature` |

**组合也能工作：**
```
ralph ulw: migrate database
```

注: 关键词是可选的——自然语言也能正常工作。关键词只是给你显式的控制。

---

## HUD 状态栏

**OMC 状态的实时可视：**

| OMC | autopilot:exec | 3 个 agent | 5/12 任务 | ctx:45%% | $2.34 |
|---|---|---|---|---|---|
| 活动模式 | #运行中 Agent | 进度 | 上下文窗口 | 成本 |

**配置：**
```
/oh-my-claudecode:hud setup
```

**预设：**
- `minimal` — 只显示活动模式
- `focused` — 模式 + 进度（默认）
- `full` — 包含成本在内的所有内容

注：HUD 与 Claude Code 的 statusLine API 集成，实时显示编排状态。

---

## 便签智能系统

**计划作用域的知识捕获：**
位置：`.omc/notepads/{plan-name}/`

| 文件 | 用途 | 示例 |
|------|---------|------|
| `learnings.md` | 技术发现 | "Redis 需要显式 TTL 用于限流 key" |
| `decisions.md` | 设计决策 | "选用 JWT 而非会话，以便无状态扩展" |
| `issues.md` | 已知问题 | "生产环境 OAuth 回调 URL 必须为 HTTPS" |
| `problems.md` | 障碍 | "需要 Redis 实例用于限流" |

API：
```javascript
addLearning("plan-auth", "OAuth refresh tokens expire after 7 days")
addDecision("plan-auth", "Using passport.js for OAuth integration")
getWisdomSummary("plan-auth")
```

注：知识持久跨会话——之后在同一计划上的工作会自动获得这些上下文。

---

## 分析 & 成本追踪

**追踪 token 用量与成本：**

```
$ omc-analytics summary

最近 7 天会话摘要
-----------------------------
总会话数: 23
总 tokens: 1,234,567
总成本: $18.45

各模型:
  Haiku:  890,000 tokens  ($0.89)
  Sonnet: 300,000 tokens  ($4.50)
  Opus:    44,567 tokens  ($13.06)

各模式:
  autopilot:  45% 成本
  ultrawork:  30% 成本
  其他:      15% 成本

最贵的 5 个会话:
  1. "build fullstack app"     $4.23
  2. "debug auth race cond"    $2.15
  3. "refactor database"       $1.89

注：分析帮你了解 token 花在哪，并优化使用模式。

---

<!-- .slide: data-background="#1a1a2e" -->

# 第六部分
## 上手入门

---

## 安装

**方法 1：插件市场（推荐）**
```bash
/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode
```

**方法 2：全局 npm**
```bash
npm install -g oh-my-claudecode
```

**方法 3：手动 git clone**
```bash
git clone https://github.com/Yeachan-Heo/oh-my-claudecode.git
cd oh-my-claudecode
npm install && npm run build
```

**前置要求：**
- Claude Code CLI
- Claude Max/Pro 订阅，或 Anthropic API key
- Node.js 20+

注: 插件仓库最简单，一个命令搞定。

---

## 首次使用步骤

**第 1 步：安装**
```bash
/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode
```

**第 2 步：设置**
```bash
/oh-my-claudecode:omc-setup
```
（会配置默认项、HUD、偏好）

**第 3 步：上手做点什么**
```
autopilot: build a REST API for managing tasks
```

**就是这样。** 其余全部自动。

注: 零学习曲线意味着安装后即可立即使用 OMC。

---

## 配置

**项目级：** 项目根目录的 `CLAUDE.md`
**全局：** `~/.claude/CLAUDE.md`

**关键设置:**

```json
// ~/.claude/settings.json
{
  "omc": {
    "defaultExecutionMode": "ultrawork",
    "autopilot": {
      "maxIterations": 10,
      "maxQaCycles": 5,
      "skipValidation": false
    },
    "hud": {
      "preset": "focused"
    }
  }
}
```

**Agent 自定义：**
- 在 `agents/*.md` 中修改 agent 提示词
- 为每个 agent 覆盖工具
- 创建自定义 agent

注: 大多数用户从未需要配置任何东西——默认参数已适合典型使用。

---

# 第七部分
## 收尾

---

## 现实世界的使用场景

| 使用场景 | 最佳模式 | 原因 |
|----------|-----------|------|
| **后端 API 开发** | autopilot | 完整端到端工作流 |
| **前端组件库** | ultrapilot | 大量独立组件 |
| **数据库迁移** | ralph | 需要穿越错误的持久性 |
| **CI/CD 管道搭建** | pipeline:implement | 顺序阶段 |
| **文档生成** | swarm:writer | 并行文档编写 |
| **Bug 分诊与修复** | swarm:executor | 大量独立修复 |
| **安全审计** | pipeline:security | 结构化审查流程 |
| **探索性原型** | plan（预算敏感） | 迭代 |

注: 将正确模式匹配到任务类型，是用好 OMC 的关键。

---

## 资源

**GitHub 仓库**
```
github.com/Yeachan-Heo/oh-my-claudecode
```

**网站 & 文档**
```
yeachan-heo.github.io/oh-my-claudecode-website
```

**NPM 包**
```
npm install -g oh-my-claudecode
```

**文档目录**
```
/docs/REFERENCE.md      - 完整特性参考
/docs/MIGRATION.md      - 升级指南
/docs/ARCHITECTURE.md   - 工作原理
```

**获取帮助**
```
/oh-my-claudecode:omc-help    - 使用指南
/oh-my-claudecode:omc-doctor  - 问题诊断
```

注: GitHub 仓库包含所有文档、示例和问题追踪。

---

## 问答

| 问题 | 回答 |
|----------|--------|
| OMC 适配 Claude API keys 吗？ | 支持，Max/Pro 订阅和 API keys 都可以 |
| 我可以把 OMC 用于其他 AI 模型吗？ | 不可以，OMC 专为 Claude Code 设计 |
| 如何停止失控的 autopilot？ | 说 "停止"、"取消" 或 `/oh-my-claudecode:cancel` |
| HUD 不显示怎么办？ | 运行 `/oh-my-claudecode:hud setup` |
| 能创建自定义 agent 吗？ | 能，在 `agents/` 目录添加 `.md` 文件 |
| 有没有成本上限？ | 没有内置限制，但 `eco` 可以控制成本 |

---

## 致谢

**oh-my-claudecode**

零门槛。终极力量。

```
github.com/Yeachan-Heo/oh-my-claudecode
```

**现在就开始：**
```
/plugin marketplace add https://github.com/Yeachan-Heo/oh-my-claudecode
/plugin install oh-my-claudecode
autopilot: build something amazing
```

---

## 附录 A：完整 Agent 参考

| Agent | 模型 | 最佳用途 |
|-------|-------|----------|
| architect | opus | 复杂架构、深度调试 |
| architect-medium | sonnet | 中等分析 |
| architect-low | haiku | 快速代码咨询 |
| executor | sonnet | 标准实现 |
| executor-high | opus | 复杂重构 |
| executor-low | haiku | 简单修复 |
| explore | haiku | 快速文件搜索 |
| explore-high | opus | 架构级搜索 |
| designer | sonnet | UI 组件 |
| designer-high | opus | 设计系统 |
| researcher | sonnet | 外部文档、API |
| writer | haiku | 文档 |
| vision | sonnet | 图像分析 |
| planner | opus | 战略规划 |
| analyst | opus | 需求提取 |
| critic | opus | 计划评审 |
| qa-tester | sonnet | 命令行测试 |
| security-reviewer | opus | 安全审计 |
| security-reviewer-low | haiku | 快速安全扫描 |
| build-fixer | sonnet | 构建错误解析 |
| tdd-guide | sonnet | TDD 工作流 |
| tdd-guide-low | haiku | 快速测试建议 |
| code-reviewer | opus | 代码质量评审 |
| scientist | sonnet | 数据分析 |
| scientist-high | opus | 复杂 ML/假设 |

---

## 附录 B：完整技能参考

| 技能 | 用途 | 触发 |
|------|---------|-------
| autopilot | 全自动执行 | "autopilot", "build me" |
| ultrapilot | 并行 autopilot | "ultrapilot", "parallel build" |
| ralph | 持久性模式 | "ralph", "don't stop" |
| ultrawork | 最大并行 | "ulw", "ultrawork" |
| ecomode | 节省 token 模式 | "eco", "budget" |
| swarm | 协调 agent | `/swarm N:agent` |
| pipeline | 顺序串联 | `/pipeline preset` |
| plan | 规划访谈 | "plan the" |
| ralplan | 迭代规划 | "ralplan" |
| cancel | 停止任何模式 | "stop", "cancel" |
| analyze | 深度调查 | "analyze", "debug" |
| deepsearch | 彻底搜索 | "search", "find" |
| deepinit | 生成 AGENTS.md | "index codebase" |
| frontend-ui-ux | 设计感知 | UI 上下文 (自动) |
| git-master | Git 专业知识 | Git 上下文 (自动) |
| ultraqa | QA 循环 | "gateway", "QA" |
| learner | 提取技能 | "extract skill" |
| note | 保存到便签 | "remember", "note" |
| hud | 配置 HUD | `/hud` |
| doctor | 诊断问题 | `/doctor` |
| help | 显示使用指南 | `/help` |
| setup | 设置向导 | `/omc-setup` |
| ralph-init | 初始化 PRD | `/ralph-init` |
| release | 发布工作流 | `/release` |
| review | 评审计划 | "review plan" |
| research | 科学家编排 | "research", "statistics" |
| tdd | 强制 TDD | "tdd", "test first" |
| mcp-setup | 配置 MCP | "setup mcp" |

---

## 附录 C：键盘快捷键汇总

| 快捷键 | 完整命令 | 作用 |
|------|--------------|------|
| `autopilot:` | `/oh-my-claudecode:autopilot` | 全自动模式 |
| `ralph:` | `/oh-my-claudecode:ralph` | 持久性模式 |
| `ulw` | `/oh-my-claudecode:ultrawork` | 并行执行 |
| `eco:` | `/oh-my-claudecode:eco` | 节省 token 模式 |
| `plan` | `/oh-my-claudecode:plan` | 规划访谈 |

**组合:**
```
ralph ulw: task        # 持久 + 并行
ralph eco: task        # 持久 + 高效
autopilot eco: task    # 自动 + 高效 (eco 胜出)
```

注: 当关键词冲突时，更严格的模式获胜（eco 胜于 ulw）。