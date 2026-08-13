---
title: "ArXiv Daily Researcher"
description: "基于 LLM 的智能学术论文监控、筛选与深度分析系统 — 自动从 ArXiv 与 20+ 顶级学术期刊获取论文，关键词加权评分精准筛选，PDF 深度分析，追踪关键词演变趋势，生成专业报告并推送多渠道通知。"
pubDate: "Jul 22 2026"
updatedDate: "Jul 22 2026"
heroImage: "/itemPreview.png"
badge: "active"
github: "https://github.com/yzr278892/arxiv-daily-researcher"
tags: ["python", "arxiv", "llm", "academic", "automation", "docker", "streamlit", "pdf-analysis"]
---

**ArXiv Daily Researcher** 是一个基于 LLM 的智能学术论文监控、筛选与深度分析系统。自动从 ArXiv 与 20+ 顶级学术期刊获取最新论文，通过可配置的关键词权重评分系统精准筛选相关论文，下载 PDF 进行深度分析，追踪关键词演变趋势，生成专业报告，并推送多渠道通知——全程自动化，零人工干预。

## 核心功能

### 📡 多数据源支持
从 ArXiv 预印本和 **20+ 顶级期刊**（PRL、Nature、Science 等）同步抓取。期刊论文自动检测 ArXiv 版本并切换来源，规避版权限制，获取完整摘要与可下载 PDF。可选接入 **Semantic Scholar** 获取引用数和 AI 单句摘要。

### 🎯 双 LLM 加权评分
CHEAP_LLM 对每篇论文的每个关键词独立评分（0–10），乘以权重求和后与动态及格线比较。支持从参考 PDF **自动提取关键词**并分配层级权重，支持专家作者加分。

### 🔍 深度 PDF 分析
通过筛选的论文自动下载 PDF，SMART_LLM 提取**研究方法、创新点、技术栈、关键结论、局限性、研究关联、未来方向**七个维度。支持 MinerU 云端与 PyMuPDF 本地双模式解析，可自动降级。摘要自动中文翻译（MD5 缓存去重）。

### 📈 关键词趋势追踪
每篇论文关键词写入 SQLite，通过 AI 批量语义归并，生成 Mermaid 柱状图和折线趋势图。HTML 报告含 CSS 彩色水平柱状图 + 趋势热图，每个关键词贯穿所有视图共享一种颜色。

### 🔬 研究趋势分析
全新独立运行模式：指定关键词与任意时间范围，批量检索 ArXiv 论文，LLM 逐篇生成 TLDR，SMART_LLM 单次调用综合分析五个维度（热点话题、时间演变、核心研究者、研究空白、方法论趋势），大幅降低 Token 消耗。

### 📊 Token 消耗追踪
线程安全全局计数器，实时统计每次运行所有 LLM 调用的 Token 消耗，按模型分类展示在报告末尾和所有通知渠道中。

### 📄 Markdown + HTML 双格式报告
按数据源/关键词独立输出报告。三种报告类型：每日研究、研究趋势、关键词趋势。Markdown 含可折叠详情块，HTML 卡片式响应式布局。样式通过外置 CSS 变量完全可自定义。

### 🔔 多渠道通知
支持 **Telegram Bot**（富文本 MarkdownV2/HTML）、飞书（文本卡片/富文本卡片）、邮件（SMTP，支持 BCC 批量）、**Discord Webhook**、ServerChan，以及本地缓存投递。通知模板完全可自定义。

### 📊 Streamlit 配置面板
内置 Streamlit 配置面板，提供 GUI 编辑所有配置项，实时预览 Markdown/HTML/Telegram 三种渲染效果，一键启动每日/趋势/关键词趋势运行，无需手动编辑配置文件。

## 项目架构

```
arxiv-daily-researcher/
├── main.py                  # 主入口路由
├── configs/                 # 配置文件目录
├── src/
│   ├── fetchers/            # 数据获取层
│   ├── filters/             # LLM 评分与筛选
│   ├── analyzers/           # PDF 深度分析
│   ├── reporters/           # 报告生成
│   ├── notifiers/           # 多渠道通知
│   └── trend/               # 趋势分析
├── data/                    # 数据存储
├── logs/                    # 运行日志
├── docker/                  # Docker 部署
├── scripts/                 # 工具脚本
└── docs/                    # 文档
```

## 技术栈

| 层级 | 技术 |
|------|------|
| **核心** | Python 3.10+ |
| **数据获取** | ArXiv API, Requests, BeautifulSoup4 |
| **LLM 集成** | OpenAI API, LiteLLM (多模型路由) |
| **PDF 解析** | MinerU (云端), PyMuPDF (本地) |
| **数据存储** | SQLite |
| **报告生成** | Jinja2, Mermaid |
| **通知推送** | Telegram Bot API, 飞书 Webhook, SMTP, Discord Webhook, ServerChan |
| **配置界面** | Streamlit |
| **部署** | Docker, GitHub Actions |
| **向量/搜索** | Semantic Scholar API |

## 快速开始

```bash
# Clone 仓库
git clone https://github.com/dkkyjy/arxiv-daily-researcher.git
cd arxiv-daily-researcher

# 安装依赖
pip install -r requirements.txt

# 配置环境变量
cp .env.example .env
# 编辑 .env 填入 LLM API Key 和通知渠道配置

# 运行每日论文监控
python main.py --mode daily

# 研究趋势分析
python main.py --mode trend --keywords "quantum computing,LLM" --days 365

# 启动 Streamlit 配置面板
streamlit run configs/streamlit_app.py
```
