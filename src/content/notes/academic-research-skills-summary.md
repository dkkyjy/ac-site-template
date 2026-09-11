---
title: "Academic Research Skills - 学术论文全流程技能包"
category: "技能库"
description: "Imbad0202/academic-research-skills 完整总结：deep-research（13-agent 研究）、academic-paper（12-agent 写作）、academic-paper-reviewer（5 席位评审）、academic-pipeline（10 阶段编排）四大技能，从研究到发表的全流程。"
pubDate: "2026-09-10"
badge: "guide"
tags: ["academic", "research", "paper", "skills", "claude-code"]
---
# Academic Research Skills — 学术论文全流程技能包（v3.21.2）

> 仓库地址：https://github.com/Imbad0202/academic-research-skills （v3.21.2，作者 Cheng-I Wu，CC-BY-NC-4.0，DOI: 10.5281/zenodo.20696614）
> 一句话：**面向 Claude Code 的契约式学术流水线**——研究 → 写作 → 完整性检查 → 评审 → 修改 → 复审 → 定稿，四大技能、27 种模式、39 个提示角色（3 个插件级 agent，其余默认内联执行）。

---

## 1. 项目档案

- **形态**：Claude Code 插件市场格式（`.claude-plugin/marketplace.json`），含 4 个 skill + 16 个 commands + 3 个插件级 agent
- **版本体系**：`academic-pipeline` v3.21.2 / `deep-research` v2.12.1 / `academic-paper` v3.3.1 / `academic-paper-reviewer` v1.11.1
- **核心立场**：**AI 是副驾驶，不是飞行员**——不替你写论文，而是处理"苦力活"（查文献、格式化引用、验证数据、逻辑一致性检查），让你专注真正需要大脑的部分
- **设计动因**：参考 Lu et al. (2026, *Nature* 651:914-919) 对 **The AI Scientist** 的失败模式清单（实现 bug、幻觉结果、捷径依赖、方法论捏造、frame-lock、引用幻觉）——ARS 刻意走 **human-in-the-loop**，而非全自动
- **与 humanizer 的区别**：不帮你"隐藏用了 AI"，而是帮你写得更好——Style Calibration 从你的既往写作学习风格，Writing Quality Check 抓住让文字显得机器味的模式

---

## 2. 四大技能总览

| Skill | 版本 | 定位 | Agent 数 | 模式数 |
|-------|------|------|----------|--------|
| `deep-research` | 2.12.1 | 深度研究（选题到系统综述） | 13 | 8 |
| `academic-paper` | 3.3.1 | 论文写作（大纲到格式转换） | 12 | 11 |
| `academic-paper-reviewer` | 1.11.1 | 多视角评审（含复审/校准） | 7 | 6 |
| `academic-pipeline` | 3.21.2 | 全流程编排器（串起上面三者） | 5 | 10 阶段 |

**依赖关系**：`academic-pipeline` 编排 `deep-research`（Stage 1 RESEARCH）→ `academic-paper`（WRITE/REVISE）→ `academic-paper-reviewer`（REVIEW/RE-REVIEW），中间插入强制完整性检查。

---

## 3. deep-research — 13-agent 深度研究团队

覆盖从研究问题提出到系统综述与元分析的完整研究管线，**8 种模式**：

| 模式 | 输出 | 字数 |
|------|------|------|
| `full`（默认） | 完整 APA 7.0 报告（9 个核心 agent） | 3,000–8,000 |
| `quick` | 研究简报 | 500–1,500 |
| `review` | 对给定文本的评审报告（Editor+DA+Ethics） | N/A |
| `lit-review` | 注释书目 + 综合 | 1,500–4,000 |
| `three-way-scan` | WHY/HOW/WHAT 三视角论文短名单对比 | 800–2,000 |
| `fact-check` | 来源核验报告 | 300–800 |
| `socratic` | 苏格拉底式引导研究对话（5 层） | 迭代式 |
| `systematic-review` | 完整 PRISMA 2020 报告 + forest plot + GRADE 表 | 5,000–15,000 |

**13 个 agent 亮点**：RQ 制定、方法设计、系统检索、**source verification**（来源核验）、跨源综合、偏倚风险评估（RoB 2 / ROBINS-I）、元分析（效应量/异质性/GRADE）、编辑评审、魔鬼代言人、伦理审查（AI 协助研究伦理/归属完整性/双重用途筛查）、**post-research literature monitoring**（发表后文献监测：撤稿预警/矛盾发现）。

---

## 4. academic-paper — 12-agent 论文写作团队

**11 种模式**：`full`（全文草稿）/ `plan`（大纲规划）/ `outline-only` / `revision`（按评审意见修改）/ `revision-coach`（把散乱审稿意见解析成修改路线图）/ `abstract`（双语摘要）/ `lit-review` / `format-convert` / `citation-check` / `disclosure`（AI 使用披露）/ `rebuttal-audit`（回复信审计）。

**12 个 agent**：intake（配置访谈：论文类型/期刊/引用格式/语言/字数）、literature_strategist（检索策略+注释书目+文献矩阵）、structure_architect（结构选择+提纲+证据映射）、argument_builder（论点-证据链+CER+反方处理）、draft_writer（逐节草稿）、citation_compliance（引用格式核验+DOI 检查）、abstract_bilingual（中英双语摘要+关键词）、peer_reviewer（模拟双盲评审，最多 2 轮）、formatter（LaTeX/DOCX-via-Pandoc/PDF/期刊格式/投稿信）、socratic_mentor（大纲模式的苏格拉底导师）、visualization（生成可发表级图表代码：matplotlib/R ggplot2、色盲安全配色、LaTeX 集成）、revision_coach。

**输出格式**：文本 5 种（Markdown/LaTeX/DOCX/PDF/期刊模板），引用 5 种（APA 7 / Chicago / MLA / IEEE / Vancouver）。

---

## 5. academic-paper-reviewer — 7-agent 多视角评审团队

**5 席位、角色分离的评审小组**（Journal-Fit 编辑 + 3 位同行评审 + 魔鬼代言人），按领域专业配置；角色分离不等于独立错误过程，这是明示的设计声明。

**6 种模式**：`full`（默认：5 份评审报告 + 编辑决定 + 修改路线图）/ `re-review`（管线 Stage 3' 的验证复审，冻结 Round-1 评审卡、checker 闭环规则推导结论）/ `quick`（15 分钟快速评估）/ `methodology-focus`（方法论深度审查）/ `guided`（苏格拉底逐问题引导评审）/ `calibration`（评审员校准：3 篇金标准论文 × 完整评审组，测评审员准确度）。

**输出**：5 份评审报告 + Editorial Decision（Accept/Revise/Reject）+ Revision Roadmap；魔鬼代言人有专门报告结构。

---

## 6. academic-pipeline — 10 阶段全流程编排器

```
RESEARCH → WRITE → 2.5 INTEGRITY → REVIEW → REVISE → 3' RE-REVIEW → 4' RE-REVISE → 4.5 FINAL INTEGRITY → FINALIZE → PROCESS SUMMARY
```

- **Stage 2.5 / 4.5 强制完整性检查**：引用/主张 5 阶段核验（`integrity_review_protocol`），FAIL → 修复重验（最多 3 轮），之后进入 Integrity Check FAIL Loop → 记录用户决策
- **IRON RULE（v3.2）**：2.5 和 4.5 必须跑 **AI 研究失败模式清单**（7 模式：实现 bug/幻觉结果/捷径依赖/bug-as-insight/方法论捏造/frame-lock 等）；任一模式 SUSPECTED 即**阻断**，必须用户确认/带理由覆盖/修改——无配置项可静默，唯一出路是带审计轨迹的用户确认
- **两阶段评审**：Stage 3 首轮 full review → Stage 3' 复审（冻结首轮评审卡、checker 推导结论）
- **并行化（v3.3）**：Stage 2 中文献策略与图表生成可并行（仿 PaperOrchestra）
- **Process Summary**：产出论文创建过程记录 MD + LaTeX→PDF（双语）

---

## 7. 质量保障机制（契约体系）

| 版本 | 机制 |
|------|------|
| v3.6.6 | Generator-Evaluator 契约（四调用结构：生成器/评估器分离） |
| v3.7.3 + v3.8 | L3 claim-faithfulness gate（主张忠实度关卡） |
| v3.9 / v3.10 | Cross-index triangulation（跨索引三角互证）+ 策略层 |
| v3.11 | 确定性引用验证关卡（#182） |
| #660 / #672 | Tortured-phrase 咨询（被 AI 扭曲的短语）+ 跨文档一致性咨询 |
| #684 | Review-target criteria binding（评审目标标准绑定） |

---

## 8. 安装与使用

```text
# Claude Code 插件市场（30 秒）
/plugin marketplace add Imbad0202/academic-research-skills
/plugin install academic-research-skills

# 或传统方式：把 skill 目录 symlink 到 skills 目录
# 本机已装：~/.agents/skills/{academic-paper, academic-paper-reviewer, academic-pipeline, deep-research}
```

**触发词**（多语言）：写论文 / 学术论文 / 審查論文 / 論文審查 / 深度研究 / 文献综述 / systematic review / meta-analysis / fact-check / 심층 연구 / 논문 심사 等。

**典型入口**：`/ars-plan` 苏格拉底式对话梳理论文结构，或直接说"帮我写一篇……的论文"进入 full 模式。

---

## 9. 一句话定位

**给"想要用 AI 辅助做严肃学术研究"的人**——它不替你思考，但把你从引用、格式、核验、评审这些环节中解放出来，并用契约化的质量关卡（完整性检查、主张忠实度、确定性引用验证）把 AI 幻觉挡在论文之外。适用于从课程论文到期刊投稿的全流程。
