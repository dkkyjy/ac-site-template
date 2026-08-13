---
title: "引导 Claude Code：何时使用 CLAUDE.md、技能、钩子和子智能体"
description: "引导 Claude Code：何时使用 CLAUDE.md、技能、钩子和子智能体"
pubDate: "2026-06-18"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more"
---

# 引导 Claude Code：何时使用 CLAUDE.md、技能、钩子和子智能体

            ---

            /\* 博客嵌入与代码块的流体断行 \*/
.u-rich-text-blog .w-embed,
.u-rich-text-blog pre.w-code-block {
--max-w: 860px;
--gutter: 24px;
--available: calc(100vw - (var(--gutter) \* 2));
--w: min(var(--max-w), var(--available));
width: var(--w);
max-width: var(--w);
margin-left: calc((640px - var(--w)) / 2);
margin-right: calc((640px - var(--w)) / 2);
box-sizing: border-box;
}
@media (max-width: 720px) {
.u-rich-text-blog .w-embed,
.u-rich-text-blog pre.w-code-block {
width: 100%;
max-width: 100%;
margin-left: 0;
margin-right: 0;
}
/\* 将文章列限制在视口内，防止内容溢出页面 \*/
.blog\_post\_layout.u-column-custom,
.blog\_post\_content\_wrap,
.u-rich-text-blog {
max-width: 100% !important;
box-sizing: border-box;
}
html,
body {
overflow-x: hidden;
}
}
/\* 嵌入内部包装器：当内容溢出时水平滚动 \*/
.u-rich-text-blog .w-embed figure {
width: 100% !important;
max-width: 100% !important;
margin: 0 !important;
}
.u-rich-text-blog .w-embed figure > div {
width: 100% !important;
max-width: 100% !important;
overflow-x: auto !important;
-webkit-overflow-scrolling: touch;
}
/\* 表格：宽屏上的比例，移动端上自然宽度加滚动 \*/
.u-rich-text-blog .w-embed table {
width: 100% !important;
table-layout: fixed !important;
}
.u-rich-text-blog .w-embed table th:nth-child(1),
.u-rich-text-blog .w-embed table td:nth-child(1) {
width: 22%;
}
.u-rich-text-blog .w-embed table th:nth-child(2),
.u-rich-text-blog .w-embed table td:nth-child(2) {
width: 39%;
}
.u-rich-text-blog .w-embed table th:nth-child(3),
.u-rich-text-blog .w-embed table td:nth-child(3) {
width: 39%;
}
.u-rich-text-blog .w-embed td code,
.u-rich-text-blog .w-embed th code {
overflow-wrap: anywhere;
word-break: break-word;
white-space: normal;
}
@media (max-width: 639px) {
.u-rich-text-blog .w-embed table {
width: auto !important;
min-width: 640px !important;
table-layout: auto !important;
}
.u-rich-text-blog .w-embed table th,
.u-rich-text-blog .w-embed table td {
min-width: 0 !important;
width: auto !important;
}
}
/\* 代码块 \*/
.u-rich-text-blog pre.w-code-block {
overflow-x: auto;
-webkit-overflow-scrolling: touch;
}
@media (max-width: 639px) {
.u-rich-text-blog pre.w-code-block {
font-size: 0.82rem;
}
}

.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) {
font-size: var(--\_typography---font-size--body-3);
line-height: 1.55;
}
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th,
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td {
font-size: var(--\_typography---font-size--body-3);
line-height: 1.55;
padding: 16px 18px;
overflow-wrap: anywhere;
word-break: normal;
hyphens: none;
vertical-align: top;
}
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th:nth-child(1),
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td:nth-child(1) { width: 16%; }
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th:nth-child(2),
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td:nth-child(2) { width: 20%; }
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th:nth-child(3),
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td:nth-child(3) { width: 24%; }
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th:nth-child(4),
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td:nth-child(4) { width: 16%; }
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th:nth-child(5),
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td:nth-child(5) { width: 24%; }
@media (max-width: 639px) {
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) th,
.u-rich-text-blog .w-embed table:has(thead tr th:nth-child(5)) td {
padding: 12px 14px;
line-height: 1.5;
}
}

Claude 的设计宗旨是适应你的工作方式，而 Claude Code 允许你对其进行自定义。

有七种方法可以指导 Claude 的行为：CLAUDE.md 文件、规则、[**技能**](https://code.claude.com/docs/en/skills)、[**子智能体**](https://code.claude.com/docs/en/sub-agents)、[**钩子**](https://code.claude.com/docs/en/hooks-guide)、输出样式以及追加系统提示。

每种方法控制：

* 指令何时加载到上下文中；
* 在长时间会话中是否持续（压缩行为）；以及
* 指令的权威程度。

下表快速总结了每种方法的主要区别，而本文提供了更详细的说明和决策框架，帮助你确定每条 Claude 指令应归属于哪种方法。

| 方法 | 加载时机 | 压缩行为 | 上下文代价 | 何时使用 |
| --- | --- | --- | --- | --- |
| CLAUDE.md（根目录） | 会话开始时；整个会话期间保持在上下文中 | 记忆化。读取一次并在会话期间缓存；压缩后清除缓存并重新读取 | 高。每行都会消耗令牌，无论是否相关 | 构建命令、目录布局、单仓结构、编码约定、团队规范 |
| CLAUDE.md（子目录） | 按需加载，当 Claude 读取该子目录下的文件时 | 丢失，直到再次接触该子目录 | 低。仅在处理相关子目录时才消耗上下文 | 特定于子目录的约定 |
| 规则 | 会话开始时（用户级规则）或仅当匹配的文件被触及时（路径限定） | 压缩时重新注入 | 中。除非路径限定，否则始终开启 | 特定约束或约定（例如，所有 API 处理器必须使用 Zod 验证输入） |
| 技能 | 会话开始时加载名称和描述；当技能被调用时加载完整内容 | 已调用的技能在共享预算内重新注入；最旧的优先丢弃 | 低。仅当调用时才加载完整内容；受调用技能的共享令牌预算限制 | 程序性工作流（部署或发布检查清单） |
| 子智能体 | 会话开始时加载名称、描述和工具列表；内容仅在通过智能体工具调用时加载 | 只有最后一条消息（摘要及元数据）返回主会话 | 低。在调用前不消耗主上下文；在其自己的隔离上下文窗口中运行 | 并行运行工作或需要隔离运行且仅返回摘要的侧面任务（深度搜索、日志分析、依赖审计） |
| 钩子 | 在生命周期事件中触发 | 完全绕过压缩 | 低。配置位于主上下文之外；部分输出可能返回（例如阻塞错误） | 确定性自动化：运行 linter、完成后发布到 Slack、阻塞命令、在 PreCompact 时备份聊天历史 |
| 输出样式 | 会话开始时；注入到系统提示中 | 永不压缩 | 高。占用上下文窗口，但会覆盖默认系统提示 | 重大的角色变更（代码助手转变为通用助手） |
| 追加系统提示 | 会话开始时；作为 CLI 标志传入 | 永不压缩；仅适用于该次调用 | 中等。会话中第一次请求后缓存 | 语气、回复长度、格式偏好 |

## 传递指令的七种方法

有七种方式可以自定义 Claude Code 的行为：CLAUDE.md 文件提供始终开启的项目上下文，规则提供硬性约束，技能提供可重用的程序，子智能体用于委派工作，钩子用于确定性自动化，输出样式或系统提示附加用于全局变更。每种方法都在上下文代价和权威程度之间权衡——选择正确的方法是最主要的工作。

### CLAUDE.md 文件

CLAUDE.md 是项目根目录下的一个 markdown 文件。它在会话开始时加载到上下文中，并在整个会话期间保留。

构建命令、目录布局、单仓结构、编码约定和团队规范都自然适合放在这里。

有两种类型，它们的加载方式不同：

* **始终加载**：第一种是根目录下的 CLAUDE.md 文件，可以放在共享仓库中，也可以本地保存以记录你对某个项目的个人偏好。所有这类文件在会话开始时加载，在长时间会话中不会丢失或降级。当 Claude Code 压缩对话时，它会重新读取这些文件。
* **按需加载：** 位于初始化会话的文件夹下子目录中的 CLAUDE.md 文件。例如，`app/api/CLAUDE.md` 在 Claude 读取 `app/api` 下的文件时加载，而不是在会话开始时。它与路径限定规则具有相同的压缩行为：在再次接触该子目录之前消失。

当前工作目录以下的所有子目录 CLAUDE.md 文件都会在 Claude 读取该目录内的文件时加载。

在共享仓库中，CLAUDE.md 会像任何无人管理的配置文件一样增长：每个团队都会添加自己的指令，但没有人会删除过时的内容。这种代价会随着规模扩大而递增。

每一行都会加载到仓库中每位工程师的每个会话中，无论是否与他们的任务相关。这会消耗令牌，并削弱对真正重要指令的遵从度。随着文件增长，应将团队特定的约定推送到路径限定规则中，将程序推送到技能中，这样它们只会在相关时加载。

**提示：** 将 CLAUDE.md 保持在 200 行以内，指定责任人，并像审查代码一样审查其变更。内容本身应遵循与任何提示相同的规则：[编写有效的提示](https://claude.com/blog/best-practices-for-prompt-engineering)意味着要明确具体、解释约束背后的原因，并展示示例。

可以将此文件视为向 Claude 提供代码库概览，或作为一个索引，指向 Claude 在需要时可以找到更多信息的其他文件。

在单仓中，为每个团队的目录分配自己的子目录 CLAUDE.md，这样每个团队只加载自己的约定，开发者可以使用 `claudeMdExcludes` 设置跳过他们从不接触的团队的文件。

对于必须应用于组织中每个仓库的标准（安全策略、合规要求），可以通过 MDM 或配置管理将集中管理的 CLAUDE.md 部署到开发者的机器上，并且不能通过个人设置排除。

更多关于设置 CLAUDE.md 的信息，请参阅我们的博客文章：[CLAUDE.md 文件：为你的代码库自定义 Claude Code](https://claude.com/blog/using-claude-md-files)。

### 规则

[**规则**](https://code.claude.com/docs/en/memory#organize-rules-with-claude/rules/) 是位于 `.claude/rules/` 目录下的 markdown 文件，用于向 Claude 提供特定的约束或约定。

无范围限定的规则行为类似于 CLAUDE.md，始终在会话开始时加载，并在压缩时重新注入。这可能会浪费令牌，因为即使上下文与手头任务无关也会被加载。

路径限定规则允许你仅在相关时加载规则指令，通过添加 `paths` 字段来控制加载时机。

例如：一个限定在 `src/api/**` 路径下的规则，在处理纯文档会话时不会进入上下文。只有当 Claude 读取 `src/api/` 目录内的文件时才会加载。

示例如下：

```
---
paths:
  - "src/api/**"
  - "**/*.handler.ts"
---
所有 API 处理器必须在使用 Zod 处理输入之前进行验证。
```

**提示**：文件特定的约束，例如“迁移文件只能追加”，最适合作为**规则**放在你的 `paths:` 前置元数据中。当指令涉及跨多个（但非全部）代码库领域的关注点或文件时，应优先使用路径限定规则，而不是嵌套的 CLAUDE.md 文件。

### 技能

[**技能**](https://code.claude.com/docs/en/skills) 位于 `.claude/skills/` 目录下，是包含指令、脚本和资源的文件夹，Claude 会动态加载它们。每个技能都有一个 `SKILL.md` 文件，包含名称、描述和正文。

只有名称和描述在会话开始时加载；当 Claude 调用技能时，完整正文才会加载，调用方式可以是斜杠命令（`/code-review`），也可以是自动匹配任务。

技能通过你的系统提示触发。

例如，`/code-review` 是一个内置技能，用于审查当前差异并报告发现，而无需编辑文件。该技能定义了操作手册，因此每次调用时 Claude 都会遵循相同的结构化方法。

压缩时，Claude Code 会在所有已调用技能的总预算内重新注入技能。如果你在会话期间调用了许多技能，最旧的技能会先被丢弃。

**提示：** 程序性指令，例如部署工作流、发布检查清单或审查流程，应放在技能中，而不是 CLAUDE.md 中。

Claude Code 自带一些技能，但你也可以编写自己的自定义技能。我们的[构建 Claude 技能的完整指南](https://claude.com/blog/complete-guide-to-building-skills-for-claude)会教你如何操作。

### 子智能体

[**子智能体**](https://code.claude.com/docs/en/sub-agents) 是位于 `.claude/agents/` 目录下的 markdown 文件，用于定义用于特定侧面任务的隔离助手。每个文件使用 YAML 前置元数据（名称、描述，以及可选的模型和工具访问字段），后跟一个正文，该正文将成为子智能体的系统提示。

子智能体与技能类似，名称、描述和工具列表在会话开始时加载，但子智能体正文中的较大上下文不会自动调用。Claude 通过智能体工具调用它们，并传入一个提示字符串。

Claude Code 的上下文窗口包含 Claude 关于你的会话所知道的一切。[此处的交互式时间轴](https://code.claude.com/docs/en/context-window)展示了什么内容在何时加载。

子智能体正文中的较大指令上下文不仅不会自动调用，而且永远不会进入父对话。

然后，子智能体在其自己的全新上下文窗口中运行，唯一返回主会话的是子智能体的最后一条消息（通常是许多子任务的聚合结果）加上元数据。

这种模式具有可扩展性：子智能体可以嵌套多达五层，[动态工作流](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)可以协调数十到数百个后台智能体，而无需你指定子智能体架构的每个细节。协调计划和中间结果保存在脚本变量中，而不是 Claude 的上下文窗口中，从而在保持指令保真度的同时实现扩展。

**提示：** 这种隔离是选择子智能体而非技能的主要原因之一。当侧面任务（如深度搜索、日志分析或依赖审计）会以你不再会引用的中间结果弄乱主对话时，请使用子智能体。当你希望程序在主线程内执行，以便你可以看到并引导每一步时，请使用技能。

### 钩子

[**钩子**](https://code.claude.com/docs/en/hooks-guide) 是用户定义的命令、HTTP 端点或 LLM 提示，通过在 Claude 生命周期中的[特定事件](https://code.claude.com/docs/en/hooks#hook-lifecycle)（如文件编辑、工具调用或会话开始）触发，从而提供对 Claude 行为的更确定性控制。

Claude Code 会话中钩子可以触发的事件映射。

你在 `settings.json`、托管策略设置或技能/智能体前置元数据中注册钩子。

有几种类型的钩子：命令、HTTP、mcp\_tool、提示和智能体。所有钩子都是确定性触发的。前三种可确定性地执行，而后两种（提示和智能体）使用 Claude 的判断而非一组规则来确定输出。

钩子的上下文代价较低，因为配置或指令位于主上下文窗口之外。框架运行处理程序（命令、HTTP、mcp\_tool）或使用单独的窗口进行模型调用（提示、智能体），具体取决于钩子类型。

某些钩子的输出可能会保存到主上下文窗口中。例如，阻塞钩子的标准错误会保存在上下文中，以便 Claude 知道该调用为何被拒绝。

但大多数钩子的输出不会保存到主窗口，除非配置明确返回它。如果你在压缩之前使用 `PreCompact` 事件将聊天历史备份到另一个文件以供以后引用，Claude 将不知道哪个文件保存了聊天历史。

这使得这些钩子类型与 CLAUDE.md、规则和技能有根本区别。你可以在我们的文章[**如何配置钩子**](https://claude.com/blog/how-to-configure-hooks)中了解更多信息。

**提示：** 将钩子用于任何应该确定性发生的事情：在编辑后运行 linter、完成后发布到 Slack，或在执行前阻止特定命令。`PreToolUse` 钩子可以检查任何工具调用，并以退出码 2 拒绝它。

它们的上下文代价较低，因为它们是框架运行的代码，而不是加载到上下文中的 Claude 指令。

### 输出样式

[**输出样式**](https://code.claude.com/docs/en/output-styles) 是位于 `.claude/output-styles/` 目录下的文件，用于将指令注入系统提示。它们永远不会被压缩，在每次会话开始时加载，并在会话中的第一个请求后被缓存，这意味着它们的上下文代价中等。

由于它们位于系统提示中，输出样式在我们迄今介绍的所有方法中具有最高的指令遵循权重，应谨慎使用。

**对输出样式的更改将替换默认的输出样式**（除非你在样式的前置元数据中设置了 `keep-coding-instructions: true`）。

在 Claude Code 中，这会移除那些告诉 Claude 它正在帮助用户处理软件工程任务的指令，以及其他关键默认指令，例如：

* 如何限定更改范围；
* 何时添加或省略代码注释；
* 如何处理安全问题；以及
* 验证习惯，例如在宣布工作完成之前运行测试。

默认情况下，自定义输出样式会丢弃所有这些内容，Claude Code 变得更像一个通用助手，而不是软件工程助手。

**提示**：在编写自定义输出样式之前，请检查内置样式。**主动型**、**解释型**和**学习型**覆盖了最常见的需求（自主性、教学模式、协作编码），而无需你维护样式文件。

### 追加系统提示

修改输出样式的替代方案是 `append-system-prompt` 标志。修改输出样式文件可能导致 Claude 行为发生重大意外变化，而追加标志仅是对原始系统提示的补充。它不会修改 Claude 的角色；只是在其默认角色中添加指令。

它也是在调用时传入的，仅适用于该次调用，而不是像文件那样跨会话持久化。

与其他传递指令的方法相比，追加系统提示可能具有更高的上下文代价。它会增加输入令牌，尽管提示缓存会在会话中第一次请求后降低此代价。指示 Claude 使用更冗长或更长的样式也会增加输出令牌。

**提示：** 追加系统提示最适合添加特定的编码标准、输出格式或领域特定知识。请记住，追加系统提示的遵循效果会递减。通常，使用此方法提供的指令越多，Claude 遵守的程度就越低，特别是在存在矛盾的情况下。

## 何时使用每种方法

如果你发现自己出现以下情况之一，你可能需要考虑为你的指令寻找其他位置：

**在 CLAUDE.md 中写“每次 X 时，始终执行 Y”。** 如果该行为应该可靠地发生，例如每次编辑后运行 prettier 或完成后发布到 Slack，请改用 `settings.json` 中的钩子。模型选择运行格式化程序与格式化程序自动运行是不同的。

**在 CLAUDE.md 中写“永远不要这样做”。** 当存在绝对不允许发生的事情时，指令是错误的工具。Claude 大多数时候会遵循指令，但在压力下、长时间会话或模糊情况下，或者由于任务中访问的文件存在提示注入，模型可能无法遵循提示规则。真正的护栏需要确定性，而执行方法是[钩子](https://code.claude.com/docs/en/hooks)和[权限](https://code.claude.com/docs/en/permissions)。`PreToolUse` 钩子可以检查调用并以退出码 2 阻止它。[**托管设置**](https://code.claude.com/docs/en/settings#managed-settings)更进一步：它们由管理员部署，不能被用户的本地配置覆盖，并且是强制执行确定性、组织级护栏的唯一方法。

**在 CLAUDE.md 中写一个 30 行的程序。** 程序属于技能。CLAUDE.md 用于 Claude 应始终掌握的事实：构建命令、单仓布局、团队约定。部署运行手册或安全审查检查清单应位于 `.claude/skills/` 中，其正文仅在调用时加载。

**一个没有路径限定的 API 特定规则。** 如果某个规则仅适用于 `src/api/**`，使用 `paths:` 限定它可以在不相关的工作期间使其脱离上下文。无范围限定的规则在机械上与将内容放入 CLAUDE.md 相同：始终加载，始终消耗令牌。

**将个人偏好写入项目级别的 CLAUDE.md 文件。** 所有基于文件的方法都有一个用户级别的对应项，无论你在哪个仓库中，都会为每个 Claude Code 会话加载。使用本地文件存储个人偏好（始终使用语义提交消息）。将项目级别文件用于团队范围但特定于某个代码库的偏好。

## Claude Code 自定义入门

你可以在我们的 [Claude Code 最佳实践](https://code.claude.com/docs/en/best-practices#write-an-effective-claude-md)文档中找到更多技巧和模式，以充分利用 Claude Code，从配置环境到跨并行会话扩展。

一旦你掌握了其中几个工具，你可以将它们（技能、子智能体、钩子、输出样式）打包成一个[插件](https://code.claude.com/docs/en/plugins)，以便在队友或项目之间共享一致的设置。

*本文由 Anthropic 员工 Michael Segner 撰写。*

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保你的最终完整响应，包括执行指令时生成的任何章节标题或标题，仅使用 zh-CN 语言编写。
