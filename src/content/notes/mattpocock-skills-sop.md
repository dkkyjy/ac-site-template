---
title: "Matt Pocock Skills - 蒸馏总结"
category: "Matt Pocock"
description: "Matt Pocock 40+ AI Coding Agent 技能库蒸馏，核心价值与可组合路由。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["mattpocock", "skills", "agent"]
---

# Matt Pocock Skills — Morphling 蒸馏成果

> 目标仓库: https://github.com/mattpocock/skills
> 作者: Matt Pocock (@mattpocock)
> 类型: AI Agent Skill 库（Claude Code / Codex）
> 星标: ~191k ⭐

## 1. 核心价值

此仓库是一套**AI Coding Agent 技能库**，40+ 个技能覆盖日常编码和生产力工作流。核心价值：

1. **可预测性（Predictability）** — 每个技能定义明确的步骤和完成标准，让 agent 每次执行相同的过程
2. **可组合性（Composability）** — 小技能通过"路由技能"（ask-matt）组合
3. **渐进揭露（Progressive Disclosure）** — 信息分层，只暴露当前需要的细节
4. **双轴验证** — 代码审查分离"规范合规性"和"规格一致性"

## 2. 系统架构

### 2.1 目录结构

```
repo/
├── skills/                          # 技能目录
│   ├── engineering/                 # 工程技能（promoted）
│   │   ├── code-review/SKILL.md
│   │   ├── tdd/SKILL.md
│   │   ├── implement/SKILL.md
│   │   ├── research/SKILL.md
│   │   ├── to-spec/SKILL.md
│   │   ├── to-tickets/SKILL.md
│   │   ├── domain-modeling/SKILL.md
│   │   ├── wayfinder/SKILL.md
│   │   ├── ask-matt/SKILL.md        # 路由器技能
│   │   └── ...                      # 17个总技能
│   ├── productivity/                # 生产力技能（promoted）
│   │   ├── grill-me/SKILL.md
│   │   ├── grilling/SKILL.md
│   │   ├── handoff/SKILL.md
│   │   ├── teach/SKILL.md
│   │   └── writing-great-skills/SKILL.md
│   ├── misc/                        # 不常用（not promoted）
│   ├── personal/                    # 个人配置（not promoted）
│   ├── in-progress/                 # 草稿（not promoted）
│   └── deprecated/                  # 已废弃（not promoted）
├── .agents/                         # 元文档
│   ├── invocation.md                # 调用模式说明
│   └── writing-docs.md              # 文档写作指南
├── .claude-plugin/                  # Claude Code 插件配置
│   ├── plugin.json
│   └── marketplace.json
├── docs/                            # 用户文档页（对应 promoted skills）
│   ├── engineering/
│   └── productivity/
├── CLAUDE.md                        # 仓库级 CLAUDE 配置
├── CONTEXT.md                       # 领域术语表
├── scripts/
│   ├── link-skills.sh               # 安装技能
│   └── list-skills.sh               # 列出技能
└── README.md                        # 主索引
```

### 2.2 技能格式

每个技能包含：
- `SKILL.md` — 核心定义（必选）
- `agents/openai.yaml` — Agent 配置（可选）
- `scripts/` — 辅助脚本（可选）

### 2.3 SKILL.md 格式

```markdown
---
name: skill-name
description: 一句话描述（对人/对 agent 取决于 invocation 类型）
disable-model-invocation: true  # 仅用户调用；省略则 agent 也可调用
argument-hint: "参数提示"       # [可选] 用户输入参数提示
---

# 标题

技能正文...
```

### 2.4 agents/openai.yaml 格式

```yaml
interface:
  display_name: "技能显示名"
  short_description: "简短描述"
policy:
  allow_implicit_invocation: false  # 仅用户调用
```

## 3. 核心概念

### 3.1 调用模式（Invocation）

| 类型 | 配置 | 谁可调用 | 上下文成本 |
|------|------|---------|-----------|
| **Model-invoked** | 省略 `disable-model-invocation` | agent + 用户 | 高——description 每轮占用窗口 |
| **User-invoked** | `disable-model-invocation: true` | 仅用户 | 零——但消耗认知负荷（用户要记住技能存在） |

**路由器技能**: 当 user-invoked 技能数量超过用户记忆容量时，创建一个路由器技能（如 `ask-matt`）来索引所有技能。

### 3.2 信息层次（Information Hierarchy）

技能内容分三层，按可及性排序：

```
1. In-skill step      → 放在 SKILL.md 正文中的有序步骤
   （Primary tier）     每个步骤以完成标准结束
   
2. In-skill reference → SKILL.md 中的定义/规则/事实
   （On demand）        按需查阅，无顺序依赖
   
3. External reference → 推送到外部文件，通过上下文指针引用
   （Context pointer）  仅在指针触发时加载
```

**渐进揭露**: 常用内容留在顶层，少用内容推到外部文件。

### 3.3 完成标准（Completion Criterion）

每个步骤必须以**可检验的条件**结束：
- ✅ "每个已修改的模型都已记录"（可检查）
- ❌ "生成变更列表"（模糊，易过早完成）

### 3.4 分支（Branching）

技能可能有多条执行路径。分支是最干净的渐进揭露测试：
- 内联所有分支都需要的内容
- 指针引用只有部分分支需要的内容

## 4. 核心技能清单（promoted）

### Engineering（17个）

| 技能 | 类型 | 功能 |
|------|------|------|
| **ask-matt** | user-invoked | 路由器——索引并推荐其他技能 |
| **code-review** | model-invoked | 双轴代码审查：标准合规 + 规格一致 |
| **codebase-design** | model-invoked | 代码库设计评估 |
| **diagnosing-bugs** | model-invoked | Bug 诊断 |
| **domain-modeling** | model-invoked | 领域建模：术语表 + ADR |
| **grill-with-docs** | model-invoked | 带文档的设计追问 |
| **implement** | user-invoked | 从 spec/ticket 实现 |
| **improve-codebase-architecture** | model-invoked | 架构改进 |
| **prototype** | user-invoked | 原型开发 |
| **research** | model-invoked | 背景研究（后台 agent） |
| **resolving-merge-conflicts** | user-invoked | 解决合并冲突 |
| **setup-matt-pocock-skills** | user-invoked | 初始化配置 |
| **tdd** | user-invoked | 测试驱动开发 |
| **to-spec** | user-invoked | 从对话生成 spec |
| **to-tickets** | user-invoked | 从 spec 拆 ticket |
| **triage** | model-invoked | Issue 分类 |
| **wayfinder** | user-invoked | 大规模工作计划 |

### Productivity（5个）

| 技能 | 类型 | 功能 |
|------|------|------|
| **grill-me** | user-invoked | 追问设计（/grilling 的包装） |
| **grilling** | model-invoked | 追问——遍历决策树 |
| **handoff** | user-invoked | 会话交接文档 |
| **teach** | user-invoked | 多会话教学 |
| **writing-great-skills** | user-invoked | 技能写作元参考 |

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