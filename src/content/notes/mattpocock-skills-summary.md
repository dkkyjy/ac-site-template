---
title: "Matt Pocock Skills - 完整用法指南"
category: "技能库 / 框架"
description: "Matt Pocock skills 的完整总结与用法指南。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["mattpocock", "skills"]
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
