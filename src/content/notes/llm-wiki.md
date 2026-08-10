---
title: "LLM Wiki - 用法示例"
description: "LLM Wiki 用法示例展示。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["llm", "wiki"]
---

# 📖 LLM Wiki 用法示例展示

> 这是 [wei-llm-wiki](https://github.com/zyw-Wayne/wei-llm-wiki) 的演示，基于 Karpathy 的 LLM Wiki 方法论。

## 底层理念

**与 RAG 的区别**：RAG 每次从原始文本重新推导 → LLM Wiki **预编译**成结构化 wiki 页面。每次新文章让整个知识库更丰富，好的答案存回为知识节点。**复合知识，不是重复推理。**

---

## 第一步：初始化知识库

```bash
wiki init ~/my-wiki
```

| `wiki init <path>` | 效果 |
|---|---|
| 创建 `raw/`、`wiki/`、`evolve/` 目录结构 | ✅ |
| 部署 `WIKI.md`（schema，从模板复制） | ✅ |
| 部署 `knowledge-graph.html`（交互式知识图谱） | ✅ |
| 写入 `config.json`（持久化路径，后续免输路径） | ✅ |

**目录结构**：
```
~/my-wiki/
├── raw/                      # 原始文章（只读）
│   └── <文章标题>/
│       ├── article.md
│       └── images/           # 在线文章的本地图片
├── wiki/                     # LLM 编译的知识页面
│   ├── index.md              # 内容目录（查询入口）
│   ├── log.md                # 操作日志（追加式）
│   └── *.md                  # 各概念/主题/查询存档页面
├── evolve/                   # 知识进化追踪
│   ├── index.md              # 总控面板
│   └── <分类名>.md
├── knowledge-graph.html      # 交互式知识图谱
└── WIKI.md                   # 知识库 schema
```

---

## 第二步：摄入文章（4种来源自动识别）

```bash
wiki ingest https://mp.weixin.qq.com/s/xxxxx      # 微信公众号 → wechat-article-down
wiki ingest https://blog.example.com/article       # 普通网页 → chrome-devtools 抓取
wiki ingest https://github.com/owner/repo          # GitHub文档仓库 → GitHub MCP 批量读取
wiki ingest ~/notes/research.md                    # 本地文件 → 直接读取
```

**自动判断流程**：
```
wiki ingest <来源>
     │
     ├─ https://mp.weixin.qq.com/...          → wechat-article-down
     ├─ https://github.com/{owner}/{repo}...  → GitHub MCP 扫描+批量读取
     ├─ https://...（其他）                     → chrome-devtools 抓取正文
     └─ 本地路径                                → Read 工具读取
```

**混合来源单次摄入**：
```bash
wiki ingest https://mp.weixin.qq.com/s/aaa ~/notes/b.md https://blog.example.com/c
```

---

## 第三步：Ingest 内部发生了什么

以一篇网页文章为例：

1. **获取文章**（chrome-devtools 抓取正文 + 提取脚本 → 自动转 Markdown）
2. **下载图片** → `raw/<标题>/images/`，替换文章中远程 URL 为本地路径
3. **编译知识** → 读取 `raw/*/article.md`，提取核心概念/观点/实体
4. **更新/新建 wiki 页面**（每个概念单独一页）

**wiki 页面格式**（来自 `references/page-format.md`）：

```markdown
---
title: 页面标题
tags: [tag1, tag2]
sources: [raw/文章标题/article.md]
updated: YYYY-MM-DD
---

# 标题

## 定义
一段话清晰定义这个概念/主题是什么。

## 核心观点
- 观点一（来源：[[来源文章名]]）
- 观点二

## 矛盾与争议
> ⚠️ 矛盾：来源A说...，来源B说...，尚未统一。

## 相关概念
- [[相关页面1]] — 一句话说明关系

## 来源
- [文章标题](../raw/文章标题/article.md)
```

5. **更新 index.md**（按主题分组）
6. **追加 log.md**

---

## 第四步：查询知识库

```bash
wiki query "Claude Code 和 Codex 的适用场景对比"
```

**Query 工作流**：
1. 读取 `wiki/index.md` → 定位相关页面
2. 读取相关页面 → 综合给出答案
3. 若回答有洞察价值 → 存为 `query-<主题>.md`（持久化新知识）
4. 追加 log.md

---

## 第五步：知识进化（Evolve）

```bash
wiki evolve agent              # 审计「Agent 开发」分类的覆盖度
wiki evolve                    # 更新所有已评估分类
wiki evolve --all              # 全量评估所有分类
```

**五维评估体系**：广度 · 深度 · 实用性 · 时效性 · 交叉引用

**进化向量**追踪每个知识方向的成熟度：
- 🔴 空白 → 🟡 有基础覆盖 → 🟢 成熟

**被动进化**：查询过程中自动发现知识盲区或事实矛盾，更新进化档案。

---

## 第六步：健康检查与可视化

```bash
wiki lint      # 检查：矛盾、过时内容、孤儿页面、缺失交叉引用、超大页面
wiki graph     # 部署交互式知识图谱HTML
wiki log       # 聚合操作日志
wiki list      # 一屏知识概览
wiki refresh   # 根据实际状态重新生成 WIKI.md 元信息
```

**知识图谱**（交互式 D3.js 可视化，76节点·291边·15分类）：
```
node index.js          # 或直接浏览器打开 knowledge-graph.html
```

---

## 第七步：Collection 模式（多知识库）

一个父目录管理多个独立主题知识库：

```
~/wikis/
├── COLLECTION.md           # 集合注册表
├── ai-coding/              # 独立知识库 1
│   ├── WIKI.md
│   ├── raw/
│   └── wiki/
├── prompt-engineering/     # 独立知识库 2
│   ├── WIKI.md
│   ├── raw/
│   └── wiki/
└── agent-development/      # 独立知识库 3
    ├── WIKI.md
    └── ...
```

操作时自动列出供选择：
```
❯ wiki ingest
? 选择知识库:
  1. ai-coding
  2. prompt-engineering
  3. 创建新知识库
```

---

## 实际案例

- **真实知识库**: [llm-wiki-kb](https://github.com/zyw-Wayne/llm-wiki-kb) — 覆盖 AI 编程工具、Agent 开发、Prompt 工程等
- **安装**: `npx skills add https://github.com/zyw-Wayne/wei-llm-wiki`

---

## 对比 RAG vs LLM Wiki

| 维度 | RAG | LLM Wiki |
|---|---|---|
| 每次查询 | 向量检索 → 注入上下文 → 重新推理 | 直接读已编译的结构化页面 |
| 知识增长 | 加文档（向量化） | 每次ingest提炼概念，每次query存档洞察 |
| 矛盾处理 | 混在上下文中，无感知 | 显式标注 ⚠️ 矛盾，追踪 |
| 可读性 | 原始文档片段 | 结构化概念页，交叉引用 |
| 知识图谱 | 无 | D3.js 交互式图谱 |
| 覆盖度审计 | 无 | 五维评估+进化向量 🟢🟡🔴 |

---

这就是 **llm-wiki** 的完整用法——它不是一个"搜索工具"，而是一个 **知识编译器**：你投喂文章，它自动提炼概念、建立链接、追踪矛盾、演化知识成熟度，并将其可视化为交互式知识图谱。
