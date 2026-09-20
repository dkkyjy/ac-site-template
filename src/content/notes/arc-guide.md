---
title: "ARC — 理论物理论文研究 Agent 指南"
category: "专项工具 / 科研"
description: "Agent Research Copilot (ARC) 的功能地图与用法：论文读取层 arc-paper、研究域构建 arc-domain、五个托管科研 workflow，附本机安装与实测踩坑记录。"
pubDate: "2026-09-20"
badge: "guide"
tags: ["arc", "research-agent", "arxiv", "inspire", "theoretical-physics"]
---

# ARC — 理论物理论文研究 Agent 指南

> 一句话：ARC 不是「帮你读论文的脚本」，而是把**理论物理论文获取、结构化阅读、研究域构建、想法生成、计算核查**封装成一套带持久化状态和证据链的 agent 工具层。

- 仓库：[tririver/arc](https://github.com/tririver/arc)（v2.0.3，stable 分支）
- 依赖：[tririver/ac-foundation](https://github.com/tririver/ac-foundation)（durable jobs / LLM 调用 / 中性文档层 / proposer-reviewer 编排）
- 论文引用：Yanjiao Ma, Yi Wang, Xingkai Zhang. *ARC: An LLM-Native Agent Workflow for Theoretical Physics Research*. ChinaXiv:202606.00234
- 本机路径：`~/storage/github/arc`（源码）+ `~/storage/github/ac-foundation`（依赖，锁定 commit `65c9c442`）+ `~/.ac/runtimes/v1/arc/f3bd4c2b…`（SHA 锁定运行时，doctor `ready=true`）

---

## 1. 它到底解决什么问题

arXiv 上的论文，人读得动，脚本却很难「读对」：HTML 版有章节结构但公式编号与印刷版不一致，PDF 有排版但抽取易错，INSPIRE 有引用关系但不含正文。ARC 的做法是**分两层**：

1. **确定性层（`arc-paper` / `ac-document`）**：纯工具，无 LLM 参与。拿元数据、目录、章节正文、参考文献、引用者、全文检索、公式检索，全部返回 typed JSON envelope。
2. **模型主导层（`arc-domain` + 五个 workflow）**：由 LLM 完成域构建、想法生成与评审、计算规划与核查，但**每一句科学断言都要挂证据**（论文 id + 章节/公式引用），并且全程写入项目本地持久状态，可暂停、可恢复、可审计。

### 设计哲学（摘自仓库 `AGENTS.md`）

- **基础设施而非流水线**：给模型信息与反馈，信任模型做科学判断；不要把可争辩的科学品味编码成硬路由、删除、取消资格。
- **硬停止只留给系统边界**：缺权限、状态损坏、机器契约不可用、危险操作、完全无结果——仅此而已。交付失败不得抹掉已完成的科学工作。
- **隐藏状态不能是唯一交付物**：叙述性报告必须发布为 HTML/PDF，机器可读产物（JSON/CSV/图数据）保留原生格式。
- **缓存优先（cache-first）**：同一篇论文只取一次，后续所有操作走本地缓存。

---

## 2. 架构：三个包 + 一个运行时

```
arc (插件仓库)
├── plugins/arc/bin/{arc-runtime, arc-paper, arc-domain}   # 宿主可见的 3 个入口
├── plugins/arc/skills/arc/                                # agent 侧 skill：manuals / rules / workflows
│   ├── manuals/{arc-paper,arc-domain,ac-document,ac-llm,ac-jobs,ac-proposer-reviewer}.md
│   ├── rules/{interaction,integrity,operating,math_typeset,self-reflection}.md
│   └── workflows/{domain,ideas,plan,calculate,check}.md
└── packages/{arc-paper, arc-domain}                       # Python 包（+ ac-foundation 提供其余）
```

| 组件 | 角色 |
| --- | --- |
| `arc-paper` | arXiv / INSPIRE / DOI 论文获取、缓存、结构化阅读、引用遍历、摘要 |
| `arc-domain` | 从一个种子论文出发构建「带证据的研究域」（引用网络 + 排序 + 导出） |
| `ac-document` | provider 无关的本地文档导入、解析、冻结读取、关键词清单、富文档导出 |
| `ac-llm` | typed LLM 请求（provider 选择、durable 状态、可恢复执行） |
| `ac-jobs` | 检查/校验/停止上层命令创建的 durable run，调整并发 |
| `ac-proposer-reviewer` | typed、durable 的 proposer–reviewer 批次（想法生成的核心引擎） |

**运行时是懒安装 + SHA 锁定的**：首次真实 CLI 调用时才把 ac-foundation 与 arc 的精确 Git revision 装进 `~/.ac/runtimes/v1/arc/<hash>/`，所以插件本身很轻。

---

## 3. 安装

### 3.1 官方路径（宿主插件）

```bash
# Codex
codex plugin marketplace add tririver/arc --ref stable
codex plugin add arc@arc

# Claude Code
/plugin marketplace add tririver/arc@stable
/plugin install arc

# DeepSeek Harness
dsh plugin --profile arc add github:tririver/arc
```

预检 / 预热运行时：

```bash
plugins/arc/bin/arc-runtime doctor    # ready=true 表示 6 个工具齐备
plugins/arc/bin/arc-runtime setup
```

### 3.2 本机离线路径（github.com 直连不通时）

实测：`github.com:443` 不可达，但 `codeload.github.com` / `api.github.com` / `pypi.org` 可达，所以用 tarball + local 源（两个目录都**不是 git 检出**，没有 `.git`）：

```bash
# 1) 两个仓库都以 tarball 解压落盘
#    ~/storage/github/arc            ← arc stable 分支（VERSION = 2.0.3）
#    ~/storage/github/ac-foundation  ← commit 65c9c442…，由 arc 锁定（见下）

# 2) 4 个环境变量缺一不可
export AC_FOUNDATION_REPO_ROOT=~/storage/github/ac-foundation
export AC_PRODUCT_REPO_ROOT=~/storage/github/arc
export AC_INSTALL_SOURCE=local
export AC_INSTALL_PYTHON_BIN=/opt/homebrew/bin/python3.12   # 需 ≥3.11

# 3) 安装并检查
cd ~/storage/github/arc && ./plugins/arc/bin/arc-runtime setup
./plugins/arc/bin/arc-runtime doctor   # ready=true；sources[].commit 就是 ac-foundation 的锁定 commit
```

> ⚠️ 本机系统 `python3` 是 anaconda 3.10.8，**不满足 Python ≥3.11**，必须显式指定 Homebrew 的 3.12。
> ⚠️ ac-foundation 的 commit 由 `plugins/arc/skills/arc/scripts/runtime-sources.json` 锁定，版本不匹配会在 setup 阶段报错；`doctor` 的 `sources` 字段会回报实际使用的 commit 与内容 sha256。

Claude Code 挂载本地插件（实测可用）：

```bash
claude plugin marketplace add /Users/dkk/storage/github/arc
claude plugin install arc@arc        # → v2.0.3 enabled
```

---

## 4. `arc-paper`：确定性论文读取层

### 4.1 命令全景（`--help` 实抓，共 34 个子命令）

| 类别 | 命令 |
| --- | --- |
| 标识符 | `extract-paper-ids`、`safe-dir-name` |
| 元数据 | `get-title`、`get-abstract`、`get-authors`、`get-metadata`、`get-citer-count`、`search-metadata` |
| 结构阅读 | `get-table-of-contents`、`get-section`、`read-cached-source-range`、`reconstruct-cached-structure` |
| 检索 | `search-full-text`、`search-equations`、`extract-keywords` |
| 引用 | `get-references`、`get-citers`、`search-citers`、`lookup-reference`、`admit-reference`、`materialize-reference` |
| 获取/缓存 | `fetch-arxiv-auto`、`fetch-arxiv-pdf`、`fetch-arxiv-html-bundle`、`export-arxiv-html-bundle`、`export-arxiv-html-acquisition`、`import-source`、`parse-local`、`export-rich-document`、`cache` |
| 运维 | `status`、`stop`、`validate` |

### 4.2 典型阅读循环（以 arXiv:0911.3380 实测）

```bash
arc-paper get-metadata arXiv:0911.3380
arc-paper get-table-of-contents --reference 0911.3380 --source-format html
arc-paper search-full-text     --reference 0911.3380 --source-format html --term "isocurvaton" --context-lines 1
arc-paper get-section          --reference 0911.3380 --source-format html "Conclusion"
arc-paper search-equations     --reference 0911.3380 --source-format pdf  --term "2.30" --context-lines 8
```

### 4.3 实测结果（真实数字）

| 操作 | 结果 |
| --- | --- |
| `fetch-arxiv-auto` | 抓到 arXiv HTML v4 全文（1.09 MB）并落缓存 |
| `get-metadata` | 标题正确；`citation_count` = **662**；DOI `10.1088/1475-7516/2010/04/027`；`identifiers` 含 `inspire:837197` |
| `get-citer-count 0911.3380` | **662**（与 metadata 的 `citation_count` 一致） |
| `get-table-of-contents` | 完整章节树，每节带 `section_id`，可直接喂给 `get-section` |
| `get-section ... "Conclusion"` | 抽出正文 7169 字符 |
| `get-references` | **82** 条参考文献（带 INSPIRE recid） |
| `search-full-text --term inflation` | **121** 处命中 |
| `get-citers --limit 3` | 返回引用论文的完整对象（abstract 等字段俱全） |
| `extract-paper-ids` | `"see arXiv:0911.3380v4 and 10.1088/…"` → `["arXiv:0911.3380", "doi:10.1088/…"]` |

### 4.4 坑（都是实测踩到的）

1. **参数风格不统一**：`get-table-of-contents` / `get-section` / `search-full-text` 用 `--reference`，而 `get-citers` / `get-citer-count` / `extract-paper-ids` 用**位置参数**。写脚本前务必 `arc-paper <cmd> --help`。
2. **全文检索的结果字段是 `occurrences`，不是 `matches`**——按 `matches` 取值会看到「0 命中」的假象。
3. **`search-equations` 在 arXiv HTML 源上恒 0 命中**：HTML 转换器生成的公式标签与论文印刷编号（如 2.30）对不上；manuals 的建议是用 `--source-format pdf`（本机未验证）。
4. **空结果 ≠ 出错**：必须检查 `status`、`warnings`、`error` 和 `data` 下的具体字段，否则会把「参数写错」误判为「论文里没有」。
5. 缓存默认落在**当前工作目录**的 `.arc/cache/arc-paper`，换目录就换缓存；跨命令要一致，或统一设 `ARC_PAPER_CACHE`。

---

## 5. `arc-domain`：从种子论文到「带证据的研究域」

```bash
arc-domain build <seed_paper> --project-dir <dir> \
  --intent "研究意图" --recent-window-days 365 \
  --citer-selection-mode representative-plus-recent \
  --foundation-mode infer-from-seed \
  --llm-provider claude --model deepseek-v4-pro --model-tier medium \
  --workers 8 --host-authority unknown
```

构建流程（`workflows/domain.md` 四阶段）：

1. **Preflight & 解析域起点**：冻结 `as_of_date`；「某领域近两年」修饰的是**引用者语料**而不是种子论文本身；默认 `recent_window_days=365`。
2. **建域缓存**：拉取引用者池、排序、构建引用图（`--citer-pool-limit` / `--ranked-paper-limit` / `--graph-node-limit` 可调）。
3. **发布交付物**：
   - 可见：`<project-dir>/domain/<seed>_domain.html`、`<seed>_domain_summary.pdf`
   - 隐藏：`.arc/domain/packages/<seed>_domain_summary.{json,md}`、`<seed>_paper_json_pack.json`
   - 原则：**Markdown 不能是唯一用户可见交付**（所以有 HTML + PDF）。
4. **范围边界与人工复核**：自动域构建**不授权**想法生成，想法生成**不授权**规划或计算——每步必须显式请求。

其他子命令：`status` / `resume` / `stop` / `validate` / `get-summary` / `get-graph` / `materialize-export`（把活跃导出复制到指定路径，报告 `digest` 与 `size_bytes`，**不覆盖已有文件**）。

**有界自动重试**：域摘要模型返回结构合法但身份/证据覆盖校验失败时，会带校验反馈重试一次；再失败则暂停并保留两份候选供人工编辑。

---

## 6. 五个托管 workflow（ARC 的核心资产）

| workflow | 用途 | 关键产出 |
| --- | --- | --- |
| `domain.md` | 从种子论文构建研究域 | 域 HTML/PDF、证据包、论文包 |
| `ideas.md` | 从域卡片生成并评审研究想法 | 排序后的 ideas、每轮 proposer/reviewer 记录、组合级评估 |
| `plan.md` | 生成人可读的计算 work-note | `work-note.md` + 分步结构（ready-step 边界由它拥有） |
| `calculate.md` | 执行计算并记录结果 | 双计算器（two-calculator）执行记录、裁判信任决策 |
| `check.md` | 核查 Markdown/PDF 研究笔记 | note-check 状态、`planning-request.md` 交接 |

要点：

- **职责严格分离**：`plan.md` 拥有 work-note 结构与 ready-step 边界；`calculate.md` 拥有执行与结果记录；`check.md` 只拥有笔记解析与交接。互不越界。
- **ideas 的循环结构**：每轮恰好 1 个 proposer + 1 个 reviewer，默认最多 3 轮提交；批次结束后默认跑一次**组合级**科学评估（advisory，不改变评分/排名）。
- **Research Tools 互补**：ARC 论文工具与 web 搜索互为补充，要求「聚焦核查而非穷举搜索」，且每条来源/查询都要记录。
- **重负载不是停止条件**：workflow 明确写了 Heavy Workload Rule——工作量大不是跳过阶段的理由，除非命中具体停止条件或用户显式停止。
- **可观测性**：`ac-proposer-reviewer` 只暴露 `inspect`（尽力而为）/ `trace`（仅已提交轮次的 ref 与 revision）/ `show-round`（展开单轮 JSON）三个查询面；**禁止**直接读 durable 循环目录来替代这些查询。

### 触发策略：什么时候 ARC 会自动介入

这是 ARC 设计里最反直觉的一块（`SKILL.md` 的 Preflight Gate）：

- **隐式论文入口**：任何属于 `arc-paper` 的任务（标题/摘要/章节/公式上下文/引用者/摘要批次……）**无需点名 ARC** 即可激活；甚至「下载 2024 年以来引用 0911.3380 的论文并做摘要」这类编排也算。但隐式入口**永远不**越界到托管 workflow 或非论文的 Foundation 能力。
- **显式 ARC 入口**：只有用户点名 ARC / 调用 `$arc` / 明确延续已开始的 ARC 任务，才会路由到 `ac-llm`、`ac-jobs`、`ac-proposer-reviewer` 或五个托管 workflow。
- 默认 `automation_level: auto`，**不要在启动时让用户选执行模式**；只有用户明确要求逐步确认才切 `interactive`。
- 论文摘要类操作即使内部调用 LLM，也仍属于隐式入口——「是否用 LLM」不是边界，**包边界才是边界**。

---

## 7. 状态与缓存约定

| 路径 | 内容 |
| --- | --- |
| `~/.ac/runtimes/` | SHA 锁定的可复用运行时（AC Foundation 拥有） |
| `~/.ac/`（`AC_HOME`） | Foundation 主目录；`AC_RUNTIME_HOME` 可指定可移植位置 |
| `.ac/cache/ac-document/`（`AC_DOCUMENT_CACHE`） | 中性文档缓存 |
| `.arc/cache/arc-paper/`（`ARC_PAPER_CACHE`） | 论文缓存，默认相对**当前目录** |
| `<project-dir>/.arc/` | 项目本地 durable 状态：runs、LLM 会话与 transcript、子工作区、域状态、诊断、未发布世代 |

原则：durable run 属于**项目状态而非共享缓存**，不做 legacy 共享根的兜底发现；在 ARC 源码检出里跑生成物要落在被 gitignore 的 `local/` 下。

---

## 8. LLM 通路：本机实测与最大的坑

ARC 把宿主 AI CLI 当 provider（`ac-llm doctor` 报 `available:true` 只说明**CLI 存在**，不代表能调通）。

最小请求（`ac-llm generate --request req.json --run-root <dir>`）：

```json
{
  "schema_version": "ac.llm.request.v4",
  "task_id": "probe-1",
  "prompt": "Reply with exactly: ARC_OK",
  "output": {"kind": "text"},
  "model": {"provider": "claude", "model": "deepseek-v4-pro", "tier": "medium"},
  "session": null,
  "inputs": []
}
```

本机实测结论：

| provider | 结果 |
| --- | --- |
| `codex` | 真实启动（codex-cli 0.153.4，生成 `.omx` 会话），但本机 codex 指向 ollama 网关且未登录 → `This model requires an Ollama account`，挂到超时 |
| `claude`（默认 tier 映射） | haiku / sonnet / opus 全被代理网关 **403**：`This token does not have permission to use the model: claude-haiku-4-5-…` |
| `claude` + **显式模型名** | ✅ `status: completed`，148 s，结果制品内容 = `ARC_OK` |

**结论：ARC 的 `--model-tier` 映射（low/medium/high/xhigh）在本机代理网关下不可用，必须在 `model.model` 里显式写本机可用的模型名。**

失败时怎么查：run 目录下有 provider-failure 制品，`raw_events` 里是宿主 CLI 的真实事件流（含 403 原文），`stderr_tail` 可能为空——**只看 returncode 会误判**。

---

## 9. 实测总结与使用建议

**已验证可用**（本机 v2.0.3）：论文获取与缓存、元数据/目录/章节/全文检索、参考文献、引用者列表与计数、标识符抽取、LLM 通路（显式模型名）、Claude 插件挂载、runtime doctor。

**未验证/受限**：`search-equations` 在 HTML 源上无结果（tex/pdf 源待测）；`arc-domain build` 完整流程未跑（依赖 LLM，成本较高）；`ideas/plan/calculate/check` 四个 workflow 未实跑。

**什么时候用它**：
- 需要**可复现的论文事实**（章节原文、公式上下文、引用关系）→ `arc-paper`，比让模型自由回忆可靠得多。
- 需要**带证据链的领域地图**（谁是源头、谁在引用、最近有什么新工作）→ `arc-domain`。
- 需要**从域到想法到计算的全链路**且要求可审计、可恢复 → 五个托管 workflow（必须先点名 ARC，并接受它跑得久）。

**什么时候别用它**：只是想快速浏览一篇论文摘要——直接问模型或用普通检索更快；ARC 的价值在**结构化、可缓存、可追溯**，不在速度。

---

*本文基于本机实际安装（v2.0.3，2026-09）与官方文档/源码实测整理；数字均来自真实命令输出。*
