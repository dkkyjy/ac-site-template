---
title: "Matt Pocock Skills - 完整用法与蒸馏指南"
category: "AI 开发方法论"
description: "Matt Pocock skills 的完整总结、用法与 GA 蒸馏/映射指南（22 技能详解、用法、Build Chain 主链编排、GA 映射）。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["mattpocock", "skills", "build-chain", "workflow"]
---

# mattpocock/skills — 完整总结与用法指南

> 仓库: https://github.com/mattpocock/skills
> 作者: Matt Pocock (@mattpocock)
> 星标: ~191k ⭐
> 许可证: MIT
> 核心定位: AI Coding Agent 技能库（Claude Code / Codex）

---

## 一、仓库概述

一个**AI Agent 技能集合仓库**，包含 41 个提示词技能（SKILL.md），专为 Claude Code 和 Codex 等 AI 编码代理设计。每个技能定义一种可预测的工作模式，让 AI 代理能以一致、高质量的方式完成特定任务。

### 核心理念

1. **可预测性（Predictability）** — 每个技能有明确的步骤和完成标准，代理每次按同样的过程执行
2. **可组合性（Composability）** — 技能之间通过"路由技能"（ask-matt）相互调用
3. **渐进揭露（Progressive Disclosure）** — 先让用户选择技能，再由技能驱动具体执行细节
4. **技能即文档（Skills as Docs）** — 技能同时是代理的"知识手册"，记录持续更新的领域知识

---

## 二、仓库结构

```
mattpocock/skills/
├── CLAUDE.md                 # 项目入口 — 路由技能选择器
├── AGENTS.md                 # 技能注册/路由配置
├── CONTEXT.md                # 领域术语表（GLOSSARY）
├── README.md                 # 项目介绍
├── .claude-plugin/           # Claude Code 插件注册
│   ├── plugin.json           # 插件清单
│   └── marketplace.json      # 市场信息
├── .out-of-scope/            # 明确定义的非目标
├── docs/                     # 文档
├── scripts/
│   ├── link-skills.sh        # 技能安装脚本
│   └── list-skills.sh        # 技能列表查看
├── skills/
│   ├── engineering/          # 🔵 ⭐ 工程技能（编码核心）
│   ├── productivity/         # 🟢 ⭐ 生产力技能（工作流）
│   ├── misc/                 # ⚪ 杂项（少用）
│   ├── personal/             # 🔴 个人绑定技能
│   ├── in-progress/          # 🟡 草稿/开发中
│   └── deprecated/           # ⚫ 不再使用
└── .changeset/               # 版本发布管理
```

### 技能目录（bucket）说明

| Bucket | 标记 | 说明 |
|--------|------|------|
| **engineering/** | 🔵 promoted | **日常编码技能** — 22 个核心，每天使用 |
| **productivity/** | 🟢 promoted | **工作流技能** — 文档/沟通/决策 |
| **misc/** | ⚪ | 不常用，特定场景 |
| **personal/** | 🔴 | 作者个人定制，不可移植 |
| **in-progress/** | 🟡 | 草稿/实验性 |
| **deprecated/** | ⚫ | 不再维护 |

---

## 三、22 个 Promoted 技能详解

### 🔵 Engineering（工程技能）

| 技能 | 触发词 | 功能 |
|------|--------|------|
| **code-review** | review a diff | 按标准和规格审查代码差异 |
| **debug** | debug this | 系统化调试：重现→隔离→修 |
| **implement** | implement this | 根据 spec 实现功能 |
| **refactor** | refactor this | 安全重构现有代码 |
| **research** | research something / look up / investigate | 查阅高信任度源并产出 Markdown 文档 |
| **solve** | solve this bug / help with this | 解决已知 bug 或配置问题 |
| **tdd** | tdd this | 红-绿-重构 TDD 循环 |
| **type-heal** | type-heal / fix types | 系统化修复 TypeScript 类型错误 |
| **write-tests** | write tests for this | 为已有函数编写测试 |

### 🟢 Productivity（生产力技能）

| 技能 | 触发词 | 功能 |
|------|--------|------|
| **grill-me** | grill me | 启动 grilling（需求追问） |
| **grilling** | （模型自动调用） | 面试式追问直到设计树完 |
| **batch-grill-me** | （实验性） | 轮次批量追问（效率更高） |
| **choose** | choose between X and Y | 比较两个选项并决策 |
| **handoff** | handoff / compact | 压缩对话为移交文档 |
| **plan** | make a plan / write a spec | 产出带结构的计划 |
| **teach** | teach me about X | Socratic 教学 |
| **writing-great-skills** | （禁用模型调用） | 技能写作元参考 |
| **write-doc** | write docs for this | 为代码编写文档 |

### 其他技能

| 技能 | Bucket | 功能 |
|------|--------|------|
| summarise-chat | misc | 总结聊天内容 |
| create-release-note | misc | 创建发布说明 |
| find-by-pattern | misc | 按模式搜索代码 |
| domain-modeling | personal | 领域建模 |
| generate-pr-summary | personal | PR 摘要生成 |
| glossary | personal | 术语表服务 |

---

## 四、用法指南

### 4.1 安装

对于 Claude Code：

```bash
# 克隆仓库
git clone https://github.com/mattpocock/skills.git

# 安装技能（创建符号链接到 ~/.claude/skills/）
cd skills
bash scripts/link-skills.sh

# 列出已安装技能
bash scripts/list-skills.sh
```

更新技能：
```bash
cd /path/to/skills
git pull
bash scripts/link-skills.sh   # 重新建立符号链接
```

### 4.2 技能格式（SKILL.md）

每个技能是一个 Markdown 文件，包含：

```markdown
---
name: skill-name              # 技能唯一标识
description: 一句话描述       # 说明技能用途
disable-model-invocation: true  # （可选）禁止模型自动调用
---

# 技能正文

## 触发条件
在什么情况下用户会激活此技能

## 流程
1. 第一步...
2. 第二步...
3. 直到完成标准...

## 完成标准
- [ ] 条件1
- [ ] 条件2
```

可选附加文件：
- `agents/openai.yaml` — OpenAI/Codex 的配置/描述
- 脚本/模板/示例

### 4.3 交互模式

**用户启动型（User-invoked）**：用户用自然语言触发
- "review this diff" → code-review
- "write tests for this" → write-tests
- "grill me" → grill-me

**模型启动型（Model-invoked）**：AI 根据上下文自动选择
- grilling（检测到需求模糊时自动追问）
- research（检测到需要事实查证时）

**禁用模型调用（`disable-model-invocation: true`）**：
- 仅用户可直接引用（如 writing-great-skills）

### 4.4 核心工作流

```
用户输入模糊任务
    │
    ▼
[CLAUDE.md] 入口路由
    │
    ├─ 明确 → 直接执行指定技能
    ├─ 模糊 → 触发 grilling（追问需求）
    │              │
    │              ▼
    │         轮次追问直到设计树收敛
    │              │
    │              ▼
    └─ 需求明确后 → 执行实现/代码技能
                        │
                        ▼
                   完成 → 可选 handoff（写移交文档）
```

### 4.5 CLI 用法

```bash
# 列出所有技能
bash scripts/list-skills.sh

# 查看技能详情
cat skills/engineering/code-review/SKILL.md

# 安装新编写的技能
bash scripts/link-skills.sh
```

---

## 五、典型技能速查

### code-review（代码审查）

审查双维度标准：
| 维度 | 关注点 |
|------|--------|
| **审查者标准（Reviewer Standards）** | 代码正确性、测试覆盖、性能、安全性 |
| **当前规范（Current Spec）** | 功能/架构/体验是否符合要求 |

运行步骤：
1. 读取 diff 和 spec
2. Checkout 到目标分支
3. 跑测试确保基础通过
4. 对每个文件逐维度审查
5. 按严重性（critical/major/minor/nit）分类
6. 统计 checkpoint — 计算能检测到的问题中实际发现的比例

### handoff（移交）

当任务复杂/多轮时，生成移交文档让下个会话能无缝继续：
1. 列出已做和待做的事情
2. 标记关键决策点和未解决的分歧
3. 提供线索（文件路径/参考）
4. 避免重复已有内容（引用而非复制）

### research（研究）

1. 确认研究目标
2. 查阅高信任源（官方文档/源码/权威参考）
3. 在 `/tmp/` 创建临时 Markdown
4. 组织结论供用户审查
5. 用户确认后移入最终位置

### writing-great-skills（技能写作）

**核心原则**：
- 每个技能解决一个可预测的问题
- 信息层次：路径 > 代码/命令 > 描述
- 召唤模式：让模型易于匹配触发条件
- 完成标准：明确定义"什么算完成"
- 渐进步进：从用户角度逐步推进
- 不重复已有内容：引用而非复制

---

## 六、关键配置文件

### CLAUDE.md（入口路由）

项目根目录的 `CLAUDE.md` 是 Claude Code 的入口文件，主要职责：
1. 引导用户从技能列表中选择
2. 根据用户输入的路由到对应技能
3. 维护路由表（技能路径 → 技能名称）

### AGENTS.md

技能注册配置，告诉 Claude 工程中有哪些技能可用。

### CONTEXT.md

项目领域术语表（GLOSSARY），解决代理和领域专家之间的"语言分歧"，定义关键术语的精确含义。

### plugin.json（Claude Code 插件注册）

```json
{
  "name": "mattpocock-skills",
  "version": "1.2.0",
  "description": "Matt Pocock's agent skills for Claude",
  "skills": ["code-review", "implement", "refactor", "debug", "tdd", ...]
}
```

---

## 七、设计哲学

### 7.1 技能是可预测的脚本

> "A skill exists to wrangle the conversation into a predictable pattern so the agent can repeat it."

每个技能本质上是一个可重复的过程模板，让代理不仅"知道做什么"，还"知道怎么做"。

### 7.2 三层信息层次

1. **路径（Path）** — 告诉代理去拿什么文件（最高效）
2. **代码/命令（Code/Commands）** — 具体执行内容
3. **描述（Description）** — 仅当路径和代码不够时使用

### 7.3 渐进揭露

- 先问基本方向（"要实现什么？"）
- 再揭露技能选择列表
- 用户选技能后进入执行细节
- 一次只提必要的选项

### 7.4 完成标准

每个技能必须定义明确的完成条件：
- **可验证**：能客观判断是否完成
- **无歧义**：不用"感觉"或"差不多"
- **用户确认**：最终需要用户点头

---

## 八、使用技巧

1. **日常组合**：处理 bug 时先用 `debug` 分析，再用 `implement` 修复，最后 `code-review` 确认
2. **模糊需求**：不确定时先 `grill me` 追问，再用 `plan` 写 spec
3. **多轮对话**：换话题前先用 `handoff` 打包当前进展
4. **学习新东西**：`teach me about X` 触发 Socratic 教学
5. **代码质量**：`code-review` + `write-tests` 组合确保质量
6. **持续更新**：`git pull` + `bash scripts/link-skills.sh` 保持技能最新

---

## 九、与本仓库（GenericAgent）的映射

| Matt Pocock Skills | GenericAgent | 说明 |
|-------------------|-------------|------|
| SKILL.md | `memory/*_sop.md` | GA 的 SOP 是其"技能" |
| CLAUDE.md 路由 | `memory/global_mem_insight.txt` | GA 的 L1 导航 |
| skills/engineering/ | `memory/*_sop.md` (工程类) | code-review, verify 等 |
| skills/productivity/ | `memory/*_sop.md` (生产力类) | goal_hive, checklist 等 |
| `grill-me` | `memory/grilling_sop.md` | 已移植 |
| `writing-great-skills` | `memory/mattpocock_skills_sop.md` | 已蒸馏 |
| agents/openai.yaml | （GA 无对应） | GA 不依赖 OpenAI/Codex |
| .claude-plugin/ | （GA 无对应） | GA 自有激活机制 |

---

*最后更新: 2026-07-28*
*基于仓库 commit: main branch (depth 1 clone)*


---

## 十、参考：Morphling 蒸馏补充

### 3.5 分支（Branching）（Branching）

技能可能有多条执行路径。分支是最干净的渐进揭露测试：
- 内联所有分支都需要的内容
- 指针引用只有部分分支需要的内容

## 5. 与 GA 系统的映射关系

| Matt Pocock 概念 | GA 等价物 | 差异 |
|-----------------|-----------|------|
| SKILL.md + frontmatter | memory/*.md SOP | GA 的 SOP 缺少 frontmatter（name/description/invocation） |
| Model-invoked vs User-invoked | system prompt vs user prompt | GA 隐性区分，未显式声明 |
| Information hierarchy | SOP 内的分级 | GA 隐式使用但未概念化 |
| Context pointer | 文件内的引用/footnote | 概念一致 |
| Completion criterion | checklist_sop 中的验证点 | GA 已有但更松散 |
| Plugin/registry | memory/*.md 文件系统 | GA 更简单：文件即技能 |
| Progress disclosure | — | GA 未显式使用此模式 |
| Router skill | — | GA 暂无对应概念 |

## 6. 可移植的高价值技能（已适配 GA 格式）

以下技能已从原仓库提取核心模式，适配为 GA 可用的参考模式：

### 6.1 Grill-Me（追问）
**模式**: 遍历决策树，每轮问所有已满足前提的决策，然后从答案重新计算。
**GA 应用**: 在开放式任务开始时使用，对齐需求。

### 6.2 Handoff（交接）
**模式**: 压缩当前会话为交接文档，包含进度、下一步、推荐技能。
**GA 应用**: 任务中断/切换时，生成交接摘要。

### 6.3 Research（研究）
**模式**: 启动后台 agent 调查问题，只信任一手来源。
**GA 应用**: 复杂调研任务，使用 web 搜索 + 文档阅读。

### 6.4 Code Review（代码审查-双轴）
**模式**: 分"标准合规"和"规格一致"两个独立轴审查，不合并排序。
**GA 应用**: 已有 `opencodereview_sop.md`，可借鉴双轴分离原则。

### 6.5 Domain Modeling（领域建模）
**模式**: 主动建立领域术语表 + ADR，每个术语定义清晰边界。
**GA 应用**: 新项目启动时，先构建 CONTEXT.md。

## 7. 对 GA 技能写作的建议

基于 writing-great-skills 的核心理念：

1. **每个 SOP 应有 frontmatter**: name / description / invocation type
2. **步骤必须有完成标准**: 可检验的条件，避免过早完成
3. **信息分层**: 核心步骤在顶层，参考内容推到外部
4. **分支处理**: 不同使用路径用上下文指针隔离
5. **model-invoked vs user-invoked**: 显式声明谁可调用
6. **减少重复**: description 中不同触发词应代表真正不同的分支

## 8. 对照验证

| 维度 | 原项目 | Mrophling 输出 |
|------|--------|---------------|
| 技能数量 | 41 | —（参考模式） |
| 架构文档 | CLAUDE.md + README | 本 SOP |
| 安装方式 | skills.sh / Claude plugin | GA 的工作记忆注册 |
| 验证工具 | claude plugin validate | GA CLI 已内置 |
| 发布渠道 | Claude Code marketplace | —（GA 内部使用） |

## 9. 边界与免责

- 未搬运的具体 SKILL.md 内容仍属原作者（MIT 许可证）
- 本 SOP 仅提取**架构模式和写作方法论**
- 部分技能（personal/, misc/）因与 GA 无关而舍弃
- Claude Code / Codex 特定插件配置因 GA 不依赖而舍弃

---

## 十一、主链编排 Build Chain（想法→交付）

> 以下内容原为独立笔记 `build-chain-sop.md`（2026-08-10），已整合于此。
> 关系：这是把 grill 家族（已独立入 `grilling_sop.md`）接上「从想法到代码落地」的其余环节的**主链编成**。`grilling_sop.md` 已含单个询问题；本 SOP 讲各环节如何衔接与纪律。

## Matt Pocock Build Chain — 想法→交付 的全链路（Morphling 蒸馏自 mattpocock/skills 主流程）

> 来源（原版 docs+SKILL.md 实读): skills/engineering/{ask-matt,wayfinder,to-spec,to-tickets,implement,code-review,domain-modeling,triage,improve-codebase-architecture} + productivity/{grilling,grill-me,grill-with-docs,to-questionnaire}。
> 关系: 这是把 grill 家族（已独立入 grilling_sop.md）接上"从想法到代码落地"的其余环节的**主链编成**。grilling_sop 已含单个询问题；本 SOP 讲各环节如何衔接与纪律。

### 0. 路由（原版 ask-matt）——先定走哪条
- 有 repo 且在改 → `grill-with-docs`（有态, 落 CONTEXT/ADR）；无 repo / 纯会话 → `grill-me`。两者同一 grilling primitive，有库时总是用有态版。
- 卡在「对方脑袋」的知识 → `to-questionnaire`（非 grill）。
- 大到跨多会话 → `wayfinder`（map of 决策工单）；已议完只差落地 → 跳过询/映射直接 to-spec。某个粗想法有没有写 → grill。
- 外部灌进 bug/reports → `triage`（只对非本人创建的）。借 bug 根因 → `diagnosing-bugs`。整体架构漂移 → `improve-codebase-architecture`(周期性维护)。

### 主链（理想干线）：idea → ship
```
[大·雾]wayfinder ─┐
[可单会话]grill-with-docs │
[问完写spec]to-spec ─→ to-tickets(切 tracer→blocking) ─→ implement(每票) ─→ code-review(双轴)
[决策在别处]to-questionnaire ─┘
纪律层(ddomain-modeling: CONTEXT.md 词汇 + ADR) 全程在跑
```

#### ① 对齐（grill 家族，见 grilling_sop.md)
单会话模式：整批问 frontier，❓/➡️ 格式，事实/决策分离。

#### ② to-spec（把议定义成可建 spec）
- 开始*写作*而非再访谈；引用已决，不虚构 decision。先给 seams（接缝）。
- **out-of-scope 必须有实数**——被拒绝的清单最有价值。
- spec 是给 agent 读的：显式 dense, 引用居多；人只看 seams+out-of-scope。
- spec 是可替换的"快照"，ship 后即 stale——真正耐久的是 CONTEXT.md + ADRs。
- 坑：大 spec 会截断，`to-spec` 与 `to-tickets` 要同一窗口连跑、别 clear；别在同一入库重复开票 → 先自查 busy 区域。AFK 会把 parent spec 当工作单（最高频 bug）→ 明确排除或用完后去 label。

#### ③ to-tickets（切成 tracer-bullet 工单 + blocking 边）
- target：**垂直**切片（一条窄但穿透 schema/API/UI/tests 的完整通路），可 demo/可自行验证，适配单一新鲜上下文窗口。非横向切一层。
- 每票声明 **blocking edges**（谁先完成它才开工）；无 blocker 的可立即开始。
- 宽重构是例外：单一机械改动打穿全库一票崩多调用点。用 **expand–contract**：先加新形式傍旧（不破坏）→ 按 blast radius 分批迁移（每批一票、bl乘 expand）→ 全 caller 离后删旧形式（contract 票 blocker=所有迁移票）。CI 逐批绿靠旧态仍存。
- 发布：本地 `/.scratch/<slug>/issues/<NN>-<slug>.md`（01 起、依赖排座 blocker 先）；真实 tracker per-issue、原生 blocking/sub-issue 或"Blocked by"列号，打 `ready-for-agent` 标签。
- 先 `quiz 用户`：以编号 list 展示 Title / Blocked by / What it delivers，问 granularity、blocking 是否对、可否合并拆。迭代到批准。
- 不改父 issue。frontier = 所有 blockers 已完成的可抓票。

#### 4 implement（每票一步，可信上游不重验）
- 开工先**重述**要 build 什么（从 ticket/spec），不是问"做什么"。（坑：`#2` 解析在新会话可能指 todo/checklist，务必传全引用 URL 或 owner/repo#2 并回读 title）
- 对每个 seam 跑 **TDD**（红-绿），typecheck+单测反复跑，末尾整库全测一次。
- **只做一票宽度**的 diff；会自行提交到当前分支（无 PR 模式，需显式 override "commit+开PR"）。
- 信任上游：不重新校验 spec 形状（坏结构照建）；wayfinder 地图应先转 spec 再 implement。
- **绝不可并行多 implement 会话**（共享 cwd/index/HEAD：amend 交叉、stash 消失、签错分支）。要并行自行建 git worktrees。
- 一票烧 150k tokens 是拐状，正常——上游到 to-tickets 里 right-size 每票装进一窗口。
- 提交前内部跑 code-review；提交后 review。审查 diff=`git diff <fixed-point>...HEAD`，不含 staged/working-tree。

#### 5 code-review（双轴、永不合并）
- 两个 sub-agent 各自独立评，结果两个块 `## Standards`(built right) 与 `## Spec`(right thing)，**绝不合并、不重排、不选单赢家**（一边过一边败时合并会让赢轴掩盖败轴）。
- Standards：读 repo 文档(CODING_STANDARDS/CONTRIBUTING),缺时 fallback 12 Fowler smells 基线；每 finding 引用标准文件+规则或 named smell+hunk；linter 已管的跳过。**repo 总是领先**基线。
- Spec：找 spec（commit 引用 issue → 传入路径 → docs/specs/.scratch 匹配 branch）→ 对比逐行要求；无 spec 则**注明 "no spec available" 而非凭空造需求**。
- 必须先给 fixed-point：ref 解析失败/空 diff → 拒绝起步（勿在两个 sub-agent 里才炸）。
- 产出格式：`## Standards` / `## Spec` 两块，每轴 worst issue，结尾拒绝整体 winner。

#### 7. 纪律层 domain-modeling（全程伴随）
- 主动打磨领域模型：Challenge 与 CONTEXT.md 冲突的用词；模糊词提出精确规范语（account→Customer/User?）；用 concrete 场景压边界；与代码交叉核对矛盾并当场场 surfacing。
- 单 context：`/CONTEXT.md`(纯词汇, 无实现细节) + `docs/adr/NNNN-*.md`。多 context：根 `CONTEXT-MAP.md` 指向各处（系统级 ADR 在根 docs, 各 context 在 src/<ctx>/docs/adr）。**懒创建**：有首个 term 才建 CONTEXT，有首个 ADR 才建 adr/。
- ADR 只建当**三条件全真**：①难撤销(改了代价重) ②无上下文会惊（后人问"为何这样"）③真 trade-off（有真备择并选一）。缺一条→不建。
- CONTEXT.md inline 即时更新，不批量。它**只是词汇表**，不是 spec/scratch。

#### 6. on-ramps
- **triage**（外部原报告，非 to-tickets 已产出的）：`bug`/`enhancement` 分类 + 唯一状态机 `needs-triage→needs-info→ready-for-agent→ready-for-human` 或 `wontfix`。改票前**先 verify**（按 reporter 复现/检分支跑测；查 redundancy 与 `.out-of-scope/` 曾拒绝）。`ready-for-agent` 附 **agent brief**：标注类型/签名/行为契约（非文件路径），是契约，原 report 仅 context。拒绝 enhancement 写 `.out-of-scope/<concept>.md`（按概念去重匹配，如 night↔dark-mode）。
- **improve-codebase-architecture**：周期性（每几天）维护、非链中：扫出**候选**(以领域概念命名，非 FooBarHandler；聚类在你刚编辑的文件) → 出 HTML 报告到 temp → **不改代码** → 停下问你选哪个 → 选中的进 grill/to-spec; rejected 可记 ADR 防重提。强制 **单候选/会话**；多候选等其他放票。无共享词表时先 grill-with-docs 建立 √ 词汇胜独立其输出。AI 不知它会说"代码没问题"，全靠 Speculative 强度徽来暗示。

### 全局纪律（跨环节）
- **单上下文可连到 smart zone(~150k)**：1–3 步同一窗口不 clear，直到 after to-tickets；approaching 边界在 phase boundary 用 compact 不硬推。
- **禁并发 implement**（上）；one invoke one ticket；worktree 自行。
- **frontier 驱动**：永远做"blockers 全完"的最前方票。
- 验收通用：每环节 ends by **停下问**而不自动继续；所有反馈能被下游验证（下 段消费上 段产物而不是重访谈）。

### GA 集成映射
- GA 已有 goal_hive/subagent/checklist 可承载 to-dos；本 SOP 补的是**纪律**：tracer-bullet 垂直切片、blocking 边与 frontier、TDD seam 循环、双轴独立 review、领域词表/ADR 落地。
- 直接落地点：GA 谈大改造先过 ①②（grill + spec + out-of-scope 实列）；拆工单严守 ③ 的垂直+blocking；实施每票 ④ 独立新鲜上下文；代码整改一律 ⑤ 双轴 review 不合并、无凭据不造需求。
- 巡仓库架构/长期维护用 ⑥ improve-codebase 流程。
