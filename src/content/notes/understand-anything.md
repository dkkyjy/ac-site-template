---
title: "Understand-Anything - 使用示例"
category: "技能库 / 框架"
description: "Understand-Anything 使用示例。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["ua", "工具"]
---

# Understand-Anything 使用示例

## 📌 概述

**Understand Anything** 是一个 AI 编程插件（支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI 等平台），它能将代码库、知识库或文档转化为**交互式知识图谱**，包含文件、函数、类、依赖关系的可视化分析。

> 版本: **v2.6.0** | 项目: [github.com/Lum1104/Understand-Anything](https://github.com/Lum1104/Understand-Anything) | 在线演示: [understand-anything.com/demo](https://understand-anything.com/demo/)

---

## 🚀 快速上手

### 1️⃣ 安装插件

```bash
# Claude Code (原生)
/plugin marketplace add Lum1104/Understand-Anything
/plugin install understand-anything

# 或在其他平台：
# Codex → "Fetch and follow instructions from https://raw.githubusercontent.com/Lum1104/Understand-Anything/refs/heads/main/.codex/INSTALL.md"
# VS Code + Copilot → 克隆即自动发现
# Copilot CLI → copilot plugin install Lum1104/Understand-Anything:understand-anything-plugin
```

### 2️⃣ 分析代码库

```bash
# 在项目根目录运行
/understand
```

> 多 Agent 管线自动扫描项目，提取文件、函数、类、依赖，生成知识图谱，保存到 `.understand-anything/knowledge-graph.json`

### 3️⃣ 启动仪表盘

```bash
/understand-dashboard
```

> 浏览器打开交互式 Web 仪表盘——图谱按架构层着色、可搜索、可点击。选中节点查看代码、关系和中文解释。

### 4️⃣ 后续探索

```bash
# 提出问题
/understand-chat 支付流程是怎么工作的？

# 分析当前修改的影响范围
/understand-diff

# 深入理解某文件或函数
/understand-explain src/auth/login.ts

# 生成新人入职指南
/understand-onboard

# 提取业务领域知识（领域、流程、步骤）
/understand-domain

# 分析 Karpathy 模式 LLM Wiki 知识库
/understand-knowledge ~/path/to/wiki
```

---

## 🎯 核心命令一览

| 命令 | 功能 |
|------|------|
| `/understand` | 分析代码库生成知识图谱 |
| `/understand [--full]` | 强制完整重建（忽略缓存） |
| `/understand [--auto-update]` | 启用提交时自动更新图谱 |
| `/understand [--review]` | 使用 LLM 做全量图谱审查 |
| `/understand-dashboard` | 打开交互式可视化仪表盘 |
| `/understand-chat <问句>` | 基于知识图谱问答 |
| `/understand-diff` | 分析当前修改的波及范围 |
| `/understand-explain <文件/函数>` | 深度解释特定代码 |
| `/understand-onboard` | 生成团队新人入职指南 |
| `/understand-domain` | 提取业务域/流程/步骤 |
| `/understand-knowledge <目录>` | 分析 Karpathy 模式 LLM Wiki |

---

## 🧠 多 Agent 管线架构

分析管线由 **5 个专门 Agent** 协同工作（`/understand-domain` 额外加第6个）：

| Agent | 角色 |
|-------|------|
| `project-scanner` | 发现文件，检测语言和框架 |
| `file-analyzer` | 提取函数/类/导入，生成节点边 |
| `architecture-analyzer` | 识别架构层次 |
| `tour-builder` | 生成引导式学习路线 |
| `graph-reviewer` | 验证图完整性和引用一致性 |
| `domain-analyzer` | 提取业务域/流程/步骤（`/understand-domain` 用） |
| `article-analyzer` | 从 Wiki 文章提取实体/声明/隐式关系（`/understand-knowledge` 用） |

> 文件分析器并行运行（最多5并发，每批20-30文件），支持增量更新。

---

## 📦 分享图谱给团队

```bash
# 提交 .understand-anything/（排除中间文件）
git add .understand-anything/

# .gitignore 中添加
.understand-anything/intermediate/
.understand-anything/diff-overlay.json

# 大图(10MB+)用 git-lfs
git lfs install
git lfs track ".understand-anything/*.json"

# 启用自动更新
/understand --auto-update
```

> 图是纯 JSON——提交一次，队友跳过管线。对入职、PR 审查和文档即代码非常有用。

---

## 🌐 多平台兼容性

| 平台 | 安装方式 |
|------|----------|
| Claude Code | ✅ 原生插件市场 |
| Codex | ✅ AI驱动安装 |
| Cursor | ✅ 自动发现 |
| VS Code + Copilot | ✅ 自动发现 |
| Copilot CLI | ✅ 插件安装 |
| Gemini CLI | ✅ AI驱动安装 |
| OpenCode / OpenClaw / Antigravity / Pi Agent | ✅ 均支持 |

---

## 🖼️ 交互式仪表盘功能

- **🧭 引导式游览** — 按依赖顺序自动生成的架构走查
- **🔍 模糊与语义搜索** — "哪些部分处理认证？" → 返回相关节点
- **📊 差异影响分析** — 提交前查看修改波及哪些系统
- **🎭 角色自适应 UI** — 根据使用者（初级/PM/高级）调整细节
- **🏗️ 层次可视化** — 按 API/Service/Data/UI/Utility 自动分组着色
- **📚 语言概念标记** — 12种编程模式（泛型、闭包、装饰器等）出现处自动标注

---

**一句话总结：** 把任何代码库变成可交互的知识图谱，让你在阅读代码之前先看到全貌。
