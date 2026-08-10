---
title: "OpenSpec 蒸馏 — Spec-Driven Development"
description: "Spec-Driven Development(SDD) 轻量规格驱动开发框架：先对齐再构建。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["openspec", "sdd", "engineering"]
---

# OpenSpec 蒸馏 — Spec-Driven Development (SDD) 技能

## 定位
OpenSpec (Fission-AI/OpenSpec, 64k★) 是 AI 编码助手的**轻量规格驱动开发(SDD)**框架。
核心理念五词: **agree first, then build confidently**（先对齐，再自信构建）。
价值: 需求不只在聊天历史里,而在一个可运行的规格层;人对齐"要建什么"之后才写代码。
哲学: fluid not rigid / iterative not waterfall / easy not complex / brownfield-first(增量改旧系统) / 个人到企业可扩展。

## 蒸馏范围
- 12 个 `skills/openspec-*-change` 工作流技能（本质是 agent 执行手册）
- `docs/overview.md + concepts.md` 心智模型
- `schemas/spec-driven/templates/` 四个工件模板
- `openspec/specs/*` 真实 delta spec 示例

## 两条支柱目录
```
openspec/
├── specs/                      # 真理源: 系统"当前如何工作"
│   └── <domain>/spec.md        # 按域组织: auth/ payments/ api/
└── changes/                    # 建议改动: 一个 change = 一个文件夹
    ├── <change-name>/          # 每次工作一个文件夹
    └── archive/                # 归档: YYYY-MM-DD-<name>/
```

## Change 文件夹结构 (一次工作单元)
```
openspec/changes/<change>/
├── proposal.md     # WHY+WHAT (为什么/改什么/影响哪些能力)
├── design.md       # HOW (技术方案: Context/Goals/Non-Goals/Decisions/Risks)
├── tasks.md        # 实现清单 (分组 checkbox)
├── .openspec.yaml  # 元数据: schema, created, skip_specs, retire_capabilities
└── specs/          # Delta specs (只写"变化", 不写整个世界)
    └── <capability-path>/spec.md
```

## Delta Spec 语法 (核心技巧)
只描述差异(diff), 不描述终点. 段落头是动词:
```
## ADDED Requirements            # 新增需求
## MODIFIED Requirements         # 修改需求 (保留原始需求名, 常用 `(REPLACES 旧名)`)
## REMOVED Requirements          # 删除需求
```
每个需求 `### Requirement: <名>` + 一个或多个 `#### Scenario:` (WHEN/THEN/AND 结构)。
关键词 RFC2119: **MUST/SHALL**=绝对, **SHOULD**=推荐, **MAY**=可选。

**skipping**: 零新增能力改动(纯refactor/tooling/docs)必须在 .openspec.yaml 设 `skip_specs: true`, 否则 validate 拒收零 delta change。不要为凑数硬造需求。
**删除 sections**: 新能力才写 `## Purpose`; 已有能力删掉该节。

## Artifact 流 (推进顺序)
```
proposal ──► specs ──► design ──► tasks ──► implement
  why          what       how        steps      do it
```
可随时回访任一 artifact——它们是 enabler, 不是 gate。

## Spec 写作原则
- spec 是**行为契约**不是实现计划: 只写可观察行为/输入输出/错误条件/外部约束/可测场景。
- 禁写: 内部类名/函数名、库/框架选型、分步实现细节(那些进 design.md/tasks.md)。
- 快测: 若"实现细节变了但外部可见行为不变", 不该进 spec。
- 渐进严谨: 默认 Lite(行为优先需求+范围+非目标+少量验收); 高风险的(API/contract/迁移/安全/跨团队)才用 Full。

## 12 个工作流技能 (生命周期)
| 命令 | 作用 | 关键动作 |
|------|------|---------|
| on-board | 黑场项目引导, 走完整周期 | 实地实操 |
| explore | 探索模式: 想法先行澄清需求 | think through before commit |
| propose | 一步生成全 artifacts(proposal/specs/design/tasks) | 最快上手 |
| new-change | 实验性分步创建 change | step-by-step |
| ff-change | 快进: 跳过逐步, 一次建齐全部 artifacts | quick generate |
| update-change | 修订已有 planning artifacts 并保持彼此一致 | 改计划不碰代码 |
| continue-change | 继续: 创建下一个 artifact | 推进 |
| apply-change | 按 tasks 实现代码 | 循环到完成/阻塞 |
| verify-change | 核对实现与工件匹配(完整性/正确性/一致性) | 产出报告 |
| sync-specs | 把 delta specs 合并进主 specs(不归档) | agent 驱动智能 merge |
| archive-change | 完工归档 + 同步 specs → 主 specs, 移到 archive/ | 关闭循环 |
| bulk-archive | 批量多 change 归档 + 冲突解决 | 2+ |

## 关键执行规则 (蒸馏自 skills, 避坑)
- **apply**: 先 select change → `status --json` 解 schema → 读全部 contextFiles → 按 task 逐个实现, 立刻勾 checkbox; 模糊/错误/阻塞就 Pause 询问, 不猜; 保持代码改动最小化且贴合每个 task。
- **不下改代码** (update/update-change): 只建 planning artifacts, 改计划就指到 apply。
- **proposal.md 的 Capabilities**: 新能力用 kebab-case 路径(如 user-auth, identity/user-auth); 仅当 spec 级行为变化才列入 Modified Capabilities, 否则写空。
- **archive** 的 target 命名: name 已含 `YYYY-MM-DD-` 前缀则原样用, 否则 prepend `YYYY-MM-DD-`; 禁叠第二个日期。
- **sync-specs 冲突**: 同一主 spec 被多个 change 触碰时, 检查代码库判断谁真的实现了; 只同步已实现的; 多个都实现则按时序合并(新者优先)。
- **verify 降级**: 只有 tasks.md→只验 task 完成; tasks+specs→验完整性+正确性; 全工件→三者都验(含 coherence)。不确定时倾向 SUGGESTION>WARNING>CRITICAL。每个 issue 必须给具体文件:行引用和可执行建议。

## Morphling 分析 (2026-08-07)
| 组件 | 决策 | 理由 |
|------|------|------|
| SDD 方法论/工件格式/delta语法 | **调用(distill to SOP)** | 工具无关, 可直接复用 |
| openspec CLI (TypeScript) | **不重写/可调用** | `npx openspec@latest init` 可请求装; 也可按本SOP纯手写工件流程 |
| 12 工作流技能 | **吸收为本系统工作流** | 已浓缩进本文档各节 |

## 接入方式
- 本系统 new change 时: 建 `openspec/changes/<name>/` + 4 artifacts; 用 delta spec 描述改动。
- 实现工单: 参考 review_sop/code_review_principles 做独立验证; 本 SOP 提供**先规格后实现**的纪律。
- Continue: 对已归档 change 可回看 archive/ 理解"为何改"。

## 记录 (2026-08-07 蒸馏)
目标: https://github.com/Fission-AI/OpenSpec (v最新, MIT), 产品 = @fission-ai/openspec npm。
装: `npx openspec@latest init`; 或独立加 skills: `npx skills add Fission-AI/OpenSpec`。
CLI 命令: init/status/apply/instructions/list/show/validate/archive/doctor/context/view/store/new change。