---
title: "ArXiv Daily Researcher"
description: "用 LLM 追踪 arXiv 与期刊论文的自部署研究助手 — 多来源检索与跨源合并，四种研究兴趣评分策略，中文摘要、PDF 全文分析与专题趋势报告，浏览器内完成配置、运行和阅读，结果可推送到邮件与即时通信。"
pubDate: "Jul 22 2026"
updatedDate: "Oct 21 2026"
heroImage: "/itemPreview.png"
badge: "active"
github: "https://github.com/yzr278892/arxiv-daily-researcher"
tags: ["python", "arxiv", "llm", "academic", "automation", "docker", "webui", "pdf-analysis"]
---

**ArXiv Daily Researcher** 是一个自部署的论文追踪与研究助手。它定期检索 arXiv 与可选期刊来源，按关键词和个人偏好筛选论文，生成中文摘要、PDF 分析与 Markdown / HTML 报告。配置、运行和阅读都在浏览器里完成，结果也能发送到邮件或即时通信平台。

## 核心功能

### 📡 多来源检索与跨源合并
追踪 arXiv 新论文和修订版，支持 PRL、PRA/PRB、Nature、Science、Hugging Face Papers 和自定义 OpenAlex 期刊。同一篇论文的不同来源记录会合并展示，来源信息、可去重字段和无法安全合并的原始分析各自保留。

### 🎯 四种研究兴趣评分
提供**核心相关性 V2**（默认）、加权关键词 V1、加权关键词＋扣分、偏好学习 V1 四种策略。可分别设置关注主题、不关注主题和作者偏好，查看每篇论文的逐项评分、扣分与通过原因。

### 🔍 中文摘要与全文分析
评分阶段生成摘要译文、简短 TL;DR 和关键词；通过筛选的论文继续分析方法、创新点、结论、局限与未来方向。支持 PyMuPDF 本地解析与 MinerU 服务，Semantic Scholar TL;DR 翻译可选，合格论文的 PDF 可归档到 reference 目录并按分数上限保留。

### 📈 专题研究与关键词趋势
按关键词、日期和 arXiv 分类开展专题研究，汇总热点话题、时间演变、核心研究者、研究空白与方法演变。关键词趋势报告展示已收集论文中的主题变化，可在 HTML 报告中查看彩色柱状图与热图。

### 🗃️ 报告、收藏与历史维护
按批次浏览 Markdown / HTML 报告，检索论文并记录喜欢或不喜欢的内容。支持按日期补跑过去日报、从旧 HTML 报告导入历史、补全历史数据与扫描遗漏论文；未完成的论文留在队列中，可在后续运行继续处理。

### 🔔 通知与运行管理
支持邮件、企业微信、钉钉、Telegram、Slack 和通用 Webhook，通知模板可自定义。侧边栏显示当前版本，检查到新 Release 时给出发布页链接。

### 📊 用量、备份与同步
按模型和时间统计普通输入、缓存输入与输出 Token；本地备份保存 SQLite 一致性快照并设置保留策略，WebDAV 可同步配置、报告和数据库。

## 评分策略

| 策略 | 判定方式 |
| :--- | :--- |
| 核心相关性 V2 | 主关键词的相关度决定是否通过；参考关键词和作者偏好辅助排序。 |
| 加权关键词 V1 | 关键词相关度乘权重，再加作者分，与动态及格线比较。 |
| 加权关键词＋扣分 | 沿用加权关键词的加分与及格线，不关注关键词按相关度和独立权重扣分。 |
| 偏好学习 V1 | 在加权评分上结合喜欢、不喜欢和历史通过论文的学习信号调整得分。 |

## 项目架构

后台任务与 WebUI 是两个独立容器：带调度与通知的 Worker 负责检索、评分、分析和投递，面向用户的 ASGI 面板只负责配置与查看，任务状态、论文队列、报告和用量都落在 SQLite 中。

```
arxiv-daily-researcher/
├── docker-compose.yml         已发布镜像部署
├── tests/docker-compose.yml   源码开发部署
├── main.py                    命令行入口
├── src/                       检索、评分、分析、报告与 WebUI
├── configs/                   配置示例和报告、通知模板
├── runtime/config.json        当前运行配置
├── data/                      论文数据库、报告、PDF 与备份
├── logs/                      运行日志
└── .env                       模型凭据、通知和账户配置
```

## 技术栈

| 层级 | 技术 |
|------|------|
| **核心** | Python 3.12+ |
| **数据获取** | arXiv API, OpenAlex, 期刊站点与 RSS, Requests, BeautifulSoup4 |
| **LLM 集成** | OpenAI 兼容接口, LiteLLM (多模型路由，低成本/高性能双模型) |
| **PDF 解析** | PyMuPDF (本地), MinerU (服务) |
| **数据存储** | SQLite |
| **报告生成** | Jinja2, Mermaid, 可自定义 CSS |
| **通知推送** | SMTP, 企业微信, 钉钉, Telegram, Slack, 通用 Webhook |
| **配置界面** | ASGI WebUI (中英文、浅色/深色、四种主题) |
| **部署** | Docker Compose, GHCR 多架构镜像, GitHub Actions |

## 快速开始

```bash
# 获取项目
git clone https://github.com/yzr278892/arxiv-daily-researcher.git
cd arxiv-daily-researcher
cp .env.example .env

# 把 PUID / PGID 设为当前用户的 id -u / id -g
mkdir -p data logs runtime

# 启动 Worker 与 WebUI
docker compose pull
docker compose up -d
docker compose ps
```

打开 `http://HOST:8501` 创建管理员账户，在“配置 → API”填写低成本与高性能两个模型的 API Key、Base URL 和模型名，在“关键词”填写研究背景与主关键词、选择来源和评分策略，点击“保存所有更改”，然后在“运行 → 每日研究”启动任务。首次试跑可把单次处理上限设为 5 篇。

面板仅建议在可信局域网或 Tailscale 中直接使用 HTTP；公网访问请配置 HTTPS 反向代理，并可通过 `ADR_WEBUI_BIND_ADDRESS` 与 `WEBUI_COOKIE_SECURE` 限制监听地址和启用 Secure Cookie。

~~~bash
# 使用源码镜像做开发验证（不影响已发布部署）
docker compose -f tests/docker-compose.yml up -d --build
~~~

仓库另提供命令行配置向导 `src/utils/setup_wizard.py`、`main.py` 命令行入口，以及每日研究与趋势研究的 GitHub Actions 工作流。升级前备份 `.env`、`runtime/`、`data/` 和自定义模板。
