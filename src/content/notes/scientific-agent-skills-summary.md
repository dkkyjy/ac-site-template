---
title: "Scientific Agent Skills - 163 个科学技能包"
category: "技能库 / 框架"
description: "K-Dense-AI/scientific-agent-skills 完整总结：163 个科学 Agent Skills（19 个分类、100+ 科学数据库），把任意 AI agent 变成 AI Scientist，兼容 Claude Code / Cursor / Codex 等，含论文 arXiv:2609.00065。"
pubDate: "2026-09-10"
badge: "guide"
tags: ["K-Dense", "science", "research", "skills", "claude-code", "bioinformatics"]
---
# Scientific Agent Skills — 163 个科学技能包

> 仓库地址：https://github.com/K-Dense-AI/scientific-agent-skills
> 研究日期：2026-09-10 | 材料：仓库 tarball（231MB / 3072 文件）+ GitHub API
> 本机状态：**已安装 163 个 skill**（`~/.agents/skills/` + `~/.claude/skills/` symlink 全部有效）

---

## 1. 项目档案

- **作者**：K-Dense Inc.（https://k-dense.ai）
- **许可证**：MIT
- **版本**：v2.66.0（`plugin.json`）/ README badge v2.65.0
- **论文**：*Scientific Agent Skills: A Library of Procedural Knowledge for Research Agents*（[arXiv:2609.00065](https://arxiv.org/abs/2609.00065)）
- **体量**：**163 个可用的科学 skill** + **100+ 科学数据库**（官方分类统计 19 类，跨分类计数）
- **形态**：同时是 [Agent Skills](https://agentskills.io/) 标准技能库，也是 [Agent Plugins](https://agent-plugins.org/) 1.0.0 插件包（`plugin.json` + `skills/`）
- **前身**：Claude Scientific Skills → 改名 Scientific Agent Skills，兼容范围从 Claude 扩展到任意 Agent Skills 主机
- **配套项目**：[K-Dense BYOK](https://github.com/K-Dense-AI/k-dense-byok)——免费开源的本地 AI co-scientist，自带 40+ 模型选择、web 搜索、100+ 科学数据库、全部 163 技能，数据留在本机，可经 Modal 扩展到云端算力

**一句话定位**：把"会写代码"的 AI agent 升级成"懂科研"的 AI Scientist——每个 skill 是一段可复用的**程序性知识**（procedural knowledge），覆盖从假设生成、实验设计、数据分析到论文发表、法规合规的科研全流程。

---

## 2. 设计理念

- **程序性知识库**：不是给模型"查资料"，而是给模型"怎么做"的完整工作流（命令、代码框架、边界条件、输出格式）。
- **标准化驱动**：每个 `SKILL.md` 有 YAML frontmatter，仓库通过 `skills-ref validate ./skills/<skill-name>` 校验；skill 内部常用 `uv` 按需装依赖，避免污染环境。
- **边界意识（安全第一）**：README 明确——临床决策支持、RELSA 动物福利评分、ISO 认证材料等 skill **只做"起草/评估辅助"，永远不做诊断/合规/认证的最终决定**；多个 skill 声明"不调用网络"或"不评估个人"。
- **广泛兼容**：Claude Code / Cursor / Codex / Gemini CLI / Google Antigravity / OpenClaw / NemoClaw / Pi / Hermes 等；Agent Plugins 客户端（Cursor、Codex、Copilot、VS Code、Kiro…）可整包加载。
- **建议按需安装**：官方明确提示 163 个 skill 是很大的上下文，建议按主题装子集，而非全量。

---

## 3. 安装方式（官方）

| 方式 | 命令 | 说明 |
|------|------|------|
| npx（标准安装器） | `npx skills add K-Dense-AI/scientific-agent-skills` | 交互选择 skill，常用 `-y -g --agent <name>` 非交互；通常装到 `~/.agents/skills/` |
| GitHub CLI | `gh skill install K-Dense-AI/scientific-agent-skills` | v2.90+；可装单个 skill（如 `scanpy`）、按 host（`--agent cursor`）装，自动记录 provenance；`--pin v2.65.0` 固定版本，`gh skill update` 更新 |
| Agent Plugins | symlink/复制到插件目录 | Cursor：`ln -s "$(pwd)" ~/.cursor/plugins/local/scientific-agent-skills`；Codex：`codex plugins install .` |

**本机安装实录**：npx 交互式安装器在非 TTY 下被取消（需要 `--agent '*'` 参数）；改用手动方案——从 tarball 复制 163 个 skill 到 `~/.agents/skills/`，并为每个在 `~/.claude/skills/` 建 symlink（163/163 无 broken），验证全部 `SKILL.md` frontmatter 正常。

---

## 4. 官方 19 个技能分类总览

> 以下为 README 官方分类（部分 skill 跨分类计数，故分类计数之和 > 163）。

| 分类 | 数量 | 代表 skill / 能力 |
|------|-----:|-------------------|
| 🧬 Bioinformatics & Genomics | 27 | Bulk RNA-seq 端到端、Scanpy / scVelo / Arboreto、BioPython / BioServices、gget、Genomic Coordinates（BED/GFF/GTF/VCF 0/1-based 纠错）、OneKGPd（1000 Genomes 个体级查询）、Genomic Intelligence、PyDESeq2、Pathway Enrichment（ORA/GSEA/ssGSEA）、ETE Toolkit、Waypoint 微生物组基础模型 |
| 🧪 Cheminformatics & Drug Discovery | 10 | RDKit / Datamol / Molfeat、DeepChem / TorchDrug、DiffDock、OpenMM + MDAnalysis、Rowan 云量子化学、MedChem、PyTDC |
| 🔬 Proteomics & Mass Spectrometry | 2 | matchms、pyOpenMS |
| 🏥 Clinical Research & Evidence | 8 | PK/PD 建模（NCA/群体 PK/TMDD/PBPK/RSABE-ABEL 生物等效性/首剂人体剂量/DDI ICH M12/浓度-QTc/贝叶斯 TDM，纯 stdlib 实现）、DepMap、Imaging Data Commons、PyHealth、Clinical Decision Support（仅研究用） |
| 🐭 Preclinical & Animal Welfare | 1 | RELSA 严重度评估（ARIMA 人道终点预测 + KDE 危险区，EU 2010/63 报告辅助） |
| 🖼️ Medical Imaging & Pathology | 4 | pydicom 3.0.2（隐私优先本地 preflight）、histolab / PathML、DeepSpot-M 虚拟空间转录组 |
| 🧠 Neuroscience & Electrophysiology | 3 | BIDS、Neuropixels-Analysis（spike sorting）、NeuroKit2 |
| 🤖 Machine Learning & AI | 14 | PyTorch Lightning、Transformers、Stable Baselines3、PufferLib 3.0/4.0、scikit-learn、scikit-survival、SHAP、aeon、TimesFM（Google 零样本时序预测）、PyMC、PyMOO、Torch Geometric、UMAP、statsmodels |
| 🔮 Materials, Chemistry & Physics | 7 | pymatgen + Materials Project、COBRApy、Astropy、Cirq / PennyLane / Qiskit / QuTiP 5.3 |
| ⚙️ Engineering & Simulation | 6 | 流体/工程仿真、lab-hardware-cad 等 |
| 📊 Data Analysis & Visualization | 22 | matplotlib、networkx、geopandas、liteparse、markitdown、exa-search 等 |
| 🧪 Laboratory Automation | 6 | Opentrons、Lab Archive 集成、ginkgo-cloud-lab 等 |
| 🔬 Multi-omics & Systems Biology | 3 | 多组学生物标志物发现、系统生物学网络分析（COBRApy 系） |
| 🧬 Protein Engineering & Design | 4 | ESM、DiffDock、分子动力学等 |
| 📚 Scientific Communication | 27 | citation-management、docx、latex-posters、infographics、markdown-mermaid-writing、literature-review 等 |
| 🔬 Databases & Data Access | 11 → 100+ | Hugging Science（17 科学领域数据集/模型）、OneKGPd、Genomic Intelligence、Ontology Term Resolution（EBI OLS4）、Pathogen Variant Surveillance（GenSpectrum LAPIS：SARS-CoV-2/流感 H5N1/RSV/mpox/dengue，实时 pango 谱系） |
| 🔧 Infrastructure & Platforms | 11 | Modal、Optimize for GPU（CuPy/Numba CUDA/Warp/cuDF/cuML…）、DNAnexus、LatchBio、Nextflow + nf-core、pacsomatic、OMERO、Autoskill、Pi Agent |
| 🎓 Research Methodology & Planning | 13 | Scientific Brainstorming、Hypothesis Generation、HypoGeniC/HypoRefine、Arbor（假设树迭代优化）、What-If Oracle、Consciousness Council、DHDNA Profiler、Research Grants、Paper Lookup（10 个学术库） |
| ⚖️ Regulatory & Standards | 2 | ISO 13485/14971/17025/15189 证据准备、Analytical Method Validation（ICH Q2(R2)/Q14/M10、USP、CLSI、ISO/IEC 17025，stdlib-only 无网络） |

---

## 5. 特色机制与设计细节

- **程序性知识 > 提示词**：每个 skill 把"某类科研任务怎么做"编码成可执行工作流（含依赖管理、边界条件、输出契约），模型拿到的是"程序"而不是"建议"。
- **运行时隔离**：大量 skill 用 `uv` 按需创建临时环境，避免污染全局 Python；纯 stdlib 实现（如 PK/PD、方法验证统计）保证离线可用、无专有软件依赖。
- **数据访问层**：Database Lookup 一个入口对接 ClinicalTrials.gov、ClinVar、ClinPGx、COSMIC、FDA、cBioPortal、Monarch 等；Paper Lookup 对接 10 个学术数据库。
- **供应链与质量**：GitHub Actions 有 security-scan 与 skill-tests 流水线；`gh skill` 安装记录 provenance 元数据；版本可 `--pin` 固定。
- **边界声明制度化**：临床、动物福利、ISO 合规、学术评价类 skill 均在 README/SKILL.md 中声明"不做最终决定/不评估个人/不替代认证"——这是该库与学术写作库不同的核心设计取向。

## 6. 使用注意

- **上下文成本**：163 个 skill 全装会显著增加上下文，官方建议按主题安装子集（如 `gh skill install ... scanpy`）。
- **网络依赖**：Exa、Parallel、Benchling、NCBI、Materials Project 等 skill 需要 API key 或联网；NemoClaw 默认拒绝出站网络，需预批准域名。
- **兼容性**：本机已验证 Claude Code 可发现全部 163 个 skill（`~/.agents/skills` 实体 + `~/.claude/skills` symlink，0 broken）。
- **与 academic-research-skills 的定位差异**：前者（Imbad0202）聚焦"论文写作/评审流水线"，本库聚焦"科研计算全流程"（组学/化学/临床/仿真/合规），两者互补。

---

## 7. 一句话定位

**给"想让 AI 真正上手做科研"的人**——163 个经过验证的科学技能把编程 agent 变成覆盖组学分析、药物发现、临床建模、实验自动化、论文写作与法规合规的"AI Scientist"工作台；不替你决定，但替你执行每一步可复现的科研计算。
