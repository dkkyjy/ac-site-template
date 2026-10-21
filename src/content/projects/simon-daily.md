---
title: "Simon Willison Daily Reader"
description: "多源博客抓取与翻译工具，支持 Simon Willison、Addy Osmani、Claude Blog、Anthropic Research/Engineering、OpenAI Research、Google AI/Technology、Google DeepMind、Simon Willison Guides 共 10 个来源，自动抓取文章、翻译成中文并生成 AI 摘要。提供 FastAPI 后端与 React 前端。"
pubDate: "Jul 02 2026"
updatedDate: "Oct 20 2026"
heroImage: "/itemPreview.png"
badge: "active"
github: "https://github.com/dkkyjy/simon-daily"
tags: ["python", "blog-aggregator", "translation", "fabric", "fastapi", "react", "typescript"]
---

**Simon Willison Daily Reader** 是一个多源博客抓取与翻译工具，支持从 **10 个技术博客/源** 自动抓取文章、翻译为中文并生成 AI 摘要。采用 `simon_daily` Python 包架构，提供 **CLI 子命令**、**FastAPI 后端** 和 **React/TypeScript 前端**。

## 主要功能

- **10 源支持**：Simon Willison、Addy Osmani、Claude Blog、Anthropic Research/Engineering、OpenAI Research、Google AI/Technology、Google DeepMind、Simon Willison Guides
- **自动翻译**：通过 fabric-ai 的 translate pattern 将英文翻译成中文（支持日语等多语言）
- **AI 摘要**：使用 fabric-ai 的 summary pattern 生成精简摘要
- **CLI 子命令**：`fetch`、`summarize`、`translate-remaining`、`fetch-all-anthropic`
- **Web UI**：FastAPI 后端 + React/TypeScript 前端（Vite），支持中英文双语对照、来源筛选、全文搜索
- **跨源去重**：Google 多个栏目 feed 存在重叠，列表页按原文链接去重，同一篇文章只出现一次
- **每日自动任务**：`daily_task.py` 一键抓取所有来源并部署中文版到个人 Astro 网站
- **Python API**：可直接在代码中 `import` 使用 `fetch()`、`list_posts()`、`translate_post()` 等
- **增量抓取**：基于文件名去重，已抓取自动跳过，支持按天数过滤
- **双模式抓取**：RSS 订阅（feed）和列表抓取（listing）自动切换
- **正文提取健壮性**：自动挑选页面上文本量最大的容器（避免抓到「相关文章」小卡片），带超时重试

## 支持的数据源

| Key | 名称 | 方式 |
|-----|------|------|
| `simon` | Simon Willison | Atom Feed |
| `addy` | Addy Osmani | RSS + 全文抓取 |
| `claude` | Claude Blog | 列表抓取（分页） |
| `anthropic-research` | Anthropic Research | 列表抓取（单页） |
| `anthropic-engineering` | Anthropic Engineering | 列表抓取（单页） |
| `openai-research` | OpenAI Research | 列表抓取（Next.js RSC） |
| `google` | Google AI | RSS + 全文抓取 |
| `google-technology` | Google Technology | RSS + 全文抓取 |
| `deepmind` | Google DeepMind | RSS + 全文抓取 |
| `simon_guides` | Simon Willison Guides | 列表抓取（单页） |

## 项目结构

```
simon_daily/                          # 核心 Python 包
├── sources.py                        # SOURCES 注册表
├── content.py                        # HTML→Markdown 转换
├── formatters.py                     # Atom/RSS 格式化
├── io.py                             # 文件读写、文章列表、跨源去重
├── translate.py                      # fabric-ai 翻译/摘要封装
├── fetcher.py                        # 抓取编排（feed + listing 分发）
├── cli.py                            # 命令行子命令
├── deploy.py                         # 部署流水线
├── sync_simon_to_site.py             # 同步 simon / engineering 文章到个人站
└── scrapers/                         # listing 抓取器
    ├── claude.py
    ├── anthropic_research.py
    ├── anthropic_engineering.py
    └── openai_research.py

ui/                                   # React/TypeScript 前端（Vite）
fetch.py                              # 入口：委托 cli.main()
daily_task.py                         # 入口：委托 deploy.daily_main()
server.py                             # FastAPI 应用
```

## 技术栈

| 层级 | 技术 |
|------|------|
| **后端** | Python + FastAPI + Uvicorn |
| **前端** | React + TypeScript + Vite |
| **UI 组件** | animal-island-ui（动森风格 React 组件库） |
| **RSS 解析** | feedparser |
| **HTML 解析** | BeautifulSoup4 + markdownify |
| **AI 能力** | fabric-ai（翻译、摘要，默认走本地 Ollama 模型） |
| **自动化** | GitHub Actions（UTC 0:00/12:00 定时抓取） |

## 快速开始

```bash
# 安装依赖
pip install feedparser requests beautifulsoup4 markdownify fastapi uvicorn

# 抓取并翻译（默认 Simon Willison 最近 1 天）
python fetch.py

# 指定来源和天数
python fetch.py --source addy --days 3
python fetch.py --source anthropic-research --no-translate
python fetch.py --source google
python fetch.py --source deepmind

# CLI 子命令
python fetch.py summarize --source claude           # 生成 AI 摘要
python fetch.py translate-remaining --source claude # 翻译该来源未翻译的文章
python fetch.py fetch-all-anthropic                 # 批量抓取 Anthropic

# 每日任务（抓取所有源 + 部署到个人网站）
python daily_task.py --days 3 --dry-run

# 启动 Web UI
python server.py                       # FastAPI → http://127.0.0.1:8080
cd ui && npm run dev                   # React 开发服务器 :5173
cd ui && npm run build                 # 生产构建 → ui/dist/
```

## API 端点

文章级接口都支持可选的 `source` 参数——不同来源可能存在同名 slug（如同一篇 Anthropic 文章同时出现在 Research 和 Engineering）。

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/api/sources` | 可用来源列表 |
| GET | `/api/posts[?source=&search=]` | 文章列表（全局按日期倒序，多源时按链接去重） |
| GET | `/api/posts/{slug}?lang=&source=` | 文章内容（原文/中文） |
| GET | `/api/posts/{slug}/bilingual?source=` | 原文与译文并排 |
| POST | `/api/fetch/{source_key}?days=N` | 触发抓取（后台线程） |
| POST | `/api/translate/{slug}?source=` | 翻译一篇文章 |
| GET/POST | `/api/posts/{slug}/summary?source=` | 查看/生成摘要 |

## 输出格式

所有文章以 Markdown 格式保存到 `posts/<source-dir>/`：

- 原文：`YYYY-MM-DD-slugified-title.md`
- 中文译文：`YYYY-MM-DD-slugified-title.zh-cn.md`
- AI 摘要：`YYYY-MM-DD-slugified-title.summary.md`

## GitHub Actions

`.github/workflows/fetch.yml` 每天 UTC 0:00/12:00 自动抓取 Simon Willison 的最新文章（仅原文，不翻译）。
