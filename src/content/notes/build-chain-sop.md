---
title: "build_chain - 开发全链路方法论"
category: "AI 开发方法论"
description: "从想法到代码落地的完整开发链路编排（蒸馏自 mattpocock 主流程）。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["build-chain", "workflow", "plan"]
---

# build_chain_sop — 想法→交付 的全链路（Morphling 蒸馏自 mattpocock/skills 主流程）

> 来源（原版 docs+SKILL.md 实读): skills/engineering/{ask-matt,wayfinder,to-spec,to-tickets,implement,code-review,domain-modeling,triage,improve-codebase-architecture} + productivity/{grilling,grill-me,grill-with-docs,to-questionnaire}。
> 关系: 这是把 grill 家族（已独立入 grilling_sop.md）接上"从想法到代码落地"的其余环节的**主链编成**。grilling_sop 已含单个询问题；本 SOP 讲各环节如何衔接与纪律。

## 0. 路由（原版 ask-matt）——先定走哪条
- 有 repo 且在改 → `grill-with-docs`（有态, 落 CONTEXT/ADR）；无 repo / 纯会话 → `grill-me`。两者同一 grilling primitive，有库时总是用有态版。
- 卡在「对方脑袋」的知识 → `to-questionnaire`（非 grill）。
- 大到跨多会话 → `wayfinder`（map of 决策工单）；已议完只差落地 → 跳过询/映射直接 to-spec。某个粗想法有没有写 → grill。
- 外部灌进 bug/reports → `triage`（只对非本人创建的）。借 bug 根因 → `diagnosing-bugs`。整体架构漂移 → `improve-codebase-architecture`(周期性维护)。

## 主链（理想干线）：idea → ship
```
[大·雾]wayfinder ─┐
[可单会话]grill-with-docs │
[问完写spec]to-spec ─→ to-tickets(切 tracer→blocking) ─→ implement(每票) ─→ code-review(双轴)
[决策在别处]to-questionnaire ─┘
纪律层(ddomain-modeling: CONTEXT.md 词汇 + ADR) 全程在跑
```

### ① 对齐（grill 家族，见 grilling_sop.md)
单会话模式：整批问 frontier，❓/➡️ 格式，事实/决策分离。

### ② to-spec（把议定义成可建 spec）
- 开始*写作*而非再访谈；引用已决，不虚构 decision。先给 seams（接缝）。
- **out-of-scope 必须有实数**——被拒绝的清单最有价值。
- spec 是给 agent 读的：显式 dense, 引用居多；人只看 seams+out-of-scope。
- spec 是可替换的"快照"，ship 后即 stale——真正耐久的是 CONTEXT.md + ADRs。
- 坑：大 spec 会截断，`to-spec` 与 `to-tickets` 要同一窗口连跑、别 clear；别在同一入库重复开票 → 先自查 busy 区域。AFK 会把 parent spec 当工作单（最高频 bug）→ 明确排除或用完后去 label。

### ③ to-tickets（切成 tracer-bullet 工单 + blocking 边）
- target：**垂直**切片（一条窄但穿透 schema/API/UI/tests 的完整通路），可 demo/可自行验证，适配单一新鲜上下文窗口。非横向切一层。
- 每票声明 **blocking edges**（谁先完成它才开工）；无 blocker 的可立即开始。
- 宽重构是例外：单一机械改动打穿全库一票崩多调用点。用 **expand–contract**：先加新形式傍旧（不破坏）→ 按 blast radius 分批迁移（每批一票、bl乘 expand）→ 全 caller 离后删旧形式（contract 票 blocker=所有迁移票）。CI 逐批绿靠旧态仍存。
- 发布：本地 `/.scratch/<slug>/issues/<NN>-<slug>.md`（01 起、依赖排座 blocker 先）；真实 tracker per-issue、原生 blocking/sub-issue 或"Blocked by"列号，打 `ready-for-agent` 标签。
- 先 `quiz 用户`：以编号 list 展示 Title / Blocked by / What it delivers，问 granularity、blocking 是否对、可否合并拆。迭代到批准。
- 不改父 issue。frontier = 所有 blockers 已完成的可抓票。

### 4 implement（每票一步，可信上游不重验）
- 开工先**重述**要 build 什么（从 ticket/spec），不是问"做什么"。（坑：`#2` 解析在新会话可能指 todo/checklist，务必传全引用 URL 或 owner/repo#2 并回读 title）
- 对每个 seam 跑 **TDD**（红-绿），typecheck+单测反复跑，末尾整库全测一次。
- **只做一票宽度**的 diff；会自行提交到当前分支（无 PR 模式，需显式 override "commit+开PR"）。
- 信任上游：不重新校验 spec 形状（坏结构照建）；wayfinder 地图应先转 spec 再 implement。
- **绝不可并行多 implement 会话**（共享 cwd/index/HEAD：amend 交叉、stash 消失、签错分支）。要并行自行建 git worktrees。
- 一票烧 150k tokens 是拐状，正常——上游到 to-tickets 里 right-size 每票装进一窗口。
- 提交前内部跑 code-review；提交后 review。审查 diff=`git diff <fixed-point>...HEAD`，不含 staged/working-tree。

### 5 code-review（双轴、永不合并）
- 两个 sub-agent 各自独立评，结果两个块 `## Standards`(built right) 与 `## Spec`(right thing)，**绝不合并、不重排、不选单赢家**（一边过一边败时合并会让赢轴掩盖败轴）。
- Standards：读 repo 文档(CODING_STANDARDS/CONTRIBUTING),缺时 fallback 12 Fowler smells 基线；每 finding 引用标准文件+规则或 named smell+hunk；linter 已管的跳过。**repo 总是领先**基线。
- Spec：找 spec（commit 引用 issue → 传入路径 → docs/specs/.scratch 匹配 branch）→ 对比逐行要求；无 spec 则**注明 "no spec available" 而非凭空造需求**。
- 必须先给 fixed-point：ref 解析失败/空 diff → 拒绝起步（勿在两个 sub-agent 里才炸）。
- 产出格式：`## Standards` / `## Spec` 两块，每轴 worst issue，结尾拒绝整体 winner。

### 7. 纪律层 domain-modeling（全程伴随）
- 主动打磨领域模型：Challenge 与 CONTEXT.md 冲突的用词；模糊词提出精确规范语（account→Customer/User?）；用 concrete 场景压边界；与代码交叉核对矛盾并当场场 surfacing。
- 单 context：`/CONTEXT.md`(纯词汇, 无实现细节) + `docs/adr/NNNN-*.md`。多 context：根 `CONTEXT-MAP.md` 指向各处（系统级 ADR 在根 docs, 各 context 在 src/<ctx>/docs/adr）。**懒创建**：有首个 term 才建 CONTEXT，有首个 ADR 才建 adr/。
- ADR 只建当**三条件全真**：①难撤销(改了代价重) ②无上下文会惊（后人问"为何这样"）③真 trade-off（有真备择并选一）。缺一条→不建。
- CONTEXT.md inline 即时更新，不批量。它**只是词汇表**，不是 spec/scratch。

### 6. on-ramps
- **triage**（外部原报告，非 to-tickets 已产出的）：`bug`/`enhancement` 分类 + 唯一状态机 `needs-triage→needs-info→ready-for-agent→ready-for-human` 或 `wontfix`。改票前**先 verify**（按 reporter 复现/检分支跑测；查 redundancy 与 `.out-of-scope/` 曾拒绝）。`ready-for-agent` 附 **agent brief**：标注类型/签名/行为契约（非文件路径），是契约，原 report 仅 context。拒绝 enhancement 写 `.out-of-scope/<concept>.md`（按概念去重匹配，如 night↔dark-mode）。
- **improve-codebase-architecture**：周期性（每几天）维护、非链中：扫出**候选**(以领域概念命名，非 FooBarHandler；聚类在你刚编辑的文件) → 出 HTML 报告到 temp → **不改代码** → 停下问你选哪个 → 选中的进 grill/to-spec; rejected 可记 ADR 防重提。强制 **单候选/会话**；多候选等其他放票。无共享词表时先 grill-with-docs 建立 √ 词汇胜独立其输出。AI 不知它会说"代码没问题"，全靠 Speculative 强度徽来暗示。

## 全局纪律（跨环节）
- **单上下文可连到 smart zone(~150k)**：1–3 步同一窗口不 clear，直到 after to-tickets；approaching 边界在 phase boundary 用 compact 不硬推。
- **禁并发 implement**（上）；one invoke one ticket；worktree 自行。
- **frontier 驱动**：永远做"blockers 全完"的最前方票。
- 验收通用：每环节 ends by **停下问**而不自动继续；所有反馈能被下游验证（下 段消费上 段产物而不是重访谈）。

## GA 集成映射
- GA 已有 goal_hive/subagent/checklist 可承载 to-dos；本 SOP 补的是**纪律**：tracer-bullet 垂直切片、blocking 边与 frontier、TDD seam 循环、双轴独立 review、领域词表/ADR 落地。
- 直接落地点：GA 谈大改造先过 ①②（grill + spec + out-of-scope 实列）；拆工单严守 ③ 的垂直+blocking；实施每票 ④ 独立新鲜上下文；代码整改一律 ⑤ 双轴 review 不合并、无凭据不造需求。
- 巡仓库架构/长期维护用 ⑥ improve-codebase 流程。