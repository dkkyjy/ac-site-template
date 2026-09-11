---
title: "ChatDev 2.0 - 使用指南"
category: "框架 / 工具"
description: "零代码多智能体编排平台 ChatDev 2.0 (DevAll) 的完整使用指南：平台定位、核心概念、前端/命令行用法、典型示例、完整 43 个工作流明细与自定义工作流。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["chatdev", "多智能体", "工作流"]
---

# ChatDev 2.0 (DevAll) — 使用指南与示例

## 一、平台定位

ChatDev 已从最初的"虚拟软件公司"演变为**通用的零代码多智能体编排平台**。你只需要编写 YAML 配置文件（无需代码），就能编排多个 LLM 智能体协作完成复杂任务，如：软件开发、数据可视化、3D 生成、深度调研、教学视频生成等。

运行在：
- **后端** → http://localhost:6400 (FastAPI + 智能体运行时)
- **前端** → http://localhost:5173 (Vue3 + Vite 可视化工作台)
- **API 文档** → http://localhost:6400/docs

---

## 二、核心概念

### 工作流 = YAML 配置文件

每个 `.yaml` 文件定义一个完整的多智能体协作流程，包含：

| 要素 | 说明 |
|------|------|
| **Nodes（节点）** | 任务步骤，可以是智能体(agent)、文字输入(literal)、透传(passthrough)、循环(loop_counter)、子图(subgraph)等类型 |
| **Edges（边）** | 节点间的连接，可设置触发条件、数据传递方式 |
| **Dynamic（动态能力）** | map 并行分拆 / tree 聚合归并，支持大规模并行智能体 |
| **Vars（变量）** | 模板变量，如 `${API_KEY}`、`${MODEL_NAME}` |

### 节点类型

| 类型 | 用途 |
|------|------|
| `agent` | LLM 智能体，配置角色、工具、记忆 |
| `literal` | 直接输入文本（如用户需求） |
| `passthrough` | 透传节点，合并多条路径 |
| `loop_counter` | 循环控制 |
| `subgraph` | 引用另一个 `.yaml` 作为子流程 |
| `function` | 调用工具函数 |

---

## 三、前端用法

打开 http://localhost:5173 ，可以看到可视化工作台：

1. **工作流列表** — 左侧栏展示所有预置工作流
2. **图形化编辑器** — 以 DAG 视图展示智能体节点和连接关系
3. **任务启动面板** — 选择工作流 → 输入任务描述 → 点击执行
4. **运行监控** — 实时查看智能体对话日志、中间结果
5. **YAML 编辑器** — 直接编辑工作流文件

> 注意：首次使用前需在项目根目录的 `.env` 中填入 `BASE_URL` 和 `API_KEY`

---

## 四、命令行用法

### 1. 查看可用工作流

```bash
curl http://localhost:6400/api/workflows
```

返回 42 个预置工作流列表。

### 2. 运行工作流（API）

```bash
curl -X POST http://localhost:6400/api/workflows/run \
  -H "Content-Type: application/json" \
  -d '{
    "yaml_file": "data_visualization_basic.yaml",
    "task_prompt": "请分析附件 sales_data.csv，生成销售趋势图"
  }'
```

### 3. 后端 + 前端同时启动

```bash
cd ~/storage/github/ChatDev
make dev       # 同时启动后端(6400) + 前端(5173)
make stop      # 停止服务
```

---

## 五、典型使用示例

### 示例 1：数据可视化（入门级）

**工作流**：`data_visualization_basic.yaml`

**流程**：用户输入数据 → **数据分析师**判断数据质量 → **数据清洗**（若有问题）→ **可视化工程师**生成图表 → 输出图片

**节点角色**：
- `Data Analyst` — 分析文件，判断下一步 CLEAN 还是 VISUALIZE
- `Data Cleaner` — 缺失值/格式修复
- `Data Visualization` — 使用 matplotlib/seaborn 绘图

**典型输入**："分析我的销售数据，按月份绘制趋势图"

---

### 示例 2：深度调研（中级）

**工作流**：`deep_research_v1.yaml`

**流程**：需求分析 → 逐章规划 → 多线程网页调研(Executor 子图) → 逐章撰写 → 质量审查 → 结构整理 → 输出报告

**节点角色**：
- `Demand Analyst` — 分析用户需求的广度/深度/受众
- `Planner` — 分解为章节查询任务（`<Query>` 标记分拆）
- `Executor` — 子图(`deep_research_executor_sub.yaml`)，每路并行执行搜索
- `Report Writer` — 逐章撰写
- `Quality Reviewer` — 质量审查，可回退到 Planner 或 Writer

**典型输入**："Research about recent advances in the field of LLM-based agent RL"

---

### 示例 3：经典 ChatDev 软件开发（经典版）

**工作流**：`ChatDev_v1.yaml`

**模拟一家虚拟软件公司**，包含多个专业角色：

| 角色 | 职责 |
|------|------|
| CEO | 整体决策 |
| CPO (产品总监) | 需求分析 |
| CTO (技术总监) | 技术方案 |
| Programmer (程序员) | 代码编写 |
| Software Test Engineer (测试) | 测试验证 |
| Code Reviewer (代码审查) | 审查代码 |

**流程**：用户需求 → CEO 分解 → CPO 分析 → CTO 设计架构 → 程序员编码 → 审查 → 测试 → 修改循环

**典型输入**："开发一个 Python 计算器，支持加减乘除和括号"

---

### 示例 4：3D 模型生成（进阶）

**工作流**：`spring_3d.yaml`、`blender_3d_builder_hub.yaml`

**流程**：文字描述 → 智能体撰写 Python/Blender 脚本 → 执行生成 3D 模型

**典型输入**："创建一个旋转的弹簧动画，金属质感"

---

### 示例 5：动态 Map/Tree 并行（高级）

**工作流**：`demo_dynamic.yaml`

演示 **dynamic 节点**的两种并行模式：

| 模式 | 说明 |
|------|------|
| `map` | 将输入消息**分拆**为 N 路，并行执行（如根据地区并行查攻略） |
| `tree` | 将多个结果**聚合并归**（如 N 路查完后合并为最终方案） |

**典型场景**："帮我规划上海旅游——包括景点、美食、住宿、交通"—系统会并行派出 4 个智能体分别查不同维度，再聚合输出

---

### 示例 6：投票与反思

- `demo_majority_voting.yaml` — 多个智能体对同一问题独立回答 → 投票选出最佳答案
- `reflexion_product.yaml` — 智能体先输出 → 自我反思批判 → 改进输出（Reflexion 范式）

---

## 六、工作流分类总览（完整明细）


| 类别 | 工作流 |
|------|--------|
| 🎯 **通用问题求解** | `general_problem_solving_team.yaml` |
| 📊 **数据可视化** | `data_visualization_basic.yaml`、`v2`、`v3` |
| 🔬 **深度调研** | `deep_research_v1.yaml` |
| 💻 **软件开发** | `ChatDev_v1.yaml` |
| 🎮 **游戏开发** | `GameDev_with_manager.yaml` |
| 🎨 **3D 生成** | `spring_3d.yaml`、`blender_3d_builder_*.yaml` |
| 🎓 **教学视频** | `teach_video.yaml` |
| 🔄 **反思优化** | `reflexion_product.yaml` |
| 🗳️ **投票机制** | `demo_majority_voting.yaml` |
| ⚡ **动态并行** | `demo_dynamic.yaml`、`demo_dynamic_tree.yaml` |
| 🔗 **图网络 (MacNet)** | `MACNet_v1.yaml`、`MACNet_optimize_sub.yaml` |
| 🛠️ **功能演示** | `demo_function_call.yaml`、`demo_mcp.yaml`、`demo_code.yaml` |
| 🧠 **记忆增强** | `demo_simple_memory.yaml`、`demo_mem0_memory.yaml`、`demo_file_memory.yaml` |
| 🔁 **循环控制** | `demo_loop_counter.yaml`、`demo_loop_timer.yaml` |

分类明细：

## 📊 数据可视化（3个）
| 文件 | 说明 |
|------|------|
| `data_visualization_basic.yaml` | 基础数据可视化 |
| `data_visualization_enhanced_v2.yaml` | 增强版 v2 |
| `data_visualization_enhanced_v3.yaml` | 增强版 v3（最新） |

## 🎮 3D/游戏/Blender（9个）
| 文件 | 说明 |
|------|------|
| `blender_3d_builder_hub.yaml` | Blender 3D 构建 Hub |
| `blender_3d_builder_hub_auto_human.yaml` | Blender 3D 自动人物 |
| `blender_3d_builder_simple.yaml` | Blender 3D 简洁版 |
| `blender_scientific_illustration_image_gen.yaml` | 科学插图图像生成 |
| `blender_scientific_illustration_with_human.yaml` | 科学插图（含人工审核） |
| `spring_3d.yaml` | 弹簧 3D 动画 |
| `spring_text_image.yaml` | 弹簧文本+图像（中文） |
| `spring_text_image_EN.yaml` | 弹簧文本+图像（英文） |
| `GameDev_with_manager.yaml` | Pygame 游戏开发（含管理节点） |

## 🕸 网络/MACNet（5个）
| 文件 | 说明 |
|------|------|
| `MACNet_v1.yaml` | MACNet 主版本 |
| `MACNet_Node_sub.yaml` | MACNet 节点子图 |
| `MACNet_optimize_sub.yaml` | MACNet 优化子图 |
| `net_example.yaml` | 网络论文生成（中文） |
| `net_example_en.yaml` | 网络论文生成（英文） |

## 🧠 深度研究/推理（4个）
| 文件 | 说明 |
|------|------|
| `deep_research_v1.yaml` | 深度研究 |
| `deep_research_executor_sub.yaml` | 深度研究执行器子图 |
| `react.yaml` | ReAct 智能体 |
| `reflexion_product.yaml` | Reflexion 产品头脑风暴 |

## 🔧 通用/编程（4个）
| 文件 | 说明 |
|------|------|
| `general_problem_solving_team.yaml` | 通用问题解决团队 |
| `demo_code.yaml` | 代码运行 |
| `skills.yaml` | 技能工作流 |
| `ChatDev_v1.yaml` | ChatDev v1 原始版 |

## 🎓 教学视频（1个）
| 文件 | 说明 |
|------|------|
| `teach_video.yaml` | Manim 教学视频生成 |

## 🧪 特性演示（16个）
涵盖 ChatDev 各种框架特性：
- `demo_dynamic.yaml`（动态图）、
- `demo_sub_graph.yaml`（子图）、
- `demo_mcp.yaml`（MCP 集成）、
- `demo_mem0_memory.yaml`（记忆增强）、
- `demo_majority_voting.yaml`（多数投票）、
- `demo_human.yaml`（人工介入）、
- `demo_function_call.yaml`（函数调用）、
- `demo_loop_counter/timer.yaml`（循环控制）等。

---

最有用的非演示工作流（建议尝试）：`general_problem_solving_team.yaml`、`deep_research_v1.yaml`、`data_visualization_enhanced_v3.yaml`。有没有感兴趣的想运行看看？

---

## 七、快速上手建议

**第一步** → 在 `.env` 填入 API Key（需要可用的 LLM API）

**第二步** → `make dev` 启动服务

**第三步** → 打开 http://localhost:5173 前端

**第四步** → 选择一个 demo 工作流（如 `data_visualization_basic.yaml` 或 `demo_human.yaml`），在 UI 中点击运行

**第五步** → 观察智能体之间的对话过程、中间结果和最终输出

---

## 八、自定义工作流

你可以在 `yaml_instance/` 目录下创建新的 `.yaml` 文件，定义自己的多智能体协作流程。核心要素：

```yaml
version: 0.0.0
vars: {}           # 模板变量
graph:
  id: my_workflow
  nodes:
    - id: agent1
      type: agent   # 或 literal / passthrough / subgraph ...
      config:
        name: gpt-4o
        provider: openai
        role: "你的角色提示词"
        base_url: ${BASE_URL}
        api_key: ${API_KEY}
  edges:
    - from: agent1
      to: agent2
      trigger: true
      condition: 'true'
  start: [agent1]
```

保存后后端会自动发现，前端刷新即可看到新工作流。