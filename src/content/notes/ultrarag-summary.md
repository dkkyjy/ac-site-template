---
title: "UltraRAG - 功能总结"
category: "技能库 / 框架"
description: "UltraRAG 功能总结与使用指南。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["ultrarag", "rag"]
---

# UltraRAG 功能总结与使用指南

> 基于 https://ultrarag.openbmb.cn/pages/cn/getting_started/introduction 官方文档  
> 版本: v3.0 (2026-01-23)  
> 框架: 基于 MCP（Model Context Protocol）架构的轻量级 RAG 开发框架

---

## 1. 核心理念

UltraRAG 是 **首个基于 MCP 架构的 RAG 开发框架**，由 OpenBMB 开发。它将 RAG 的核心组件标准化封装为独立的 MCP Server，通过 YAML 配置驱动的 Pipeline 实现流程编排。

### 四大组件

| 组件 | 角色 | 说明 |
|:-----|:-----|:------|
| **📑 Pipeline** | 流程定义 | YAML 编写的任务逻辑，定义组件执行顺序与业务逻辑 |
| **🕹️ Client** | 调度中枢 | 解析 Pipeline，协调各 Server 的工具调用与数据传递 |
| **⚙️ Server** | 功能执行 | 核心功能标准化封装为独立服务，支持快速扩展 |
| **🖥️ UI** | 交互演示 | 将 YAML 逻辑一键转化为对话界面 |

### 核心优势

- **🚀 低代码编排**: 原生支持串行、循环与条件分支，10 行 YAML 内实现复杂 RAG
- **⚡ 模块化解耦**: 基于 MCP 架构；新功能注册为 Tool 即可接入
- **📊 统一评测**: 内置标准化评测流程，开箱即用主流 Benchmark
- **✨ 交互 UI**: 一行命令将 Pipeline 转化为 Web UI

---

## 2. 环境部署

### 2.1 本地源码安装（推荐 uv）

```bash
# 安装 uv（包管理器）
curl -LsSf https://astral.sh/uv/install.sh | sh

# 克隆仓库
git clone https://github.com/OpenBMB/UltraRAG.git --depth 1
cd UltraRAG

# A：创建新环境
uv sync                          # 核心依赖（基础功能）
uv sync --all-extras             # 全量安装（检索/生成/语料/评测）
uv sync --extra retriever        # 仅检索模块
uv sync --extra generation       # 仅生成模块

# 激活环境
source .venv/bin/activate        # macOS/Linux

# B：安装至已有环境
uv pip install ".[all]"          # 全量
uv pip install ".[retriever]"    # 按需
```

### 2.2 Docker 部署

```bash
# 选项 A：从 Docker Hub 拉取
docker pull hdxin2002/ultrarag:v0.3.0-base-cpu    # 基础版 (CPU)
docker pull hdxin2002/ultrarag:v0.3.0-base-gpu    # 基础版 (GPU)
docker pull hdxin2002/ultrarag:v0.3.0              # 完整版 (GPU)

# 启动容器
docker run --gpus all -p 5050:5050 hdxin2002/ultrarag:v0.3.0
```

### 2.3 验证安装

```bash
ultrarag examples/sayhello.yaml
# 输出: Hello, UltraRAG v3!
```

---

## 3. 快速开始

UltraRAG 的使用分为三个阶段：

### Step 1: 编写 Pipeline 配置文件

在 `examples/` 下创建 `.yaml` 文件，包含 `servers` 和 `pipeline` 两部分：

```yaml
# examples/rag_full.yaml
servers:
  benchmark: servers/benchmark
  retriever: servers/retriever
  prompt: servers/prompt
  generation: servers/generation
  evaluation: servers/evaluation

pipeline:
  - benchmark.get_data
  - retriever.retriever_init
  - retriever.retriever_embed
  - retriever.retriever_index
  - retriever.retriever_search
  - generation.generation_init
  - prompt.qa_rag_boxed
  - generation.generate
  - evaluation.evaluate
```

### Step 2: 编译并调整参数

```bash
ultrarag build examples/rag_full.yaml
# 生成 examples/parameters/rag_full_parameter.yaml
```

修改参数文件（模型路径、数据集、检索器配置等）后保存。

### Step 3: 运行 Pipeline

```bash
ultrarag run examples/rag_full.yaml
```

运行结果自动保存至 `output/` 目录。

### Step 4: 可视化分析

```bash
python ./script/case_study.py \
  --data output/your_result.json \
  --host 127.0.0.1 --port 8501 \
  --title "Case Study Viewer"
```

---

## 4. Pipeline 示例

### 4.1 Vanilla RAG（标准 RAG）

标准 RAG 流水线：**语料编码 → 索引构建 → 检索 → 生成 → 评测**。

支持 NQ、HotpotQA、MuSiQue、2WikiMultihopQA 等多个数据集。

特殊 Pipeline 变体：
- **No Search**: 直接生成（零检索）
- **Perfect Search**: 使用 golden 段落检索
- **Corpus**: 在语料库中搜索

### 4.2 VisRAG（多模态 RAG）

构建个人知识库——对 PDF、截图、说明书等视觉文档进行 RAG。支持：
- 文档解析与视觉编码
- 多模态检索
- 图文结合的生成回答

示例场景：尼康 Z7 相机说明书问答。

### 4.3 Search-o1（Agentic RAG）

将大规模推理模型与自主检索增强生成结合。核心特征：
- **推理时检索**: 模型在思考过程中自主决定何时检索
- **文档内推理 (Reason-in-Documents)**: 在检索到的文档中进行推理
- 适用于需要多步推理的复杂问答

### 4.4 WebNote (Light DeepResearch)

Deep Research 的轻量级实现——LLM 协同工具（搜索引擎、浏览器）进行多轮自主搜索与综合报告生成。已有配套教学视频（Bilibili）。

---

## 5. RAG Client（流程控制）

### 5.1 Pipeline 结构

Pipeline 是 YAML 定义的任务计划表，明确每一步的操作。支持多层嵌套结构。

```yaml
pipeline:
  - module1.tool1
  - module2.tool2
  - - sub_pipeline_1        # 子流程
    - sub_pipeline_2
```

### 5.2 控制结构

| 结构 | 关键字 | 说明 |
|:-----|:-------|:------|
| **串行 (Serial)** | 按顺序排列 | 基础执行方式，前一步输出可作下一步输入 |
| **分支 (Branch)** | `branch` + `switch` | 根据条件跳转，如判断是否继续检索 |
| **循环 (Loop)** | `loop` + `while` | 重复执行直到满足条件 |
| **多智能体** | `agents` | 多个 Agent 并行协作，共享上下文 |

**分支示例**:
```yaml
pipeline:
  - agent.step1
  - branch:
      switch: agent.need_retrieval()    # 条件函数
      cases:
        True: retriever.search          # 需要则检索
        False: generation.generate      # 否则直接生成
  - generation.final_answer
```

**循环示例**:
```yaml
pipeline:
  - loop:
      while: "len(context) < 5"         # 循环条件
      body:
        - search.next_query
        - retriever.retrieve
        - context.append
  - generation.summarize
```

### 5.3 数据与参数

- 通过 `parameter.yaml` 管所有可配置项
- 支持 `key_map`、`shuffle`、`template` 等数据预处理
- 编译时自动生成参数文件，支持按需修改

---

## 6. RAG Servers（功能模块）

### 6.1 Server 体系

| Server | 功能 | 支持的后端/工具 |
|:-------|:-----|:----------------|
| **Corpus** | 语料管理 | HuggingFace / ModelScope / 本地 |
| **Retriever** | 检索 | BM25, Embedding (Sentence-BERT/OpenAI), Milvus, Faiss |
| **Reranker** | 重排序 | BGE-Reranker, Cohere |
| **Generation** | 生成/LLM | vLLM, OpenAI API, MiniCPM, Qwen |
| **Prompt** | 提示词管理 | Jinja2 模板引擎 |
| **Benchmark** | 基准数据 | NQ, HotpotQA, MuSiQue, 2WikiMultihop |
| **Evaluation** | 评测 | F1, ExactMatch, ROUGE, CoverEm, StringEm |
| **Router** | 路由 | 按规则分发请求 |
| **Custom** | 自定义 | 用户自行注册 Tool |

### 6.2 Retriever（检索器）

支持三大检索策略：

```
BM25 检索        → 关键词匹配，无需 GPU
Embedding 检索    → 向量相似度，支持多种嵌入模型
Web Search API   → 在线搜索（支持自定义 API）
```

检索配置项丰富：
- 嵌入模型：`text-embedding-3-small`, `MiniCPM-Embedding`, `Qwen-Embedding`
- 索引后端：`BM25`, `Faiss`, `Milvus`（`AUTOINDEX` 类型）
- 编码参数：`batch_size`, `normalize_embeddings`, `pooling_method`
- 检索参数：`top_k`, `search_params`

### 6.3 Generation（生成器）

支持主流推理/对话模型：

| 后端 | 模型示例 | 说明 |
|:-----|:---------|:------|
| vLLM | Qwen3-8B, MiniCPM4-8B | 本地部署，高性能 |
| OpenAI API | GPT-4o, GPT-4o-mini | 在线调用 |
| SGLang | 自定义 | 灵活后端 |

支持 `thinking` 模式（`enable_thinking: true`），先进的采样参数控制。

### 6.4 Server 扩展开发

三步创建自定义 Server：
1. 创建 `servers/custom/` 下的 Server 文件
2. 实现 Tool 函数（函数级接口）
3. 配置 `parameter.yaml`（含参数注册机制）

支持类封装共享变量。

---

## 7. 开发与评测

### 7.1 代码集成

两种方式在本地代码调用 UltraRAG 能力：

```python
# ToolCall：调用单个功能
from ultrarag import initialize
retriever = initialize("servers/retriever")
results = retriever.search(query="...")

# PipelineCall：运行完整流程
from ultrarag import run_pipeline
results = run_pipeline("examples/rag.yaml")
```

### 7.2 数据集

提供预处理好的公开评测集（HuggingFace / ModelScope 发布）：

| 数据集 | 类型 | 说明 |
|:-------|:-----|:------|
| NQ | 单跳问答 | Natural Questions |
| HotpotQA | 多跳问答 | 需多步推理 |
| MuSiQue | 多跳问答 | 复杂推理 |
| 2WikiMultihopQA | 多跳问答 | Wikipedia 知识 |

### 7.3 评测指标

| 指标 | 说明 |
|:-----|:------|
| F1 / ROUGE-1/2/L | 词重叠率 |
| ExactMatch (EM) | 精确匹配 |
| CoverEm / StringEm | 覆盖/字符串匹配 |

### 7.4 并行实验

步骤：
1. 编译 Pipeline → 生成基础参数文件
2. 复制多份参数文件，修改不同超参数
3. 批量运行（不修改主 Pipeline）

---

## 8. UI 交互平台

启动命令：
```bash
ultrarag [OPTIONS]
# 默认访问 http://localhost:5050
```

### 核心界面

| 面板 | 功能 |
|:-----|:------|
| **Chat（对话）** | 交互式问答，支持 Pipeline 切换 |
| **Knowledge Base（知识库）** | 文档上传与管理 |
| **Pipeline Builder** | 可视化搭建 Pipeline（拖拽式） |
| **Prompt 管理** | 提示词模板编辑 |

UltraRAG UI 不仅是聊天界面，更是完整的 RAG 开发与调试平台。

---

## 9. 版本历史

| 版本 | 日期 | 主要更新 |
|:-----|:-----|:---------|
| v3.0 | 2026-01-23 | 🎉 首个版本——拒绝"盲盒"开发 |
| v2.1.3 | 2026-01-12 | 稳定性提升，修复 Search-o1 Bug |
| v2.1.2 | 2025-11-25 | ToolCall + PipelineCall 功能 |
| v2.1.1 | 2025-11-13 | 解耦检索/索引架构，支持 Milvus/Faiss |
| v2.1 | 2025-10-22 | RAG Servers 全面升级，多模态 RAG |
| v2.0 | 2025-08-28 | 重大架构升级 |

---

## 10. 架构总览

```
┌─────────────────────────────────────────────────────┐
│                     UltraRAG                          │
│  ┌──────────────────────────────────────────────┐   │
│  │          🕹️ RAG Client (Pipeline)             │   │
│  │   Serial · Branch · Loop · Multi-Agent        │   │
│  └──────────────────┬───────────────────────────┘   │
│                     │ YAML 配置驱动                  │
│  ┌──────────────────▼───────────────────────────┐   │
│  │          ⚙️ MCP Servers (解耦)                 │   │
│  │  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐       │   │
│  │  │Retr. │ │Gen.  │ │Eval. │ │Rerank│  ...    │   │
│  │  └──────┘ └──────┘ └──────┘ └──────┘       │   │
│  └──────────────────┬───────────────────────────┘   │
│                     │                               │
│  ┌──────────────────▼───────────────────────────┐   │
│  │    📊 统一评测 / 📚 标准数据集                │   │
│  └──────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────┐   │
│  │          🖥️ UI (对话 + Builder)               │   │
│  └──────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────┘
```

---

## 11. 与类似框架的对比

| 特性 | UltraRAG | LangChain | LlamaIndex | Haystack |
|:-----|:---------|:----------|:-----------|:---------|
| 架构 | MCP Server | Chain/Graph | Index/Engine | Pipeline |
| 配置方式 | YAML | Python DSL | Python DSL | YAML/Python |
| 控制结构 | 串/分支/循环/多Agent | LCEL 表达式 | 查询引擎 | Pipeline |
| UI 平台 | 内置 (一行命令启动) | LangSmith | 无内置 | Haystack UI |
| 评测系统 | 内置完整 | 第三方 | 基础 | 第三方 |
| 多模态 RAG | ✅ VisRAG | 第三方 | ✅ | ❌ |
| 代码集成 | ToolCall/PipelineCall | ✅ | ✅ | ✅ |
| 扩展方式 | 注册 Tool | 自定义 Chain | 自定义 Reader | 自定义 Component |

---

## 12. 快速索引

| 需求 | 参考章节 |
|:-----|:---------|
| 安装 UltraRAG | §2 环境部署 |
| 运行第一个 RAG Pipeline | §3 快速开始 |
| 使用 VisRAG 处理 PDF/图片 | §4.2 VisRAG |
| 构建 Agentic RAG (Search-o1) | §4.3 Search-o1 |
| 自定义控制流程 (分支/循环) | §5.2 控制结构 |
| 添加新的 Server 或 Tool | §6.4 Server 扩展开发 |
| 在代码中调用 UltraRAG | §7.1 代码集成 |
| 运行评测对比实验 | §7.3-7.4 评测/并行 |
| 启动 Web UI 平台 | §8 UI 交互平台 |

---

> 本文档基于 UltraRAG 3.0 官方文档 (ultrarag.openbmb.cn) 整理生成  
> GitHub: https://github.com/OpenBMB/UltraRAG
