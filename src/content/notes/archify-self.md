---
title: "Archify：AI 架构图 Agent Skill（自画像交互图）"
category: "技能库 / 框架"
description: "tt-a1i/archify 上手实测：一个让 AI Agent 直接产出可交互架构图/流程图/时序图的 Skill，包含安装、authoring 流程，以及用它画出的 Archify 自身架构交互图（中文版，可直接在页面中操作）。"
pubDate: "2026-09-10"
badge: "guide"
tags: ["archify", "skills", "架构图", "可视化", "agent"]
---
# Archify：让 Agent 直接画可交互架构图

> 仓库：https://github.com/tt-a1i/archify （⭐ 56k） · 版本 2.17 · 体验日期 2026-09-10
> 一句话：**Archify 是一个 Agent Skill——你（或 AI Agent）用一份 JSON 描述"系统长什么样"，它编译成一张可缩放、可聚焦、可动画的自包含交互式架构图（HTML）。**

---

## 1. 这是什么

传统画架构图要手动拖框、连线、调布局。Archify 换了个思路：**图是"编译"出来的，不是"画"出来的**。

- 输入：一份符合 JSON Schema 的**图定义**（组件、边界、连接、标签、视图）
- 输出：**单文件 HTML**（内含 SVG + viewer 运行时，零依赖、可离线打开）
- 支持 5 类图：`architecture`（架构）/ `workflow`（流程）/ `sequence`（时序）/ `dataflow`（数据流）/ `lifecycle`（生命周期）

它最特别的是**质量门槛**：交付前必须通过 `validate`（数十项布局诊断：连线不许穿组件、标签不许重叠、不许微型拐角……），不达标就不给过。所以产出是"能看的图"，更是"干净的图"。

## 2. 安装（三行命令的事）

```bash
npx skills add tt-a1i/archify -g -y --agent '*'
node ~/.agents/skills/archify/bin/archify.mjs doctor   # 10 项自检全绿
```

安装后 Claude Code / Codex / Cursor 等 55+ 个 agent 都会自动获得这个技能；在会话里说"画个架构图"即可。

## 3. 一次完整实践：给 Archify 自己画架构图

按官方 **authoring 流程**（定义先行 → 校验 → 交付）画了 Archify 自身的架构：

1. **写定义**（`architecture` 类型）：7 个组件（AI Agent、候选 JSON、CLI、Schema、渲染器、品牌素材、交付 HTML）+ 3 个阶段边界 + 6 条连接 + 3 个聚焦视图 + 3 张说明卡
2. **validate 三连修**：10 诊断 → 6 → 0（教训：连线别手动指定 side/route，让渲染器自动路由；跨层直连是主要失败源）
3. **deliver + check**：输出 808KB 单文件 HTML，9 项结构检查全过

下图就是产物——**可以缩放、点右上角切换 3 个视图、看 trace 动画**：

<iframe src="/archify-self/archify-self.zh.html" width="100%" height="680" frameborder="0" loading="lazy" title="Archify 自画像架构图"></iframe>

> 完整交互页可直接打开：[/archify-self/archify-self.zh.html](/archify-self/archify-self.zh.html)

## 4. 使用要点（踩坑记录）

| 要点 | 说明 |
|---|---|
| 定义先行 | 先写 JSON 候选产物，再谈渲染；校验通过后产物冻结 |
| 自动路由 | 连接不手动指定 `side/route`，渲染器自动布线最干净 |
| 少跨层直连 | 上下排组件直连会穿过中间水平通道 → 先调布局/删冗余连线 |
| 中文友好 | `meta.locale: "zh-CN"` 即可，中文 label 布局一次通过 |
| CLI 子命令 | `render` / `validate` / `deliver` / `compare` / `preview` / `visual-check` |
