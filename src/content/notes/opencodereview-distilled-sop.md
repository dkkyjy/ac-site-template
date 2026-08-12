---
title: "OpenCodeReview - 蒸馏分析"
category: "代码审查"
description: "阿里 AI 代码审查 CLI 的蒸馏分析：目标/架构/测试。"
pubDate: "2026-08-10"
badge: "distilled"
tags: ["opencodereview", "distilled", "analysis"]
---

# OpenCodeReview 蒸馏分析

## 1. 目标锁定 (Target)

**项目**: OpenCodeReview (OCR) — 阿里开源 AI 代码审查 CLI 工具
**仓库**: https://github.com/alibaba/open-code-review
**解决的问题**: 基于 Git Diff 的 AI 代码审查，支持 diff-review（增量）和 full-scan（全量）两种模式，输出行级评论。
**License**: Apache-2.0
**语言**: Go（CLI核心）+ JavaScript（npm安装器/IDE插件）+ TypeScript（VSCode扩展）

**面向用户**: 开发者（本地使用）+ CI/CD 流水线（GitHub Actions/GitLab CI）
**核心价值**: AI 驱动的自动化代码审查，多 LLM 提供商（Anthropic/OpenAI/阿里通义/DeepSeek/Kimi 等），跨 IDE（VS Code/Claude Code/Codex/Cursor）集成

---

## 2. 目标拆解 (Decomposition)

### 项目类型
CLI 工具（Go 编译为单二进制） + IDE 插件生态（VS Code/Claude/Cursor/Codex 插件）

### 源仓库结构
```
open-code-review/
├── cmd/opencodereview/          # Go CLI 入口（main，子命令：review/scan/config/llm）
├── internal/
│   ├── agent/                   # Review Agent（diff-review 编排）
│   ├── scan/                    # Scan Agent（full-scan 编排）
│   ├── llm/                     # LLM 客户端（Anthropic/OpenAI 协议）
│   ├── llmloop/                 # LLM Tool-Use 循环引擎（核心编排）
│   ├── diff/                    # Git Diff 解析（parser/hunk/resolver/git runner）
│   ├── tool/                    # 工具注册系统（file_read/find/search/comment）
│   ├── model/                   # 数据模型（diff/review/scan/preview）
│   ├── config/
│   │   ├── template/            # 任务模板（含 prompts/*.md）
│   │   ├── rules/               # 审查规则系统（含 rule_docs/ 各语言规则）
│   │   ├── toolsconfig/         # Tool 定义配置（JSON）
│   │   ├── allowlist/           # 文件类型/排除模式白名单
│   │   └── testconnection/      # LLM 连接测试
│   ├── gitcmd/                  # Git 命令封装
│   ├── viewer/                  # Web 审查结果查看器（Go 模板）
│   ├── session/                 # 会话持久化
│   ├── suggestdiff/             # Diff 建议生成
│   ├── pathutil/                # 路径工具
│   ├── telemetry/               # OpenTelemetry 遥测
│   └── release/                 # 发布命名规范检测
├── extensions/vscode/           # VS Code 扩展（TypeScript）
├── plugins/                     # IDE 插件定义（Claude/Cursor/Codex/Claude-Code）
├── bin/ocr.js                   # npm 二进制入口（下载平台特定二进制）
├── scripts/                     # 安装/更新脚本
├── examples/                    # GitHub Actions / GitLab CI 示例
├── Makefile                     # 构建系统
└── package.json                 # npm 包元数据
```

---

## 3. 测例提取 (Tests)

### 官方测例（Go 单元测试，50+ 个）

| 测试文件 | 测试内容 | 模式 |
|---------|---------|------|
| `internal/llm/client_test.go` | LLM 客户端构造/消息构建 | unit |
| `internal/llm/providers_test.go` | Provider 注册/查找/环境变量检测 | unit |
| `internal/llm/resolver_test.go` | Endpoint 解析/环境变量覆盖 | unit |
| `internal/llmloop/loop_test.go` | LLM 循环编排/压缩/任务过滤 | unit |
| `internal/diff/parser_test.go` | Unified Diff 解析 | unit |
| `internal/diff/hunk_test.go` | Hunk 合并/匹配 | unit |
| `internal/diff/git_test.go` | Git 命令执行/差异获取 | unit |
| `internal/diff/resolver_test.go` | Diff 行解析 | unit |
| `internal/diff/relocation_test.go` | 行号重定位 | unit |
| `internal/diff/workspace_file_test.go` | 工作区文件检测 | unit |
| `internal/agent/util_test.go` | Agent 工具/过滤逻辑 | unit |
| `internal/agent/preview_test.go` | Preview 生成 | unit |
| `internal/scan/agent_test.go` | Scan Agent 编排 | unit |
| `internal/scan/batch_test.go` | 文件批量处理 | unit |
| `internal/tool/..._test.go` | FileFind/FileRead 等工具 | unit |
| `internal/config/*_test.go` | 配置加载/规则解析/模板 | unit |
| `internal/session/persist_test.go` | 会话持久化 | unit |
| `internal/agent/*_test.go` | Review Agent 编排/预览/计划 | unit |
| `internal/scan/*_test.go` | Scan Agent 编排/批量/去重/预算 | unit |
| `internal/viewer/hostguard_test.go` | Viewer 主机防护 | unit |
| `internal/release/asset_naming_test.go` | 发布资产命名规范 | unit |
| `cmd/opencodereview/*_test.go` | CLI 命令解析/输出格式 | unit |

### Benchmark（官方公开数据）
基于 **50** 个流行开源仓库、**200** 个真实 PR、**10** 种编程语言、**1,505** 个标注真值（80+ 工程师交叉验证）：

| 指标 | OCR vs Claude Code | 说明 |
|------|:---:|------|
| **F1** | 显著更高 | 精确率+召回率的调和平均 |
| **Precision** | 显著更高 | 低误报（核心设计：Precision 优先） |
| **Recall** | 较低 | 有意权衡：宁可遗漏也不误报 |
| **Avg Token** | **~1/9** | API 成本仅 ~1/9 |
| **Avg Time** | 更快 | 审查速度提升 |

### CI 测试管道（GitHub Actions）
- 交叉编译（6 平台：linux/darwin/windows × amd64/arm64）
- 发布到 GitHub Release + npm（平台特定包 + 主包）
- npm `postinstall` 钩子下载平台二进制

### 功能测试路径（用户手册中的验证方式）
1. `ocr review` — diff-review 工作流
2. `ocr scan` — full-scan 工作流
3. `ocr config provider/model` — LLM 配置
4. `ocr llm test` — LLM 连接测试
5. `ocr preview` — Review 预览
6. `ocr viewer` — Web 结果查看

---

## 4. 组件分解 (Component Decomposition)

| 组件 | 路径 | 功能 | 行为选择 | 理由 |
|------|------|------|---------|------|
| LLM Client | `internal/llm/` | 统一接口(Anthropic/OpenAI)，含Provider注册/Endpoint解析/Token计数 | **调用** | 已有现成 SDK(llm Client)，GA系统可直接调用外部LLM API |
| LLM Tool-Loop | `internal/llmloop/` | Tool-Use 循环引擎（多轮工具调用/任务调度/压缩/Token聚合） | **重写** | 核心编排逻辑，GA系统有自身 Agent 循环，需改写为适配 GA 架构 |
| Diff Engine | `internal/diff/` | Git Diff 解析/Hunk管理/行号重定位 | **重写（精简）** | 核心价值，但实现较复杂（Go 特定），可重写为 Python 简化版 |
| Agent (Review) | `internal/agent/` | Diff-Review 编排（解析→过滤→规划→逐文件审查） | **重写** | 核心差异化组件，需适配GA系统的SOP执行框架 |
| Agent (Scan) | `internal/scan/` | Full-Scan 编排（文件枚举→过滤→LLM审查→去重） | **重写** | 同上 |
| Tool System | `internal/tool/` | FileRead/FileFind/CodeSearch/CodeComment/Registry | **部分调用+重写** | 文件读取可用GA系统工具，评论收集逻辑需重写 |
| Template/Prompts | `internal/config/template/prompts/` | 审查提示词模板（系统/用户/规划/压缩/重定位） | **调用（核心prompt）+重写** | prompt设计可直接借鉴，格式按GA SOP风格重写 |
| Rules System | `internal/config/rules/` | 路径匹配规则/语言规则/Project Rule | **调用** | 规则概念好，可内联到SOP中实现 |
| Session/Persist | `internal/session/` | 历史记录/结果存储 | **舍弃** | GA系统有自身记忆系统 |
| Viewer | `internal/viewer/` | Web 审查结果查看器 | **舍弃** | 非核心，可后续做 |
| VSCode Extension | `extensions/vscode/` | VS Code集成 | **舍弃** | IDE扩展不在GA系统能力范围内（Web Agent） |
| CLI Command | `cmd/opencodereview/` | CLI 入口/子命令解析 | **舍弃** | GA系统不需要CLI，需要SOP接口 |
| Telemetry | `internal/telemetry/` | OpenTelemetry监控 | **舍弃** | 可后续集成 |
| Git Runner | `internal/gitcmd/` | Git 命令执行封装 | **重写（精简）** | diff-review依赖git，按GA系统工具链适配 |

### 核心差异化——值得GA吸收的能力

1. **Diff-based 代码审查管道**：解析Git Diff → 规划 → 逐文件LLM审查 → 行级评论的完整pipe
2. **LLM Tool-Use 循环**：多轮工具调用框架，含task_done/code_comment等专用工具
3. **审查提示词设计**：main_task_system.md 中"严格聚焦当前Diff"等设计哲学
4. **多Provider支持**：统一的LLM协议抽象（Anthropic/OpenAI）
5. **Full-Scan 模式**：全量文件审查（另类审查场景）
6. **审查规则系统**：路径→规则的灵活匹配（含中括号展开/glob匹配）

---

## 5. 行为选择汇总 (Action Summary)

| 决策 | 组件 |
|------|------|
| **重写** | Review Agent 编排、Scan Agent 编排、LLM Tool-Loop（适配GA架构）、Diff 解析引擎（Python精简版） |
| **调用** | LLM Client 概念（GA直接用外部LLM API）、Provider 列表、提示词设计原则、审查规则系统概念 |
| **舍弃** | CLI 命令、VS Code 扩展、Web Viewer、Telemetry、Session Persist |
| **形态** | **混合型 Morphling** → 将代码审查能力作为 GA 系统的 L3 SOP |

---

## 6. 产物的形式 (Output Form)

**形态**: L3 SOP + 核心 Python 模块

**SOP**: `memory/opencodereview_sop.md` — 面向GA系统的代码审查SOP，参考目标项目的流程设计
**核心模块**: `temp/ocr_agent/` — Python 实现的简化版 Diff 解析 + 审查编排，可供 GA 直接调用

**核心设计原则（从目标项目吸收的）**:
1. 严格聚焦当前 Diff，不在审查中评论其他文件
2. 两阶段：先规划后审查（PLAN_TASK → MAIN_TASK）
3. 行级精准评论（code_comment 匹配算法）
4. 多 LLM Provider 支持（环境变量切换）
5. 规则层叠：system rules → project rules → user rules
6. Memory Compression：长会话时的上下文压缩

---

## 7. 对照验证 (Verification Plan)

| 维度 | 目标项目 | 蒸馏产物 |
|------|---------|---------|
| diff-review | 通过 `ocr review` 命令 | SOP步骤+Python组合 |
| full-scan | 通过 `ocr scan` 命令 | SOP（简化版，聚焦纯文本文件） |
| 多Provider | 内置20+ Provider | 环境变量配置（ANTHROPIC/OPENAI/DASHSCOPE） |
| 行级评论 | 含定位算法 | 简化版定位（基于正则/行号匹配） |
| 审查规则 | glob+路径规则 | SOP 内联规则语法 |
| 自定义提示词 | 模板系统 | 直接使用目标项目的prompt设计 |

---

*蒸馏日期: 2026-06-26*
*来源: https://github.com/alibaba/open-code-review (commit HEAD)*