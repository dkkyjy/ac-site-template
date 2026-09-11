---
title: "Agent Skills (addyosmani) - 完整使用指南"
category: "技能库 / 框架"
description: "addyosmani/agent-skills 的完整总结：25 个生产级工程技能、9 个斜杠命令、4 个人格、安装方式与设计理念。"
pubDate: "2026-09-08"
badge: "guide"
tags: ["addy-osmani", "skills", "工程"]
---
# Addy Osmani / agent-skills 中文说明

> 仓库地址：https://github.com/addyosmani/agent-skills （版本 0.6.9）
> 一句话：**面向 AI 编码 Agent 的生产级工程技能包**——把资深工程师的开发流程、质量关卡与最佳实践编码成技能，让 Agent 在开发的每个阶段都一致地遵循。

---

## 1. 核心理念

AI 编码 Agent 天生走最短路径——经常跳过规格、测试、安全评审这些让软件可靠的实践。agent-skills 给 Agent 提供**结构化工作流**，强制它们像资深工程师一样自律。

- 技能不是"通用提示词"，而是**有观点、有流程**的工作流
- 融入了 Google 工程文化的最佳实践（《Software Engineering at Google》+ Google 工程实践指南）
- 具体体现在：API 设计里的 Hyrum's Law、测试里的 Beyonce Rule 与测试金字塔、代码评审里的变更规模（~100 行）与评审速度规范、简化里的 Chesterton's Fence、Git 的 trunk-based 开发、CI/CD 的 Shift Left 与 feature flags、以及"代码即负债"的废弃技能

---

## 2. 开发生命周期总览

```
  DEFINE          PLAN           BUILD          VERIFY         REVIEW          SHIP
 ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐      ┌──────┐
 │ Idea │ ───▶ │ Spec │ ───▶ │ Code │ ───▶ │ Test │ ───▶ │  QA  │ ───▶ │  Go  │
 │Refine│      │  PRD │      │ Impl │      │Debug │      │ Gate │      │ Live │
 └──────┘      └──────┘      └──────┘      └──────┘      └──────┘      └──────┘
  /spec          /plan          /build        /test         /review       /ship
```

6 个阶段、9 个斜杠命令，命令会自动激活对应的技能。

---

## 3. 9 个斜杠命令

| 你正在做的事 | 命令 | 核心理念 |
|---|---|---|
| 定义要做什么 | `/spec` | 先写规格，再写代码 |
| 规划怎么做 | `/plan` | 小颗粒原子任务 |
| 增量构建 | `/build` | 一次一个切片 |
| 证明它能工作 | `/test` | 测试就是证据 |
| 设定质量线 | `/constraints` | 一次性定标准，处处强制执行 |
| 合并前评审 | `/review` | 提升代码健康度 |
| 审计 Web 性能 | `/webperf` | 先测量再优化 |
| 简化代码 | `/code-simplify` | 清晰胜过炫技 |
| 上线交付 | `/ship` | 更快反而更安全 |

**自动化模式**：规格写好后，`/build auto` 一次生成计划并实现所有任务——你只审批一次计划，然后它自主运行。它消除的是"任务之间"的人工介入，**不是验证**：每个任务仍按测试驱动、逐个独立提交，遇到失败或风险步骤会暂停。

技能也会根据你正在做的事**自动激活**——设计 API 触发 `api-and-interface-design`，构建 UI 触发 `frontend-ui-engineering`，等等。

---

## 4. 25 个技能清单（按生命周期分组）

### DEFINE（定义）— 4 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| interview-me | 一次一问的需求访谈，挖出用户真正想要的（而非自以为想要的），直到 ~95% 置信度 | 需求不明，或用户说"interview me / grill me" |
| idea-refine | 结构化发散/收敛思维，把模糊想法变成具体提案 | 有粗略概念需要探索 |
| spec-driven-development | 写代码前先写 PRD：目标、命令、结构、代码风格、测试、边界 | 新项目、新功能、重大变更 |
| constraint-driven-development | 访谈式确定质量线（带合理默认阈值），写 CONSTRAINTS.md，按成本放置检查点，抓 Agent 静默关闭检查或跳过测试求绿 | 没有成文标准，或 Agent 产出没人看 |

### PLAN（规划）— 1 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| planning-and-task-breakdown | 把规格拆成小的、可验证的任务（含验收标准与依赖排序） | 有规格、需要可实施单元 |

### BUILD（构建）— 7 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| incremental-implementation | 薄垂直切片——实现、测试、验证、提交；feature flags、安全默认值、易回滚变更 | 任何改动超过一个文件 |
| test-driven-development | 红-绿-重构；测试金字塔（80/15/5）、测试规模、DAMP 优于 DRY、Beyonce Rule、浏览器测试 | 实现逻辑、修 bug、改行为 |
| context-engineering | 在正确时间喂给 Agent 正确信息——规则文件、上下文打包、MCP 集成 | 开新会话、切换任务、输出质量下降时 |
| source-driven-development | 每个框架决策都锚定官方文档——验证、引用来源、标记未验证项 | 想要权威、带引用的框架代码 |
| doubt-driven-development | 对每个在途非平凡决策做对抗式全新上下文评审（CLAIM→EXTRACT→DOUBT→RECONCILE→STOP），可选跨模型升级 | 高风险场景（生产/安全/不可逆）、陌生代码库、验证比日后调试便宜 |
| frontend-ui-engineering | 组件架构、设计系统、状态管理、响应式设计、WCAG 2.1 AA 无障碍 | 构建或修改用户界面 |
| api-and-interface-design | 契约优先、Hyrum's Law、One-Version Rule、错误语义、边界校验 | 设计 API、模块边界、公共接口 |

### VERIFY（验证）— 2 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| browser-testing-with-devtools | Chrome DevTools MCP 拿实时运行时数据——DOM 检查、控制台日志、网络跟踪、性能剖析 | 构建或调试任何浏览器里跑的东西 |
| debugging-and-error-recovery | 五步分诊：复现、定位、缩减、修复、防护；停线规则、安全兜底 | 测试失败、构建崩、行为异常 |

### REVIEW（评审）— 4 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| code-review-and-quality | 五轴评审、变更规模（~100 行）、严重度标签（Nit/Optional/FYI）、评审速度规范、拆分策略 | 任何变更合并前 |
| code-simplification | Chesterton's Fence、Rule of 500、在保持行为不变的前提下降复杂度 | 代码能跑但太难读/维护 |
| security-and-hardening | OWASP Top 10 防护、认证模式、密钥管理、依赖审计、三层边界系统 | 处理用户输入、认证、数据存储、外部集成 |
| performance-optimization | 先测量——Core Web Vitals 目标、剖析工作流、打包分析、反模式检测 | 有性能要求或怀疑回归 |

### SHIP（交付）— 5 个
| 技能 | 作用 | 使用时机 |
|---|---|---|
| git-workflow-and-versioning | trunk-based 开发、原子提交、变更规模（~100 行）、"提交即存档点"模式 | 任何代码变更（总是用） |
| ci-cd-and-automation | Shift Left、更快更安全、feature flags、质量门流水线、失败反馈环 | 搭建/修改构建与部署流水线 |
| deprecation-and-migration | 代码即负债思维、强制 vs 建议性废弃、迁移模式、僵尸代码清除 | 移除旧系统、迁移用户、下线功能 |
| documentation-and-adrs | 架构决策记录（ADR）、API 文档、内联文档规范——记下"为什么" | 做架构决策、改 API、上线功能 |
| observability-and-instrumentation | 结构化日志、RED 指标、OpenTelemetry 追踪、症状式告警——边构建边埋点 | 加遥测，或交付任何上生产的代码 |
| shipping-and-launch | 上线前检查清单、feature flag 生命周期、分阶段发布、回滚流程、监控配置 | 准备部署到生产 |

> 25 个 = 24 个生命周期技能 + 1 个元技能 `using-agent-skills`（教 Agent 怎么用这个包）。

---

## 5. Agent Personas（4 个专家人格）

预配置的专家人格，用于定向评审：

| Agent | 角色 | 视角 |
|---|---|---|
| code-reviewer | 高级员工工程师 | 五轴代码评审，"员工工程师会批准吗？"标准 |
| test-engineer | QA 专家 | 测试策略、覆盖率分析、Prove-It 模式 |
| security-auditor | 安全工程师 | 漏洞检测、威胁建模、OWASP 评估 |
| web-performance-auditor | Web 性能工程师 | Core Web Vitals 审计，Quick/Deep 双模式 + 指标诚实规则；通过 `/webperf` 调用 |

> 编排规则与决策矩阵见 docs/agents.md（含"人格不调用人格"规则）。

---

## 6. Reference Checklists（7 个辅助清单）

技能需要时拉取的速查资料：

| 清单 | 覆盖内容 |
|---|---|
| definition-of-done.md | 项目级通用"完成标准"（区别于每个任务的验收标准） |
| testing-patterns.md | 测试结构、命名、mock、React/API/E2E 示例、反模式（JS/TS） |
| security-checklist.md | 提交前检查、认证、输入校验、headers、CORS、OWASP Top 10 |
| performance-checklist.md | Core Web Vitals 目标、前端/后端清单、测量命令 |
| accessibility-checklist.md | 键盘导航、屏幕阅读器、视觉设计、ARIA、测试工具 |
| observability-checklist.md | 值班问题、结构化日志、RED/USE 指标、追踪、症状式告警、上线前闸门 |
| orchestration-patterns.md | 多人格编排的正/反模式、"人格不调用人格"规则 |

---

## 7. Skill 的设计结构（How Skills Work）

每个技能遵循一致解剖结构：

```
SKILL.md
├── Frontmatter      → name（小写连字符）+ description（"指导 Agent 做…，当…时使用"）
├── Overview         → 这个技能干什么
├── When to Use      → 触发条件
├── Process          → 分步工作流
├── Rationalizations → 常见借口 + 反驳（Agent 跳过步骤时的托词表）
├── Red Flags        → 出问题的信号
└── Verification     → 证据要求
```

**关键设计选择：**
1. **流程，不是散文**——技能是 Agent 遵循的工作流，不是给它们读的参考文档；每步有检查点与退出标准
2. **反合理化**——每个技能含"Agent 偷懒借口表"（如"我稍后再加测试"）并配文档化反驳
3. **验证不可妥协**——每个技能以证据要求收尾：测试通过、构建输出、运行时数据。"看起来对"永远不够
4. **渐进式披露**——SKILL.md 是入口，辅助 references 需要时才加载，token 开销最小

---

## 8. 安装方式（各 Agent）

### 最快路径（通用，70+ Agent）
```bash
npx skills add addyosmani/agent-skills            # 装全部 25 个技能
npx skills add addyosmani/agent-skills --list     # 先浏览再装
# 单独装某个：
npx skills add addyosmani/agent-skills --skill code-review-and-quality
npx skills add addyosmani/agent-skills --skill interview-me
npx skills add addyosmani/agent-skills --skill test-driven-development
```
> ⚠️ **单技能安装的坑**：只复制 `skills/<name>/`，不带仓库级 `references/` 目录——技能能用，但指向共享清单的路径失效。要完整功能用整仓安装，或把所需清单拷进技能内的 `references/`（issue #361）。

### Claude Code（推荐）
```
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```
- SSH 报错：先配 `git config --global url."https://github.com/".insteadOf git@github.com:`（或用完整 HTTPS URL）
- 本地开发：`git clone` 后 `claude --plugin-dir /path/to/agent-skills`

### 其他工具
- **Cursor**：工作流技能放 `.cursor/skills/`，短策略放 `.cursor/rules/*.mdc`
- **Antigravity CLI**：`agy plugin install https://github.com/addyosmani/agent-skills.git`
- **Gemini CLI**：`gemini skills install https://github.com/addyosmani/agent-skills.git --path skills`
- **Windsurf / OpenCode / GitHub Copilot**：见 docs/ 下各自 setup 指南

---

## 9. 项目结构

```
agent-skills/
├── skills/            # 25 个技能（24 生命周期 + 1 元）
├── agents/            # 4 个专家人格
├── references/        # 7 个辅助清单
├── hooks/             # 会话生命周期钩子
├── .claude/commands/  # 8 个斜杠命令（Claude Code）
├── .gemini/commands/  # 8 个斜杠命令（Gemini CLI）
├── commands/          # 8 个斜杠命令（Antigravity CLI）
├── plugin.json        # Antigravity 插件清单
└── docs/              # 各工具安装指南 + skill 格式规范 + 对比
```

---

## 10. 与其他技能包的对比

与 [Superpowers](https://github.com/obra/superpowers) 和 [Matt Pocock's skills](https://github.com/mattpocock/skills) 的诚实横向对比见 `docs/comparison.md`（含一场受控对决实验链接）。

三者的定位差异：
- **agent-skills**：以 Google 工程实践为底色的全生命周期流程包
- **Superpowers**：偏向 Agent 协作与推理增强
- **Matt Pocock's skills**：偏向具体工作流技巧（grill-me、to-spec 等）

---

## 11. 团队与许可

- 作者：**Addy Osmani**（Google Chrome 团队，Web 性能权威）+ 合作者 Federico Bartoli、Joan León
- **MIT 许可**——可在项目、团队、工具中自由使用

---

*本文档由官方 README（v0.6.9，430 行）总结翻译。*
