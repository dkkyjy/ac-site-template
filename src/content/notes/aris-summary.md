---
title: "ARIS (Auto-Research-In-Sleep) - 自主 ML 研究技能库总结"
category: "专项工具 / 科研"
description: "ARIS ⚔️ 文档总结：82 个 Markdown-only 科研技能、7 条科研工作流（选题→实验→写作→Rebuttal→演讲），以及「执行者 × 跨家族审查者」对抗式 review loop 的方法论。"
pubDate: "2026-09-11"
badge: "guide"
tags: ["aris", "research-automation", "claude-code", "codex", "skills", "ml-research", "mcp"]
---

# ARIS (Auto-Research-In-Sleep) 文档总结

> 仓库: https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep  
> 官网/PPT: [ARIS Intro HTML](https://wanshuiyin.github.io/Auto-claude-code-research-in-sleep/ARIS_INTRO.html) · 技术报告 arXiv:2605.03042（Hugging Face Daily Paper #1）  
> 规模（2026-09-11 实测）: **16010 star / 1375 fork** · MIT · Python · 创建 2026-03-10 · 仍在高频更新  
> 一句话: **把「做研究」拆成 82 个纯 Markdown 技能，让 Claude Code 当执行者、Codex/GPT 当独立审查者，在你睡觉时自动循环改稿与跑实验。**

---

## 1. 它是什么

ARIS = **Auto-Research-In-Sleep**，一个**「不是框架」的方法论层**：

| 特性 | 说明 |
|:-----|:-----|
| 🪶 **Lightweight Markdown-only** | 每个技能就是一个 `SKILL.md`（含契约、阶段、触发条件），无运行时依赖、无 lock-in |
| 🔀 **跨模型对抗** | 执行者（写代码/跑实验）与审查者（冷读稿件打分）**必须不同模型家族**（Claude × GPT、GLM × DeepSeek、Antigravity × Gemini…） |
| 🌙 **过夜自治** | 配置 auto-allow 后，review loop 可无人值守跑一整夜，直到评分达标 |
| 🔌 **任何 Agent 宿主** | Claude Code（主路径）/ Codex CLI / Cursor / Trae / Antigravity / GitHub Copilot CLI / OpenClaw / DeepSeek Harness，另有独立 CLI 版 **ARIS-Code** |
| 🎓 **科研全流程** | 选题 → 实验 → 写作 → Rebuttal → 换会议重投 → 演讲 PPT，甚至专利与证明 |

---

## 2. 核心理念（方法论部分，最值得抄的）

### 2.1 形式化目标：审查者 = 不可微的正则项

$$\max_\theta \; U_{\text{exec}}(\theta; x) - \lambda \cdot R_{\text{review}}(\theta; x)$$

- `U_exec`：执行者模型写代码 / 跑实验的效用
- `R_review`：来自**跨家族**审查者的对抗正则项，惩罚：过度声称、编造引用、无依据的定理推广、自我吹捧
- 关键点：这个正则项**不可微**——它只是一条全新的 LLM 线程冷读产物（"reading the artifact cold"）

### 2.2 Reviewer-Independence 协议（硬规则）

每一轮 review 都开 **全新 codex 线程**，**绝不**用 `codex-reply` 续上一轮对话。

> 教训来源：真实的 NeurIPS 投稿中，用 `codex-reply` 链式对话后，稿件分数从 **3/10 被「叙事累积」抬到 8/10**——审查者开始为先前批评辩护，而不是评估当前产物。

规范文件：`skills/shared-references/reviewer-independence.md`

### 2.3 跨家族不变式

- 执行者与审查者**必须**不同模型家族；**同家族审查不算特性**（non-feature）
- 只有一个供应商？最便宜的补法：用 `llm-chat` MCP 加一个免费的 DeepSeek / Gemini 审查者

### 2.4 循环结构

```
┌──────────────────────┐        ┌───────────────────────────┐
│  ARIS — execution    │        │  Codex MCP (GPT)          │
│  (Claude Code)       │───────▶│  — 冷读稿件                │
│  — 读文件            │        │  — 全新线程                │
│  — 写代码            │        │  — 打 1-10 分              │
│  — 部署到 GPU        │        │  — 给修复建议              │
└──────────────────────┘        └───────────────────────────┘
         ▲                                    │
         │            weakness list (.md)     │
         └────────────────────────────────────┘
                      fix list (with budget)
```

---

## 3. 七条主工作流（Workflows）

| # | 名称 | 一句话 | 入口 |
|:--:|:-----|:-------|:-----|
| **1** | Idea Discovery | 文献 → 头脑风暴 8–12 个想法 → 新颖性核查 → GPU pilot 2–3 个 → 排序报告 | `/idea-discovery` |
| **1.5** | Experiment Bridge | 计划 → 实现 → GPT 代码审查 → sanity check → 部署 → 收首批结果 | `/experiment-bridge` |
| **2** | Auto Review Loop | 审查 → 修 → 重跑 → 循环直到 **≥ 6/10**（或 `MAX_ROUNDS=4` 用尽） | `/auto-review-loop` |
| **3** | Paper Writing | 叙事 → 提纲 → 图 → LaTeX → PDF → 2 轮 review（实测 **4 → 8.5/10**） | `/paper-writing` |
| **4** | Rebuttal | 解析审稿意见 → 策略 → 可选补实验 → 起草 → 压力测试 | `/rebuttal` |
| **5** | Resubmit | 在硬约束下把论文移植到新会议（不许新实验、不许改 bib） | `/resubmit-pipeline` |
| **6** | Conference Talk | 论文 → Beamer + PPTX + 讲者备注 + 一致性审计 | `/paper-talk` |

另有两条内部/专项轨道：

- **全链路** `/research-pipeline`：Workflow 1 → 1.5 → 2 → 3，从研究方向直通可投稿论文
- **Workflow 7** `/proof-orchestrator`：独立定理证明轨道——跨天、跨轮次攻击一个定理（状态化 run 目录、跨 run 续跑、人工 GPT Pro 交接包、可选 DeepSeek 二次意见，但 `/proof-writer` 只起草、`/proof-checker` 独占验证与提交验收）
- **Workflow M** `/meta-optimize` + `/meta-apply`：ARIS 优化自己（元优化）

### 附：控制轴

| 轴 | 取值 | 效果 |
|:---|:-----|:-----|
| **effort** | `lite` / `balanced`(默认) / `max` / `beast` | 广度深度从 ~0.4× 到 5–8×（论文数、想法数、pilot 数、轮数、种子数、审计深度） |
| **assurance** | `draft` / `submission` | `max`/`beast` 时自动升为 submission：论文写作 Phase 6 **强制**在全新线程跑 `/proof-checker` + `/paper-claim-audit` + `/citation-audit`，`tools/verify_paper_audits.sh` 非零退出就**拒绝出 Final Report**（可用 `— effort: beast, assurance: draft` 逃生） |
| **reviewer** | 默认 Codex `xhigh`（深审 `ultra`） | 加 `— reviewer: oracle-pro` 可把 review 路由到 **GPT-5.5 Pro**（未装则优雅降级+告警） |

> 任何 effort 等级都**永不改变**的东西：Codex 推理保持档位下限、DBLP/CrossRef 引用核查开启、审查者独立性开启、实验完整性检查开启。

---

## 4. 技能库全貌：82 个技能 / 10 大类

| 分类 | 数量 | 技能 |
|:-----|:--:|:-----|
| 🏗️ Workflow Orchestrators | 16 | `research-pipeline` `idea-discovery` `idea-discovery-robot` `experiment-bridge` `auto-review-loop`(+`-llm` `-minimax`) `paper-writing` `rebuttal` `resubmit-pipeline` `paper-talk` `research-refine-pipeline` `patent-pipeline` `dse-loop` `meta-optimize` `meta-apply` |
| 📚 Literature & Search | 11 | `research-lit` `arxiv` `semantic-scholar` `deepxiv` `exa-search` `web-debug-search` `openalex` `gemini-search` `alphaxiv` `comm-lit-review` `novelty-check` |
| 💡 Ideation & Method Design | 5 | `idea-creator` `research-refine` `experiment-plan` `ablation-planner` `formula-derivation` |
| 🧠 Proof Engineering | 1 | `proof-orchestrator` |
| 🧪 Experiments & Infrastructure | 9 | `run-experiment` `monitor-experiment` `analyze-results` `experiment-queue` `vast-gpu` `serverless-modal` `qzcli` `training-check` `system-profile` |
| 🛡️ Review, Audit & Assurance | 8 | `research-review` `experiment-audit` `result-to-claim` `paper-claim-audit` `citation-audit` `proof-checker` `kill-argument` `integrity-forensics` |
| 📝 Paper Writing & Figures | 13 | `paper-plan` `paper-write` `paper-figure` `figure-spec` `paper-illustration`(+`-image2`) `mermaid-diagram` `pixel-art` `paper-compile` `auto-paper-improvement-loop` `proof-writer` `writing-systems-papers` `grant-proposal` |
| 🎤 Talks, Posters & Resubmission | 4 | `paper-slides` `slides-polish` `paper-poster` `paper-poster-html` |
| 📜 Patents | 9 | `invention-structuring` `claims-drafting` `embodiment-description` `specification-writing` `figure-description` `prior-art-search` `patent-novelty-check` `patent-review` `jurisdiction-format` |
| 🧰 Meta, Utilities & Integrations | 6 | `research-wiki` `wiki-enrich` `render-html` `overleaf-sync` `feishu-notify` `interview-cheatsheet` |

**几个亮点技能：**

- `research-wiki` — **持久化研究知识库**：跨会话积累论文 / 想法 / 实验 / 论断及其关系，灵感来自 Karpathy 的 LLM Wiki 模式；子命令 `init|ingest|sync|query|update|lint|stats`（配 `wiki-enrich` 持续补覆盖度），是「让 agent 记住整个领域」的关键件
- `experiment-queue` — SSH 作业队列：多种子/多配置扫描、OOM 重试、残屏清理、波次门控、崩溃安全状态
- `integrity-forensics` / `citation-audit` / `paper-claim-audit` — **防伪造/防过度声称**三件套（另见同作者的 *Anti-Autoresearch*：61 条完整性信号 → 确定性审查报告）
- `kill-argument` — **双线程对抗审查**：一个新线程写出最强 200 词拒稿备忘录，另一个新线程逐条辩护并暴露仍未解决的关键问题（理论论文投稿前使用）
- `render-html` — 把内容渲染成交互 HTML（ARIS 自己的官网、面试题库都是它生成的）
- `dse-loop` — 领域定制技能范例（EDA/芯片设计循环），作者用它演示「如何贡献新领域技能」

> **Codex 镜像**：上述每个技能在 `skills/skills-codex/` 都有平行实现（把 Codex-MCP 审查链路换成 Codex 原生 `spawn_agent` + `send_input`），语义一致。

---

## 5. 仓库结构导航

| 路径 | 内容 |
|:-----|:-----|
| `README.md` (224 KB) / `README_CN.md` (182 KB) | 主文档（README 同时是巨型 changelog，看正文请用目录跳转） |
| `docs/` | 94 篇 Markdown 指南（56 个顶层条目）：`SKILLS_CATALOG.md`（技能总目录）、`ARIS_INTRO.md`、`MODEL_COMBINATIONS.md`、`CUSTOMIZATION.md`、`GPU_SETUP.md`、`MANUAL_REVIEW_GUIDE.md`、`SESSION_RECOVERY_GUIDE.md`、各宿主适配（Cursor/Trae/Antigravity/Copilot/OpenClaw） |
| `AGENT_GUIDE.md` | **给 AI Agent 读的**结构化指南（作者建议：AI 别读 README，读这个） |
| `SETUP_GUIDE.md` / `SETUP_GUIDE_CN.md` | 6 步安装向导（macOS 本地 + 远程 Linux GPU，Claude Code 执行 + Codex MCP 审查） |
| `skills/` (86 项) | 技能本体 + `shared-references/`（契约文件：`reviewer-independence.md`、`effort-contract.md`、`assurance-contract.md`、`experiment-integrity.md`…）+ Codex 镜像 |
| `mcp-servers/` (8 个) | `claude-review` `codex-exec` `codex-image2` `feishu-bridge` `gemini-review` `llm-chat` `manual-review` `minimax-chat` |
| `tools/` (40+ 脚本) | 安装/更新（`install_aris.sh` `smart_update.sh` 及 codex/ps1 变体）、各类 fetcher（arxiv/deepxiv/exa/openalex/semantic scholar）、门禁（`review_gate.py` `forensics_gate.py` `idea_discovery_gate.py` `evidence_check.py` `threat_scan.py`）、`skill-groups.tsv`、`watchdog.py` |
| `templates/` | `CLAUDE_MD_TEMPLATE.md`、实验计划/日志、想法候选、研究契约、专利说明书等模板 |
| `aris-monitor/` | 悬浮监控小工具：纯标准库 **Tkinter**（零 pip 依赖）、**只读** `~/.claude` 判断哪个会话在等你批准，唯一写操作是「点击行 → 置前终端窗口」，**永不 kill/发信号/联网** |
| `templates/claude-hooks`, `.github/agents` | Claude hooks 与 GitHub agents 配置 |
| `tests/`, `community_papers/` | 测试；社区论文样例（如 UAV-CC.pdf） |

---

## 6. 安装与配置（实测命令）

### 6.1 前置

1. **Claude Code**（所有技能跑在它之上）
2. **Codex CLI**（作跨模型审查者）：`npm install -g @openai/codex` + `codex login`
3. **LaTeX**（仅 Workflow 3 写作需要）：`brew install --cask mactex` + `brew install poppler`；Ubuntu 用 `texlive-full latexmk poppler-utils`；只做选题+审查可不装

### 6.2 安装技能（推荐：项目级扁平软链）

```bash
# 1. 克隆一次到稳定位置
git clone https://github.com/wanshuiyin/Auto-claude-code-research-in-sleep.git ~/aris_repo

# 2. 对每个用到 ARIS 的项目挂软链
cd ~/your-paper-project
bash ~/aris_repo/tools/install_aris.sh
#   → 每个技能一条软链: .claude/skills/<skill> → ~/aris_repo/skills/<skill>
#   → 写清单 .aris/installed-skills.txt（只记录 ARIS 装过的条目）
#   → 更新 CLAUDE.md 中的 ARIS 块（compare-and-swap，尽力而为）
#   → 可重复运行，用于对齐新增/移除的上游技能

# 3. 更新内容（软链指向活的仓库，git pull 即生效）
cd ~/aris_repo && git pull
# 3a. 想同步上游新增/删除的技能，需重跑安装器：
bash ~/aris_repo/tools/install_aris.sh ~/your-paper-project
```

**选择性安装与其它开关：**

```bash
bash ~/aris_repo/tools/install_aris.sh --list-groups               # 10 个技能组目录
bash ~/aris_repo/tools/install_aris.sh --groups paper-core,lit-search
bash ~/aris_repo/tools/install_aris.sh --skills paper-writing      # 硬依赖自动带入
bash ~/aris_repo/tools/install_aris.sh --exclude patent-pipeline   # 拒绝后不再追问
bash ~/aris_repo/tools/install_aris.sh --dry-run | --uninstall     # 预览 / 仅卸载自管条目
```
（更新时上游**新技能**需逐个确认：`--add-new` 全收 / `--skip-new` 全跳；Windows 用 `tools\install_aris.ps1` 生成 junction，无需 WSL；Codex 用户走 `install_aris_codex.sh`。）

### 6.3 注册审查者 MCP

```bash
# codex-cli 0.154.0 移除了 `codex mcp-server`，ARIS 改用自带桥接:
claude mcp add codex -s user -- python3 ~/aris_repo/mcp-servers/codex-exec/server.py
```
> ⚠️ 注册/修改任何 MCP 后**必须重启 Claude Code**（MCP 只在启动时加载）。

### 6.4 过夜无人值守（可选）

`.claude/settings.local.json` 里放开权限，避免每步都弹批准：

```json
{
  "permissions": {
    "allow": ["mcp__codex__codex", "mcp__codex__codex-reply", "Write", "Edit", "Skill(auto-review-loop)"]
  }
}
```

### 6.5 可选增强

| 能力 | 说明 |
|:-----|:-----|
| 🖥️ GPU | 在 `CLAUDE.md` 声明服务器即可：**远程 SSH / 本地 GPU / Vast.ai 按需**；`/serverless-modal` 走 Modal 无服务器、`/qzcli` 管启智平台任务 |
| 📚 Zotero | 收藏夹 + 标注 + BibTeX 进入 `/research-lit`（先于网络搜索） |
| 🗒️ Obsidian + arXiv | 检索你的笔记库；arXiv 内置零配置 |
| 📱 Feishu / Lark | 手机推送 + 交互式「批准/拒绝」，过夜任务的关键 |

---

## 7. 不带 Claude / OpenAI 也能跑

ARIS 官方给了 **10 条替代模型路线**（`docs/MODEL_COMBINATIONS.md`）：Z.ai GLM、阿里 Kimi/Qwen/MiniMax、**ModelScope 免费 DeepSeek-V3.1**、OpenRouter（从一堆模型里钉一个审查者）、Codex 当执行者 + Claude/Gemini 当审查者、Google Antigravity……跨模型架构不变，只换供应商。

---

## 8. 生态（同一作者的产品线）

| 项目 | 作用 |
|:-----|:-----|
| **ARIS-Code** | 独立 CLI，体验完整版（自带 setup/model/reviewer 命令、Windows CI） |
| **dsh-aris** | DeepSeek Harness 插件：`dsh plugin --profile web add dsh-aris`（82 技能不变，Codex 仍是独立审查者） |
| **ARIS-in-AI-Offer** | 34 份中英双语 ML/LLM/多模态/生成/Agent 面试速查表（单页站，可搜、暗色、已读追踪），全由 ARIS 的 `/render-html` 生成 |
| **HERO-Anti-OverDefense** | ~550 token 的 `CLAUDE.md`/`AGENTS.md` 块，治「Hashing / Edge cases / Rubrics / Overbuild」——约束 agent **提议什么**，而非**查找什么** |
| **Anti-Autoresearch** | 61 条完整性信号 → 确定性、可交审的报告（造伪取证，不是 AI 文本检测器） |
| **ARIS-Movie-Director** | 多模态延伸：粗故事 → 逐场景校验的分镜电影（参考运行 19 个场景） |

**荣誉/背书**：Hugging Face Daily Paper #1、PaperWeekly 报道、入选 awesome-agent-skills、AI Digital Crew 当日项目。许可证 **MIT**。

---

## 9. 我的评价（值得借鉴的 4 点）

1. **「审查者独立」是可复用的工程规则**——同一对话续聊会让 LLM 审查者从「评估产物」退化成「维护立场」，3/10→8/10 的虚高就是代价；开新线程冷读是解药。
2. **跨家族 = 最低成本的对抗性**——同模型自评几乎无信息量；加一个便宜的不同家族审查者立刻有了张力。
3. **安装用「清单 + 软链」而非复制**：`.aris/installed-skills.txt` 让卸载/对齐只碰自管条目，永不动用户自己的技能——做技能分发的人可以直接抄。
4. **effort / assurance 双轴**：能力（广度深度）与「审计是否承重」正交，且明示「任何档位都不放松的底线」（引用核查、实验完整性、审查者独立）。

**适用人群**：有 GPU 的 ML 研究者 / PhD、要批量产论文与实验的组、想研究「AI 自治科研工作流」的工程人。**门槛**：至少要一个执行者 CLI + 一个异家族审查者可调用，外加（写作/实验时）LaTeX 与 GPU。

---

## 附：关键事实速查

| 项 | 值 |
|:---|:---|
| 仓库 | `wanshuiyin/Auto-claude-code-research-in-sleep`（简称 ARIS ⚔️ / Auto-Research-In-Sleep） |
| 指标 | 16010 ★ / 1375 fork（2026-09-11） |
| 许可 | MIT | 语言 Python | 创建 2026-03-10 |
| 技能 | **82 个**（10 大类）+ Codex 镜像；`skills/` 下另有 `shared-references/` 契约文件 |
| 工作流 | 1 选题 / 1.5 实验桥 / 2 自动审查 / 3 写作 / 4 Rebuttal / 5 重投 / 6 演讲 + 7 证明 + M 元优化 |
| 安装 | `git clone` → `bash tools/install_aris.sh`（项目级软链）→ `claude mcp add codex ...` |
| 技术报告 | arXiv:2605.03042 |
