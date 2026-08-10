---
title: "Claw Code 使用示例汇总"
description: "Claw Code 使用示例汇总。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["clawcode", "claude"]
---

# Claw Code 使用示例汇总

## 📋 项目概览

`ultraworkers/claw-code` 是一个**Rust 实现的 `claw` CLI 智能体工具集**（Agent Harness），该项目描述为"agent-managed museum exhibit"——由 AI 智能体自主维护的博物馆展品。其核心二进制是 `claw` CLI。

---

## 🚀 快速上手

### 1. 构建项目
```bash
cd rust
cargo build --workspace          # 首次编译
```

构建产物：
- Debug: `rust/target/debug/claw`（或 `claw.exe`）
- Release: `rust/target/release/claw`

### 2. 首次健康检查
```bash
cd rust
./target/debug/claw              # 启动交互式 REPL
/doctor                          # 检查 API 密钥、模型访问、工具配置
```

或非交互式：
```bash
./target/debug/claw doctor --output-format json
```

---

## 💻 核心使用模式

### 交互式 REPL
```bash
cd rust
./target/debug/claw
```
进入 REPL 后可输入自然语言指令或 `/` 开头的命令。

### 一次性提示（One-shot）
```bash
./target/debug/claw prompt "summarize this repository"

# 通过 stdin 管道输入
printf 'summarize this repository\n' | ./target/debug/claw prompt
```

### 简写模式
```bash
./target/debug/claw "explain rust/crates/runtime/src/lib.rs"
```

### JSON 输出（脚本友好）
```bash
./target/debug/claw --output-format json prompt "status"
```

---

## 🛠 初始化仓库
```bash
cd /path/to/your/repo
./target/debug/claw init
```
会在仓库创建 `.claw/settings.json`、`.claw.json`、`CLAUDE.md` 等配置文件，可重复运行（幂等）。

---

## ⚡ 高级斜杠命令（REPL 内）

### `/ultraplan` — 深度规划
```bash
/ultraplan refactor the auth module to use async/await
/ultraplan design a caching layer for database queries
```

### `/teleport` — 快速跳转到文件/符号
```bash
/teleport UserService          # 定位符号
/teleport src/auth.rs          # 定位文件
```

### `/bughunter` — 扫描 Bug
```bash
/bughunter                     # 扫描整个工作区
/bughunter src/handlers        # 扫描特定目录
```

---

## 🔐 模型与权限控制
```bash
# 指定模型
./target/debug/claw --model sonnet prompt "review this diff"

# 权限模式
./target/debug/claw --permission-mode read-only prompt "summarize Cargo.toml"
./target/debug/claw --permission-mode workspace-write prompt "update README.md"
./target/debug/claw --permission-mode danger-full-access prompt "..."

# 限制工具集
./target/debug/claw --allowedTools read,glob "inspect the runtime crate"
```

### 支持的模型别名
| 别名 | 实际模型 |
|------|---------|
| `opus` | `claude-opus-4-7` |
| `sonnet` | `claude-sonnet-4-6` |
| `haiku` | `claude-haiku-4-5-20251213` |

### 权限模式
| 模式 | 说明 |
|------|------|
| `read-only` | 只读，仅允许文件读/搜索/状态报告 |
| `workspace-write` | 默认，允许工作区内写操作，禁网络/Shell |
| `danger-full-access` | 完全权限，需显式指定 |

---

## 📁 配置层级

配置加载优先级（后加载覆盖前）：
1. `~/.claw.json`
2. `~/.config/claw/settings.json`
3. `<repo>/.claw.json`
4. `<repo>/.claw/settings.json`
5. `<repo>/.claw/settings.local.json`

```bash
# 查看当前配置
claw config
claw config --output-format json
```

---

## 📊 状态与工作区
```bash
# 查看工作区状态
claw status
claw status --output-format json

# 查看 Worker 状态
claw state
```

### 规则文件加载
Claw 会自动加载 `.claw/rules/` 下的 `.md`/`.txt`/`.mdc` 文件为项目指令，以及来自 Cursor、GitHub Copilot、Windsurf 等工具的规则文件。

---

## 📦 完整文档地图
- [`USAGE.md`](https://github.com/ultraworkers/claw-code/blob/main/USAGE.md) — 详细用法
- [`docs/navigation-file-context.md`](https://github.com/ultraworkers/claw-code/blob/main/docs/navigation-file-context.md) — 导航与文件上下文
- [`docs/local-openai-compatible-providers.md`](https://github.com/ultraworkers/claw-code/blob/main/docs/local-openai-compatible-providers.md) — 本地模型（Ollama/llama.cpp）
- [`PARITY.md`](https://github.com/ultraworkers/claw-code/blob/main/PARITY.md) — Rust 移植状态
- [`ROADMAP.md`](https://github.com/ultraworkers/claw-code/blob/main/ROADMAP.md) — 开发路线图

---

**总结：** Claw Code 是一个 Rust 实现的 AI 编码智能体工具集，提供 REPL/一次性命令两种交互模式，支持模型切换、权限分级、深度规划/代码导航/漏洞扫描等高级功能，并通过 JSON 输出对脚本/自动化场景友好。
