---
title: "Grilling - 需求追问方法论"
category: "AI 开发方法论"
description: "从 mattpocock grill 家族蒸馏的交互式需求澄清与设计压力测试引擎。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["grilling", "requirement", "sop"]
---

# Grilling SOP — 需求追问引擎

> 来源: [mattpocock/skills](https://github.com/mattpocock/skills) grill-me/grilling/batch-grill-me
> 适配: GA 交互式需求对齐

## 何时使用

以下场景触发 Grilling 模式：
- 用户给出模糊/开放式的任务描述
- 需要做设计决策但前提未明确
- 想"压力测试"一个计划或设计
- 用户明确说"帮我想想"/"有什么遗漏的"
- 任务涉及多个未说明的前提假设

## 核心原则

1. **决定问你，事实我查** — 需要查环境（文件/代码/工具）的事，由 GA 自行探测，不问用户
2. **一次一问** — 每次只抛一个问题+我的推荐答案，等用户回答再继续
3. **决策树遍历** — 每个问题分支到后继问题，直到所有分支被覆盖
4. **不提前行动** — 直到用户确认达成共识才执行

## 流程

### Step 1: 构建初始决策树

从用户描述中提取第一层决策节点。对每个节点：
- 明确它的**前提**（什么必须先决定）
- 明确它的**分支**（不同的选择方向）
- 给出你的**推荐答案**（基于已知信息）

示例设计树:
```
[目标是什么？]
  ├── [核心功能有哪些？]
  │     ├── [优先级排序？]
  │     └── [MVP边界？]
  ├── [技术栈选择？]
  │     ├── [性能要求？]
  │     └── [已有依赖？]
  └── [时间线？]
        └── [里程碑？]
```

### Step 2: 轮次追问

每轮做三件事：
1. **计算 frontier** — 所有前提已满足、可以问的决策节点
2. **批量提问**（推荐）或逐个提问
   - 编号每个问题
   - 给出你的推荐答案
   - 等待回答
3. **处理回答**：
   - 事实类回答 → 标记为已决定，更新设计树
   - 需查证类 → 启动后台探测（不阻塞本轮其他问题）
   - 新分歧 → 扩展设计树，添加子节点

### Step 3: 前置事实自动查证

当 frontier 中的问题需要环境信息时：
- 启动子进程并行查证（文件内容、代码结构、工具输出）
- 不阻塞其他 frontier 问题
- 子进程返回后纳入下一轮

### Step 4: 收敛判定

frontier 为空时停止。意味着：
- 每个分支都已访问
- 没有未说明的假设
- 所有决策已有答案或依赖于用户已决定的答案

### Step 5: 确认

总结设计树的关键决策点，请用户确认达成共识。确认前不执行任何实现工作。

## 输出

grilling 结束时产出：
1. 设计树摘要（所有已决定的节点）
2. 待办事实探测结果（如果有）
3. 用户确认的下一步行动

## GA 集成

此 SOP 作为模型调用型技能：当用户描述模糊或涉及多个未说明假设时自动触发。
用户也可显式要求"grill me"来启动。
---

## 原版强化细则（Morphling 蒸馏自 mattpocock/skills 的 grilling 家族）

> 来源补充: skills/productivity/grilling/{SKILL.md,agents/openai.yaml}、docs/productivity/{grilling,grill-me,skills/engineering/grill-with-docs}.md
> 三 skill 分层: **grilling**(core primitive,唯一可被模型自动触发) / **grill-me**(薄壳壳,仅"Run a /grilling">
> ✓总结, stateless无仓库), / **grill-with-docs**(工程壳, 在 grilling + domain-modeling, 写 CONTEXT.md+ADR)

### A. 每轮 = 整 frontend 强制批问（不是逐条滴灌）
- 一轮就问当前 frontier 全部可问决策，绝不逐条问；两个问题互相依赖则属于下一轮，不进本轮。
- 一轮通常 3 个左右问题、十几问跨 3 轮；**数问的数看轮次不看问题数**。

### B. ❓/➡️ 精确问答格式
每问固定形状（使整轮可按编号回答——"1要, 2选B, 3不要"）：
```
❓ **Q1** - **<标题>**: <正文,可多段含选项>

➡️ <你的推荐答案>(单独一行)
```
- edge: 当推荐答案与问题措辞相反（同意推荐=对问题答"否"）时说破。

### C. 事实/决策强分离
- 事实：frontier 问题需要环境(文件/代码/工具)时派 sub-agent 去查，**绝不问用户**；探索进行中的决策不算已settled，只有其 downstream 等待，其余立刻问。
- 决策：只归用户，必须等、必须问。严禁"自问自答"。

### D. 收敛与用户全程 control
- 结束=frontier 空；**确认共享理解前绝不行动**。无 async 模式（无人回答的 session 只产出 agent 自己的偏见）。
- 无问题数上限（scope 用自然语言控："wrap up"/把蛋糕切小块再各各 grill），过长常因 scope 太大。
- 想"一次一问"：给 agent 加全局规则 `When grilling, ask one question at a time.`

### E. ungrillable → 去 prototype（talk 解决不了就做出来看）
遇到"……must be built to be answered"（如"响应交互应该是什么感觉"）→ 停止提问，先做可弃原型，回来看一眼再一句答完。别靠 rephrase 越磨越大。

### F. It's working if（运行验收标准）
- 一轮是编号列表、每条推荐独行 `➡️`，整轮可按编号回答；
- 轮内无互依赖；后轮能问前轮问不了的；
- 主动查事实（读文件/派 sub-agent）而非把能自查的甩给用户；
- 后台研究不堵本轮，只信 downstream；
- 末尾停下要确认而非开工；问数高、轮数低。

### 坑（原版已知）
- skill 间嵌套加载不稳：被调用 skill 不保证被真实加载。grill-with-docs 需显式确认已载入 grilling+domain-modeling，否则退化为"一次问完无推荐"的即兴访谈。
- "它自问自答"= 运行 bug（另一 skill 以"resolve ticket"框架调用时最常见），故必须事实/决策分离。
- 问数可超过限，靠自然语言控制。

### GA 集成（增强）
- GA 作一次 grilling 前，先明确分层选择：无环境/无文件/话题任意 → 用 grill-me 模式(纯问答, 不落盘不作假设)；有代码库要对齐 → grill-with-docs 模式(边grill边落决策到文档)。
- "grill me" 显式触发；模糊任务自动进入。---

## grill-with-docs 蒸馏（有状态变体，追加自原版 engineering/grill-with-docs）

> 来源: docs/engineering/grill-with-docs.md + skills/engineering/grill-with-docs/{SKILL.md,agents/openai.yaml}
> 定位: 家族中**唯一有状态**的。grill-me/grilling 把 session 留在对话/脑子里；它把文件写进仓库。

### 何时用 / 选择矩阵
- 在仓库里的某一变更刚开始、词还没定 → `/grill-with-docs`（单会话工具，禁自动触发，词面 `/`）。
- 没 repo → **grill-me** | 一个会话能定的 repo 变更 → **grill-with-docs** | 大到跨多会话(greenfield/大feature) → **wayfinder**(按会话数分界) | 决策卡在别人脑子里 → **to-questionnaire** | repo 无任何 domain doc 也无特定功能 → 指向 repo 的 grill-with-docs 来 document。
- **前提**: SKILL.md 只有一行「Run /grilling + /domain-modeling」，所以**必须连装 grilling 与 domain-modeling 两个 skill**，否则不工作。

### 纸面产物（三类，不等价）
| resolved | 落点 |
|---|---|
| term（项目自己的词；解析瞬间即落） | `CONTEXT.md` 内联（多 context 仓库按根 `CONTEXT-MAP.md` 落到各 context 的 CONTEXT.md）；二者 lazy 创建 |
| 决策：**同时**满足反逆 / 无上下文即surprising / 真实trade-off 三关 | `docs/adr/` 的 ADR |
| 其余一切决定 | **只在会话里，不落盘** |

- `CONTEXT.md` 是**纯词汇表**：绝无实现细节/spec/scratch。ADR 三关同过才写，多数会话 0 ADR 是**符合设计**。
- 收获常=一份更锐的 glossary + 零 ADR；此量大多数同意只活在 context window → 会话原样交给 to-spec，不要清空。

### 坑（已实测上报）
1. **单写者假设**：两人团队 4 个月约 20% 合并 PR 有状态漂移，ADR 引用与 README 声称是最高漂移面——有状态产出要单人来策展。
2. **跑了却没产文件和 ADR**：常见是无东西够格(正常)；**真 bug**：在编排层(spec-driven wrapper/多 agent 框架/被当 pipeline 步骤)运行，文件写入半边**静默不发生**而访谈照跑——已上报未修，勿信输出，先查工作目录。
3. **一次问完无推荐且不提 CONTEXT.md**：两依赖没加载，agent 乱猜 grilling → 无分化问询；部分加载(grilling 有 domain-modeling 无)=好访谈无纸迹。该 skill 最常见问题，直接问 agent 加载了哪些 skill。
4. **"我其它决定呢？"**：glossary≠spec，多数答案不配 ADR，无 ledger 把 answer→spec/ticket/test 串起来，精确答案(排序保证/否定需求/数值默认)会被弱化 → 保留会话喂 to-spec、并以自己答案反读 spec。
5. 结尾消息开放不闭口（已知 edge）→ 小变更直接 to-implement，否则 to-spec。

### It's working if（验收）
- CONTEXT.md 会话**逐 term 实时变化**，非最后成一块；无 DR。
- glossary 纯词汇（项目词+紧定义），无实现/spec 味。
- 代码能答的问题由**读代码**答出，不问你。
- 极少或 0 ADR，有的是你"宁可不用重新议"的决定。
- 它**挑战**你用词（因现有 glossary 定义不同）。

### 在构建链的位置
`grill-with-docs → to-spec → to-tickets → implement → code-review`（在一切写成 spec 前，产出共享理解与定词，让 to-spec 不用再访谈）。

### GA 集成
- 有 repo 且在变更/文档对齐任务：先 animals 出概要上下文，用 toast-grill-with-docs 流程吃 CONTEXT.md 词表+ADRs 门控；无 repo/无文件 → 走 grill-me(striotateless)。
- 记忆即文件，让 session 的"词汇与硬决策"沉淀成可检索 repo 文档，符合 GA 记忆分层。