---
title: "OMX - 学习笔记"
category: "OMX / Codex 工作流"
description: "OMX 用法学习笔记。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["omx", "paper", "文献"]
---

# OMX 用法学习笔记

> 生成时间: 2026-06-25
> 环境: oh-my-codex v0.18.14, Codex CLI 0.36.0, 37个skills(30 active)
> 已有: arxiv-daily-researcher v3.0 (34py 文件, 双LLM论文监控系统)

---

## 一、OMX 核心技能速查

### 1.1 omx team — 多Worker并行执行

```bash
# 启动团队（N个worker）
omx team 3:executor "修复所有失败的测试"
# └─ 自动创建tmux窗格，每个worker独立Codex CLI会话
# └─ 默认自动创建worktree，共享任务状态

# 管理
omx team status my-team           # 查看状态
omx team status my-team --json    # JSON格式
omx team await my-team            # 等待完成
omx team shutdown my-team         # 关闭

# Worker间通信
omx team api send-message --input '{
  "team_name":"my-team",
  "from_worker":"worker-1",
  "to_worker":"worker-2",
  "body":"ACK"
}' --json
```

**关键点**:
- 适合**长周期、持久化、多worker**任务
- 自动使用worktree隔离
- 小型并行用 Codex native subagents（本会话内扇出）

### 1.2 omx ultragoal — 多目标任务管理

```bash
# 创建目标
omx ultragoal create-goals --brief "修复登录页面的3个bug"

# 查看状态
omx ultragoal status

# 标记完成 / 推进下一个
omx ultragoal complete-goals

# 导出产物目录结构
# .omx/ultragoal/brief.md          # 原始需求
# .omx/ultragoal/goals.json        # 目标分解
# .omx/ultragoal/ledger.jsonl      # 审计日志
```

**关键点**:
- 适合将大需求拆解为多个 Codex /goal 顺序执行
- 不支持自动 `/goal clear`（需手动执行）

### 1.3 omx autoresearch-goal — 教授评判制研究

```bash
# 创建研究任务
omx autoresearch-goal create \
  --topic "量子纠错最新进展" \
  --rubric "需覆盖: 表面码、LDPC码、硬件实现" \
  --slug quantum-2025

# 启动Codex goal
omx autoresearch-goal handoff --slug quantum-2025

# 记录评判结果
omx autoresearch-goal verdict \
  --slug quantum-2025 \
  --verdict pass \
  --evidence "找到5篇表面码论文, 覆盖要求"

# 完成
omx autoresearch-goal complete --slug quantum-2025 --codex-goal-json ./goal.json
```

**关键点**:
- 适合**需验证通过才能结束**的研究任务
- 内置 professor-critic 验收机制
- 产物在 `.omx/goals/autoresearch/<slug>/` 下

### 1.4 omx plan / ralplan — 共识规划

```bash
# 普通规划
$plan "为项目增加Redis缓存"

# 共识规划（Planner → Architect → Critic 循环）
$ralplan "设计文献分析系统的架构"
$ralplan --interactive "设计..."  # 交互模式
$ralplan --deliberate "设计..."  # 高风险详细模式
```

**关键点**:
- `$ralplan` = `$plan --consensus` 别名
- 默认全自动，`--interactive` 给用户确认节点
- `--deliberate` 添加 pre-mortem + 扩展测试计划

### 1.5 omx setup / agents-init — 项目引导

```bash
# 初始化项目OMX配置
omx agents-init ./my-project

# 初始化 + 覆盖
omx agents-init ./my-project --force

# 健康检查
omx doctor
omx doctor --team   # 检查团队运行时
```

---

## 二、文献系统在OMX框架中的映射

### 2.1 OMX Skill → 系统能力映射

| OMX能力 | 系统中使用场景 |
|---------|--------------|
| `$team` | 并行抓取多数据源、并行分析多篇论文 |
| `$ultragoal` | 管理"获取→筛选→下载→分析→报告"多步骤管线 |
| `$autoresearch-goal` | 周期性深度研究（如"本周量子计算趋势"） |
| `$ralplan` | 新增数据源/功能前的架构规划 |
| `scheduled_task_sop` | 每日定时触发论文监控 |
| `Goal Hive (BBS)` | 多Worker间消息协调、文件共享 |
| `arxiv-daily-researcher` | 核心引擎（数据源/筛选/下载/翻译/报告/通知） |
| `omx agents-init` | 初始化文献项目的AGENTS.md配置 |

### 2.2 调度架构

```
scheduled_task_sop (定时触发)
        │
        ▼
omx team N:executor "每日文献跟踪"
        │
    ┌───┼───────┬───────┬───────┐
    │   │       │       │       │
    ▼   ▼       ▼       ▼       ▼
  Worker1 Worker2 Worker3 Worker4 Worker5
  数据源   筛选    下载    分析    报告
  抓取    评分    PDF    LLM分析  生成
                              │
                              ▼
                       通知推送(多渠道)
```

---

## 三、Phase1 实施步骤

### Step 0: 环境准备
```bash
# 确认arxiv-daily-researcher可运行
cd ~/storage/github/arxiv-daily-researcher
source venv/bin/activate
python main.py --help

# 初始化OMX agents配置
cd ~/storage/github/arxiv-daily-researcher
omx agents-init .
```

### Step 1: 配置文献系统
- 修改 `configs/config.json` 设置关键词（如 `quant-ph` 等）
- 配置通知渠道
- 测试单次运行

### Step 2: 编写OMX AGENTS.md
- 为系统根目录生成AGENTS.md，描述Worker职责分工
- 定义每个Worker的scope

### Step 3: 用omx team第一次并行跑
```bash
omx team 2:executor "运行arxiv-daily-researcher的daily_research模式"
```

### Step 4: 配置定时任务
- 使用 `scheduled_task_sop` 设置每日自动运行

---

## 四、常见问题

**Q: omx team和普通python subprocess的区别？**
- omx team: 每个worker独立Codex CLI会话，tmux持久化，worktree隔离，自动任务协调
- subprocess: 简单的子进程，无隔离/状态管理

**Q: 什么时候用autoresearch-goal vs ultragoal？**
- autoresearch-goal: 研究性质，需教授评判验收（"这篇文献综述做到位了吗"）
- ultragoal: 执行性质，多步骤任务管理（"先抓取、再分析、最后通知"）

**Q: 没有tmux环境怎么办？**
- omx team依赖tmux → 先 `brew install tmux`
- 或者用Codex native subagents（同一会话内）处理小型并行
