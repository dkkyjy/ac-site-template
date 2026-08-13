---
title: "Personal Research Assistant"
description: "本地运行的 AI 天体物理研究助手（高能伽玛天文、多信使、暗物质、统计推断）— arxiv 监控、Zotero 知识库、RAG QA、Docker 沙箱内 LLM 生成分析代码，带 Web UI。"
pubDate: "Jul 03 2026"
updatedDate: "Jul 13 2026"
heroImage: "/itemPreview.png"
badge: "active"
github: "https://github.com/dkkyjy/research-assistant"
tags: ["python", "astrophysics", "arxiv", "rag", "gammapy", "ai", "fastapi", "htmx", "docker"]
---

**Personal Research Assistant** 是一个本地运行的 AI 天体物理研究助手（高能伽玛天文、多信使、暗物质、统计推断），通过三条并行管线解决信息过载、知识碎片化和分析重复三大痛点。提供 Web UI 与完整 CLI。

## 架构概览

```
                    ┌──────────────────────────────────────┐
                    │           LLM Router                  │
                    │  (skill → provider → model → fallback) │
                    └──┬──────────┬──────────┬─────────────┘
                       │          │          │
          ┌────────────┼──────────┼──────────┼────────────┐
          │            │          │          │            │
    ┌─────▼────┐  ┌───▼───┐ ┌───▼───┐ ┌───▼───┐  ┌───▼─────┐
    │Monitoring │  │Knowledge│ │Collab │ │ Web  │  │ Storage │
    │ arxiv     │  │ Zotero │ │ gen   │ │ UI   │  ├─────────┤
    │ keyword   │  │ PDF    │ │ sand- │ │ dash │  │ SQLite  │
    │ LLM filt  │  │ chunk  │ │ box   │ │ paper│  │ Qdrant  │
    │ report    │  │ embed  │ │       │ │ QA   │  │ PDF lib │
    │           │  │ KG     │ │       │ │ tasks│  │         │
    └───────────┘  └───────┘ └───────┘ └──────┘  └─────────┘
```

所有组件共享存储层与 LLM 路由中心，形成闭环：
> 监控发现新论文 → 入库 → 可问答 → 协作线可调用 → 分析结果回流知识库 → 用户反馈优化监控关键词

## 核心管线

### 🔭 监控管线（Monitoring）
- **多源获取**：arxiv API + 20+ 学术期刊，关键词加权评分 + LLM 双重筛选
- **中文摘要报告**：自动生成每日/每周 paper 报告（Jinja2 → Markdown + JSONL）
- **动态关键词优化**：从参考 PDF 自动提取关键词并分配层级权重，支持专家作者加分

### 📚 知识库管线（Knowledge Base）
- **Zotero 同步**：自动同步 Zotero 文献管理库
- **PDF 智能入库**：PDF 解析 → 分块 → Embedding → Qdrant 向量化存储
- **RAG 检索**：基于 Qdrant 的语义搜索，支持跨论文引用追溯 + 知识图谱提取
- **来源引用**：所有回答附带原文 chunk 引用，可验证

### 🤖 协作管线（Collaboration）
- **自然语言 → 分析代码**：LLM 生成 gammapy/sbi/ultranest 分析脚本（Python fenced block）
- **Docker Sandbox**：隔离执行环境，安全运行物理分析代码
- **子任务编排**：自动分解复杂分析任务为子任务并依次执行
- **结果回库**：生成的分析报告回流知识库形成闭环

### 🖥️ Web UI
- **FastAPI** + **Jinja2** 模板 + **HTMX 2.x** 局部更新 + **Bootstrap 5.3**
- 中文 i18n 国际化
- 5 个路由模块：仪表盘、日报、论文管理、QA 对话、分析任务

## CLI 命令

```bash
# Arxiv 日报管线
research-assistant arxiv fetch --categories astro-ph.HE --max-results 50 --save
research-assistant arxiv report --summarize --llm-filter

# 知识库
research-assistant knowledge ingest --all --embed
research-assistant knowledge qa "What is the current status of dark matter detection?"
research-assistant knowledge search "blazar SED" --top-k 5
research-assistant knowledge kg-extract --all

# Docker Sandbox 分析
research-assistant analyze generate "Fit a power law to Fermi-LAT data" --run
research-assistant analyze orchestrate "Compute the TS map for RX J1713" --run

# 启动 Web UI
research-assistant web --reload
```

## 技术栈

| 类别 | 技术 |
|------|------|
| **语言** | Python 3.12+ |
| **CLI** | Click + Rich |
| **后端** | FastAPI + Uvicorn |
| **前端** | Jinja2 + HTMX 2.x + Bootstrap 5.3 |
| **存储** | Qdrant（向量数据库）+ SQLite + 文件系统 |
| **LLM** | OpenAI / Anthropic / DeepSeek / Ollama（多 Provider 路由 + 自动回退） |
| **知识源** | arxiv API + PyZotero + PyPDF |
| **物理分析** | gammapy / sbi / ultranest（Docker Sandbox） |
| **配置** | Pydantic + YAML |
| **基础设施** | Docker Compose（Qdrant + Ollama） |

## 配置

| 文件 | 用途 |
|------|------|
| `llm_router.yaml` | Skill → provider + model + fallback 规则 |
| `arxiv_keywords.yaml` | 关键词评分规则（权重 + 别名） |
| `arxiv_categories.yaml` | 监控的 arxiv 分类 |
| `hardware_profile.yaml` | `init` 自动检测硬件，决定是否启用云端回退 |
| `docker-compose.yml` | Qdrant 向量库 + Ollama LLM 服务 |
| `sandbox/Dockerfile` | 物理分析沙箱（gammapy, sbi, ultranest） |

## 基础设施

```bash
# 启动后端服务
docker compose -f config/docker-compose.yml up -d qdrant ollama

# 构建分析沙箱
research-assistant analyze sandbox --build
```

## 快速开始

```bash
cd ~/storage/research-assistant
python3.12 -m venv .venv && source .venv/bin/activate
pip install -e ".[dev]"

cp config.example.env .env   # 编辑填入 API Key

research-assistant init
research-assistant web --reload

# 跑测试
pytest
```

## 相关设计文档

- [完整设计文档](docs/superpowers/specs/2026-07-03-research-assistant-design.md)
- [实现状态追踪](docs/superpowers/specs/2026-07-05-implementation-status.md)

## 状态

Phase 0（基础设施）已完成；Phase 1（三条产品线）开发中。Web UI 已具备 5 个路由模块。定时任务尚未接入——`arxiv report` 目前手动运行。
