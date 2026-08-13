---
title: "TencentDB Agent Memory"
description: "让 Agent 沉淀经验、记忆资产可复用的 Memory Hub — 从对话中自动提炼 Chat Memory 与 Skill，把文档和代码变成 Wiki 与 CodeGraph，通过 L0–L3 分层记忆 + ACL 权限装配，让新 Agent 第一天就能继承团队积累的经验。"
pubDate: "Aug 13 2026"
updatedDate: "Aug 13 2026"
heroImage: "/tencentdb-agent-memory.png"
badge: "active"
github: "https://github.com/TencentCloud/TencentDB-Agent-Memory"
tags: ["agent", "memory", "llm", "tencentdb", "rag", "skills", "codegraph", "docker"]
---

**TencentDB Agent Memory** 是腾讯云开源的 Agent 记忆中枢，解决一个实际问题：**怎样减少使用 Agent 时的重复工作？**

项目背景讲过了，不该换个 Session 再讲。文档读过了，不该每个 Agent 从第一页重读。一套做法已经跑通，不该下次再摸索一遍。这里的 Memory 不只是"记住对话"——**凡是能让下一个 Agent 少走弯路的信息，都应该被保存、组织并复用。**

```text
已有信息 → 可复用记忆资产 → 更少 Turns → 更少返工 → 更稳定的结果和更高的效率
```

## 核心特性

### 🧠 一个能记住人和事的大脑
- **Chat Memory** 保留偏好、事实、决策和交互历史
- 每个 Agent 创建时自动获得独立记忆，下次对话不必从自我介绍开始
- L0 Conversation → L1 Atom → L2 Scenario → L3 Persona，从原始对话逐层沉淀

### ⚡ 一个会积累经验的 Skill 库
- Agent 做完复杂工作后，从对话和工具调用中提炼可复用 Skill，并按需导入指定 Agent 上下文
- Skill 不只是 Prompt：有版本、资源文件、触发边界、执行步骤和验证规则
- 个人 Skill 默认私有，审核后可分享给团队再配装给其他 Agent

### 📖 一张同时看懂文档和代码的知识地图
- **Wiki** 把产品文档、设计方案和运维手册生成结构化页面与链接图谱
- **CodeGraph** 索引代码符号、文件、调用关系和影响路径

### 🛡️ 由人掌控的团队记忆面板
- 记忆可视化、审核、路由均由人工把控，团队共享经验但不共享隐私

## 技术实现

### 1. 记忆不是平铺记录，而是分层生长
| 层级 | 存储内容 | 主要用途 |
| :--- | :--- | :--- |
| **L0 Conversation** | 带完整上下文的原始对话 | 核对原话、时间戳与来源 |
| **L1 Atom** | 从对话中抽取的事实、偏好、约束与事件 | 精确召回可执行信息 |
| **L2 Scenario** | 围绕项目或场景组织成的知识块 | 快速恢复工作上下文 |
| **L3 Core / Persona** | 长期画像、稳定模式与高层认知 | 让 Agent 快速进入用户与团队语境 |

生成与检索都是分层的：平时用 L2/L3 快速装载上下文；需要具体事实时，BM25 + 向量检索 + RRF 回退到 L1/L0。结果再按条数、字符预算和超时上限封顶，防止记忆撑爆上下文窗口。

### 2. 记忆不是全局 Prompt，而是 Agent 的装备
Chat Memory、Skills、Wiki、CodeGraph 统一注册为 Memory Assets，Memory Hub 通过 **Fixed Binding + ACL** 决定某个 Agent 可用哪些资产：先按 Team、User、Agent、可见性收窄权限范围，再基于当前查询检索。切换 Agent 或框架只需重新装配，无需重新训练。

### 3. 知识不整包注入，而是按需调用
文档组织成可检索的 Wiki 页面（支持链接图谱下钻）；代码库索引为含文件、符号、调用关系的 CodeGraph 资产。Agent 先通过 `/v3/tools/list` 发现能力，再用 `/v3/tools/call` 读取相关页面、源码或影响路径——文档和代码也变成记忆的一部分，但只在真正需要时才进入上下文。

## 部署架构

一次拉起完整三件套（`memory-core` + `memory-hub` + `proxy`）：

```bash
git clone https://github.com/TencentCloud/TencentDB-Agent-Memory.git
cd TencentDB-Agent-Memory/deploy/global-images
cp .env.example .env
$EDITOR .env       # 填入两组 LLM 参数（memory 组 + proxy 组）
./start-all.sh     # 一键启动；结束会打印可直接粘贴到 Claude 的一行命令
```

打开 Panel：`http://localhost:8125`

## Benchmark

| Benchmark | 未启用 | 启用后 | 相对提升 |
| :--- | :---: | :---: | :---: |
| **PersonaMem** | 48% | **76%** | **+59%** |

PersonaMem 测试 Agent 在长时间交互后能否正确理解并应用用户信息。
