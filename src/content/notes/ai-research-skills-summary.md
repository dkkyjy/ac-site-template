---
title: "AI Research Skills — 98 个 AI 研究工程技能库"
category: "技能库 / 框架"
description: "Orchestra-Research/AI-Research-SKILLs 完整总结：98 个 AI 研究 skill（23 个分类），以 autoresearch 双循环编排层为核心，覆盖从文献调研、训练微调、推理服务到论文写作的 AI 研究全生命周期；含 9 个 agent 全自动产出论文的 demo。"
pubDate: "2026-09-11"
badge: "guide"
tags: ["Orchestra", "ai-research", "skills", "claude-code", "autoresearch", "mlops", "agent"]
---
# AI Research Skills — 98 个 AI 研究工程技能库

> 仓库地址：https://github.com/Orchestra-Research/AI-Research-SKILLs
> 研究日期：2026-09-11 | 材料：仓库浅克隆（HEAD `773a529`，42MB / 400 个 md 文件）+ 本地实装验证
> 本机状态：**已安装 98 个 skill**（`~/.agents/skills/` 实体 + `~/.claude/skills/` symlink 全部有效）

---

## 1. 项目档案

- **作者**：Orchestra Research（https://www.orchestra-research.com）
- **许可证**：MIT（个别 skill 引用的第三方库各有其许可）
- **版本**：仓库 v1.7.2（2026-06-15）；npm 包 `@orchestra-research/ai-research-skills@1.7.2`
- **体量**：**98 个 skill / 23 个分类**；42MB；每个 skill = `SKILL.md`（50–150 行速查）+ `references/`（300KB+ 深度文档）
- **形态**：既是 [Agent Skills](https://agentskills.io/) 标准技能库，也是 Claude Code Plugin（`.claude-plugin/marketplace.json`）
- **配套**：npm 交互式安装器 + Orchestra 官网一键加入项目
- **构建方式**：Claude Code 结对编程 + [Skill Seeker](https://github.com/yusufkaraaslan/Skill_Seekers) 自动抓取官方文档

**一句话定位**：把工程技能按 AI 研究的**真实工作流顺序**排成一条流水线——最前面是一个 `autoresearch` 编排层，后面是 97 个"某一环该怎么做"的专家技能，让 agent 能自主跑完"选题 → 实验 → 写论文"。

---

## 2. 设计理念

- **编排层 + 专家层双层结构**：`autoresearch` 用**双循环架构**（inner loop 快速实验迭代、outer loop 综合结果并调整研究方向）统管全流程，按需路由到下游领域 skill。这是本库与"技能堆砌型"仓库最大的区别——**先有 orchestrator，再有 executors**。
- **质量优先于数量（Quality > Quantity）**：官方明确声明遵循 [Anthropic Skills 最佳实践](anthropic_official_docs/best_practices.md)，每个 skill 提供 200–500 行聚焦、可执行的指导，采用**渐进式披露**（progressive disclosure）——`SKILL.md` 只给速查，深水区放 `references/`。
- **文档源自真实战场**：`references/` 里的 `issues.md` 直接收录**真实 GitHub issue 及其解法**，`releases.md` 记录版本历史与破坏性变更。官方自称文档来自"official repos + real GitHub issues + battle-tested production workflows"。
- **标准化格式**：

  ```
  skill-name/
  ├── SKILL.md          # 速查（元数据 / 何时用 / 快速模式 / 链接）
  ├── references/       # 深度文档
  │   ├── README.md     # 来自官方仓库
  │   ├── api.md        # API 参考
  │   ├── tutorials.md  # 分步教程
  │   ├── issues.md     # 真实 issue 与解法
  │   ├── releases.md   # 版本变更
  │   └── file_structure.md  # 代码导航
  ├── scripts/          # 可选辅助脚本
  └── assets/           # 模板与示例
  ```

- **工程化治理**：CI 里有 `check-inventory.sh`（**库存漂移守卫**，文档里的 skill/分类计数一旦与磁盘实际不符就 fail CI）与安全扫描流水线。v1.7.1 的更新说明坦承曾把 90/86/87 等过时数字散落在各文件里，随后用 CI 锁死。

---

## 3. 安装方式（官方）

| 方式 | 命令 | 说明 |
|------|------|------|
| **交互式安装器**（推荐） | `npx @orchestra-research/ai-research-skills` | 自动探测本机 agent，装到 `~/.orchestra/skills/` 并 symlink 到各 agent；可选全量 / 快速包 / 按分类 / 单个 |
| **CLI 管理** | `npx @orchestra-research/ai-research-skills list` / `update` | 查看 / 更新已装 skill |
| **Claude Code Marketplace** | `/plugin marketplace add orchestra-research/AI-research-SKILLs`<br>`/plugin install fine-tuning@ai-research-skills` | 按分类装（23 个分类可选） |
| **让 agent 自己装** | 把 `https://www.orchestra-research.com/ai-research-skills/welcome.md` 丢给 agent | 官方为 AI agent 准备的自动安装入口 |
| **通用 skills CLI**（本机采用） | `npx skills add <路径> -g --all --full-depth` | 装进 `~/.agents/skills/`，再由各 agent symlink 引用 |

**本机安装实录**：仓库浅克隆到本地后，用通用 `skills` CLI 全量安装：

```bash
npx skills add /path/to/AI-Research-SKILLs -g --all --full-depth
```

- **必须带 `-g`**：不带 `-g` 会装成"项目级"（写进仓库内的 `.agents/skills/`），全局 `~/.agents/skills/` 不会变。
- 结果：`~/.agents/skills/` 从 **360 → 456**（+96），98 个 skill 全部就位（脚本核对 **0 缺失**）；Claude Code / Codex symlink 自动创建。
- 少数 agent（Eve、PromptScript）提示不支持全局安装，属预期跳过。

---

## 4. 23 个分类 / 98 个技能总览

> 以下计数为**本地实测**（遍历 `SKILL.md` 的 `name` 字段统计）。README 的汇总表有部分过时数字（如 Multimodal 标 7 实为 10、Agents 标 4 实为 5）。

| 分类 | 数量 | 包含技能 |
|------|-----:|---------|
| 自主研究编排 | 1 | `autoresearch` |
| 模型架构 | 5 | `implementing-llms-litgpt`, `mamba-architecture`, `nanogpt`, `rwkv-architecture`, `distributed-llm-pretraining-torchtitan` |
| 分词器 | 2 | `huggingface-tokenizers`, `sentencepiece` |
| 微调 | 4 | `axolotl`, `llama-factory`, `peft-fine-tuning`, `unsloth` |
| 机制可解释性 | 4 | `nnsight-remote-interpretability`, `pyvene-interventions`, `sparse-autoencoder-training`, `transformer-lens-interpretability` |
| 数据处理 | 2 | `nemo-curator`, `ray-data` |
| 后训练 / RLHF | 8 | `grpo-rl-training`, `miles-rl-training`, `openrlhf-training`, `simpo-training`, `slime-rl-training`, `torchforge-rl-training`, `fine-tuning-with-trl`, `verl-rl-training` |
| 安全与对齐 | 4 | `constitutional-ai`, `llamaguard`, `nemo-guardrails`, `prompt-guard` |
| 分布式训练 | 6 | `huggingface-accelerate`, `deepspeed`, `training-llms-megatron`, `pytorch-fsdp2`, `pytorch-lightning`, `ray-train` |
| 基础设施 / 云算力 | 3 | `lambda-labs-gpu-cloud`, `modal-serverless-gpu`, `skypilot-multi-cloud-orchestration` |
| 推理优化 / 量化 | 7 | `awq-quantization`, `quantizing-models-bitsandbytes`, `optimizing-attention-flash`, `gguf-quantization`, `gptq`, `hqq-quantization`, `ml-training-recipes` |
| 评测 | 3 | `evaluating-code-models`, `evaluating-llms-harness`, `nemo-evaluator-sdk` |
| 推理服务 | 4 | `llama-cpp`, `sglang`, `tensorrt-llm`, `serving-llms-vllm` |
| MLOps / 实验追踪 | 4 | `mlflow`, `experiment-tracking-swanlab`, `tensorboard`, `weights-and-biases` |
| Agent 框架 | 5 | `evolving-ai-agents`, `autogpt-agents`, `crewai-multi-agent`, `langchain`, `llamaindex` |
| RAG / 向量库 | 5 | `chroma`, `faiss`, `pinecone`, `qdrant-vector-search`, `sentence-transformers` |
| 提示工程 | 4 | `dspy`, `guidance`, `instructor`, `outlines` |
| 可观测性 | 2 | `langsmith-observability`, `phoenix-observability` |
| 多模态 | 10 | `audiocraft-audio-generation`, `blip-2-vision-language`, `clip`, `evaluating-cosmos-policy`, `llava`, `fine-tuning-serving-openpi`, `fine-tuning-openvla-oft`, `segment-anything-model`, +2 |
| 新兴技术 | 6 | `knowledge-distillation`, `long-context`, `model-merging`, `model-pruning`, `moe-training`, `speculative-decoding` |
| 论文写作 / 绘图 | 4 | `academic-plotting`, `ml-paper-writing`, `presenting-conference-talks`, `systems-paper-writing` |
| 研究构思 | 2 | `brainstorming-research-ideas`, `creative-thinking-for-research` |
| Agent 原生研究产物 | 3 | `ara-compiler`, `ara-research-manager`, `ara-rigor-reviewer` |

**几个值得注意的点**：

- **多模态分类实际混入了机器人策略/VLA 内容**（`cosmos-policy`、`openpi`、`openvla-oft`），并非纯视觉-语言模型，分类边界略宽。
- **后训练占 8 个**，是单类最大的一块，且把当下 RLHF 生态几乎一网打尽（TRL / GRPO / OpenRLHF / SimPO / verl / slime / miles / torchforge）。
- **`22-agent-native-research-artifact`（ARA）** 是很有想法的一类：编译器（把研究过程编译成结构化产物）、研究管理器、**Rigor Reviewer（严格性审查员）**——试图把"科研严谨性"也做成可执行的 skill。
- **`anthropic_official_docs/`** 是唯一非 skill 目录，收录 Anthropic 官方 Skills 最佳实践，作为全库的格式与质量基准。

---

## 5. 特色机制：9 个 Demo，2 篇 agent 全自动产出的论文

官方维护了一批 demo 仓库展示"skills 真的能干活"，其中两篇论文是 agent 用 `autoresearch` 端到端写出来的：

| Demo | 用到的 skills | 做了什么 |
|------|--------------|---------|
| **Norm Heterogeneity → LoRA Brittleness** | autoresearch, ML Paper Writing, Ideation | agent 自主发现"范数异质性可预测微调难度"（**r = −0.99**），并且在原假设（ETF 重叠）被证伪后**自行转向**找到了更强的结论 |
| **RL Algorithm Brain Scan** | autoresearch, GRPO, TRL, SAELens, TransformerLens, ML Paper Writing | agent 训练 RL 模型并用可解释性工具分析内部，得出结论：**DPO 是一个秩-1 扰动**（单个 SVD 方向即可恢复 95.6%），而在线 RL 是分布式、保结构的 |
| NeMo Eval: GPQA | nemo-evaluator-sdk | 对比 Llama 8B/70B/405B 在研究生级科学题上的表现 |
| LoRA Without Regret 复现 | GRPO, TRL | 用提示复现 SFT + GRPO 实验 |
| Layer-Wise Quantization | llama.cpp, GGUF | 早期层用 Q8 达成 1.9× 压缩、仅 1.3% 困惑度损失 |
| Cross-Lingual Alignment | FAISS | 量化多语言 embedding 在 8 种语言间的语义对齐 |
| Scientific Plotting | academic-plotting | 为论文生成出版级图表（架构图 + CDF / 多面板 / 柱状图） |

**两个"自主性"信号**：一是 agent **否证了自己的假设并改道**（Norm Heterogeneity 那个 demo），二是 **多 skill 编排**（RL Brain Scan 里 agent 自己训模型再自己解剖）。这两点比"能写出 LaTeX"更能说明编排层的价值。

---

## 6. 使用注意

- **上下文成本**：98 个 skill 全装是显著的上下文负担。官方在科学技能库那套里也建议按主题装子集，这里同样适用——Claude Code Marketplace 支持 `--plugin install <分类>` 就是为此设计。
- **本机与其他技能库的同名覆盖**：安装时发现 2 个 skill 与既有安装**同名**，被 Orchestra 版本覆盖：
  - `autoresearch` —— 原先存在的是 **OMX 蒸馏版**（`[OMX] Stateful validator-gated research loop`），已被 Orchestra 版覆盖；OMX 的技能清单（`~/.omx/state/setup/installed-skills.json`）里仍记录其哈希。旧版备份在 `/tmp/autoresearch_omx_backup`。
  - `pytorch-lightning` —— 原先来自 **scientific-agent-skills** 库，已被本次覆盖；旧版原始文件仍在 `/tmp/scientific-agent-skills-main/skills/pytorch-lightning`。
  - 两者均可恢复，如果日后 `omx` 报 skill 校验错，大概率源于此。
- **网络依赖**：部分 skill 的 `references/` 抓取自官方文档，首次使用时可能仍需联网查看上游 API 细节；云算力类（Modal / Lambda Labs / SkyPilot）需要相应账号与凭证。
- **与 neighbouring 技能库的定位差异**：
  - **AI Research Skills（本库）**：以 `autoresearch` 编排层为核心，覆盖**AI/ML 训练的工程全链路**（架构 → 微调 → 后训练 → 推理 → 评测 → 论文）。
  - **scientific-agent-skills（K-Dense）**：覆盖**自然科学科研计算**（组学、化学、临床、仿真、法规），两者互补而非竞争。
  - **academic-research-skills**：聚焦论文写作/评审流水线。

---

## 7. 一句话定位

**给"想让 AI 自己跑一个研究项目"的人**——`autoresearch` 编排层加 97 个专家技能，把训练框架、量化方案、推理服务、评测工具和论文写作全部编码成可调用知识，最终产出的是能自己改道的实验结论，而不是一段漂亮但没跑过的代码。
