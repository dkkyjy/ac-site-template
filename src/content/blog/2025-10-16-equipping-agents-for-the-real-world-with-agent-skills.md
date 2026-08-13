---
title: "为现实世界装备智能体：Agent Skills"
description: "*更新：我们已将* [*Agent Skills*](https://agentskills.io/) *发布为跨平台可移植性的开放标准。（2025年12月18日）* 随着模型能力的提升，我们现在可以构建能够与完整计算环境交互的通用智能体。例如，[Claude Code](https://claude.com/product/claude-code) 可以通过本地代码执行和文件系统完成跨领域的复杂"
pubDate: "2025-10-16"
heroImage: "/post_img.png"
tags: ["anthropic-engineering"]
originalLink: "https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills"
---

*更新：我们已将* [*Agent Skills*](https://agentskills.io/) *发布为跨平台可移植性的开放标准。（2025年12月18日）*

随着模型能力的提升，我们现在可以构建能够与完整计算环境交互的通用智能体。例如，[Claude Code](https://claude.com/product/claude-code) 可以通过本地代码执行和文件系统完成跨领域的复杂任务。但随着这些智能体变得愈发强大，我们需要更可组合、可扩展且可移植的方式来为它们配备领域专业知识。

这促使我们创建了 [**Agent Skills**](https://www.anthropic.com/news/skills)：一组组织有序的文件夹，包含指令、脚本和资源，智能体可以动态发现并加载它们，以在特定任务上表现更佳。Skills 通过将你的专业知识打包成可组合的资源，扩展 Claude 的能力，将通用智能体转变为满足你需求的专用智能体。

为智能体构建一个 Skill，就像是为新员工准备一份入职指南。无需为每个用例构建碎片化、定制设计的智能体，任何人都可以通过捕获和分享自己的流程知识，用可组合的能力来专业化自己的智能体。在本文中，我们将解释 Skills 是什么，展示它们如何工作，并分享构建自己的 Skill 的最佳实践。

一个 Skill 是一个目录，其中包含一个 `SKILL.md` 文件，该文件包含组织有序的指令、脚本和资源文件夹，为智能体提供额外能力。

## Skill 的结构

为了解 Skills 的实际运用，让我们看一个真实示例：支撑 [Claude 最近推出的文档编辑能力](https://www.anthropic.com/news/create-files) 的其中一个 Skill。Claude 已经对理解 PDF 了解很多，但直接操作它们（例如填写表单）的能力有限。这个 [PDF skill](https://github.com/anthropics/skills/tree/main/document-skills/pdf) 让我们赋予 Claude 这些新能力。

在最简单的情况下，一个 Skill 是一个包含 `SKILL.md` 文件的目录。这个文件必须以 YAML 前置元数据开头，其中包含一些必需的元数据：`name` 和 `description`。启动时，智能体会将每个已安装 Skill 的 `name` 和 `description` 预加载到系统提示词中。

这个元数据是**渐进式披露**的**第一层级**：它只提供足够的信息，让 Claude 知道何时该使用每个 Skill，而无需将所有内容加载到上下文中。该文件的实际正文是**第二层级**的细节。如果 Claude 认为该 Skill 与当前任务相关，它会通过读取完整的 `SKILL.md` 到上下文中来加载该 Skill。

一个 SKILL.md 文件必须以包含文件名和描述的 YAML 前置元数据开头，该元数据在启动时被加载到系统提示词中。

随着 Skill 复杂性的增加，它们可能包含太多上下文，无法放入一个单一的 `SKILL.md` 中，或者某些上下文只在特定场景下才相关。在这些情况下，Skills 可以在 Skill 目录内打包额外的文件，并从 `SKILL.md` 中按名称引用它们。这些额外的链接文件是**第三层级**（及之后）的细节，Claude 可以根据需要选择导航和发现它们。

在下面展示的 PDF skill 中，`SKILL.md` 引用了两个额外文件（`reference.md` 和 `forms.md`），Skill 作者选择将它们与核心 `SKILL.md` 捆绑在一起。通过将表单填写指令移到单独的文件（`forms.md`）中，Skill 作者能够保持核心 Skill 的精简，相信 Claude 只在填写表单时才会读取 `forms.md`。

你可以通过额外文件将更多上下文纳入 Skill，然后 Claude 可以根据系统提示词触发它们。

渐进式披露是使 Agent Skills 灵活且可扩展的核心设计原则。就像一本组织良好的手册，从目录开始，然后是具体章节，最后是详细的附录，Skills 让 Claude 只在需要时加载信息：

拥有文件系统和代码执行工具的智能体在处理特定任务时，无需将整个 Skill 读入其上下文窗口。这意味着可以打包到 Skill 中的上下文量实际上是无限的。

### Skills 与上下文窗口

下图展示了当用户消息触发 Skill 时，上下文窗口如何变化。

Skills 通过你的系统提示词在上下文窗口中被触发。

所示的操作序列：

1. 开始时，上下文窗口包含核心系统提示词、每个已安装 Skill 的元数据，以及用户的初始消息；
2. Claude 通过调用 Bash 工具读取 `pdf/SKILL.md` 的内容来触发 PDF skill；
3. Claude 选择读取与该 Skill 捆绑的 `forms.md` 文件；
4. 最后，Claude 在加载了 PDF skill 的相关指令后，继续执行用户的任务。

### Skills 与代码执行

Skills 还可以包含供 Claude 自行决定执行的代码作为工具。

大型语言模型在许多任务上表现出色，但某些操作更适合传统的代码执行。例如，通过 token 生成来排序列表远比运行一个排序算法昂贵得多。除了效率问题，许多应用程序需要只有代码才能提供的确定性可靠性。

在我们的示例中，PDF skill 包含一个预编写的 Python 脚本，用于读取 PDF 并提取所有表单字段。Claude 可以运行这个脚本，而无需将脚本或 PDF 加载到上下文中。由于代码是确定性的，这个工作流是一致且可重复的。

Skills 还可以包含供 Claude 根据任务性质自行决定执行的代码作为工具。

## 开发和评估 Skills

以下是一些有助于开始编写和测试 Skills 的指南：

* **从评估开始：** 通过在代表性任务上运行智能体，观察它们在哪些方面遇到困难或需要额外上下文，从而识别智能体能力中的具体差距。然后逐步构建 Skills 来解决这些不足。
* **为扩展而结构化：** 当 `SKILL.md` 文件变得难以管理时，将其内容拆分为单独的文件并引用它们。如果某些上下文互斥或很少一起使用，保持路径分离将减少 token 使用。最后，代码既可以作为可执行工具，也可以作为文档。应该明确 Claude 是直接运行脚本，还是将其作为参考读入上下文。
* **从 Claude 的角度思考：** 监控 Claude 在真实场景中如何使用你的 Skill，并根据观察结果进行迭代：注意意外的轨迹或对某些上下文的过度依赖。特别关注你 Skill 的 `name` 和 `description`。Claude 在决定是否为了响应当前任务而触发该 Skill 时，会用到这些信息。
* **与 Claude 一起迭代：** 在与 Claude 一起处理任务时，请 Claude 将其成功的方法和常见错误捕获到 Skill 中的可重用上下文和代码中。如果它在使用 Skill 完成任务时偏离了方向，请它自我反思哪里出了问题。这个过程将帮助你发现 Claude 实际需要哪些上下文，而不是试图提前预设。

### 使用 Skills 的安全考虑

Skills 通过指令和代码为 Claude 提供新能力。虽然这使它们强大，但也意味着恶意 Skills 可能会在使用环境中引入漏洞，或指示 Claude 泄露数据并采取意外行动。

我们建议只从可信来源安装 Skills。当从不太可信的来源安装 Skill 时，在使用前彻底审计它。首先阅读 Skill 捆绑的文件内容，了解其功能，特别注意代码依赖项和捆绑的资源（如图像或脚本）。同样，注意 Skill 内指示 Claude 连接到可能不受信任的外部网络源的指令或代码。

## Skills 的未来

Agent Skills 现已[在](https://www.anthropic.com/news/skills) [Claude.ai](http://claude.ai/redirect/website.v1.74943c77-6bb4-4c2c-8123-9b24b2b2aa94)、Claude Code、Claude Agent SDK 和 Claude 开发者平台上[得到支持](https://www.anthropic.com/news/skills)。

在未来几周内，我们将继续添加支持 Skills 创建、编辑、发现、分享和使用的完整生命周期的功能。我们特别期待 Skills 能够帮助组织和个人与 Claude 分享他们的上下文和工作流程。我们还将探索 Skills 如何通过教会智能体更复杂的涉及外部工具和软件的工作流程，来补充 [Model Context Protocol](https://modelcontextprotocol.io/) (MCP) 服务器。

放眼更远的未来，我们希望使智能体能够自行创建、编辑和评估 Skills，让它们将自己的行为模式编码为可重复使用的能力。

Skills 是一个简单的概念，具有相应的简单格式。这种简单性使组织、开发者和最终用户更容易构建定制化的智能体并赋予它们新能力。

我们很高兴看到人们用 Skills 构建出什么。今天就开始吧，查看我们的 Skills [文档](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview)和[食谱](https://github.com/anthropics/claude-cookbooks/tree/main/skills)。

## 致谢

由 Barry Zhang、Keith Lazuka 和 Mahesh Murag 撰写，他们都非常喜欢文件夹。特别感谢 Anthropic 中许多其他 champion、支持和构建 Skills 的人。
