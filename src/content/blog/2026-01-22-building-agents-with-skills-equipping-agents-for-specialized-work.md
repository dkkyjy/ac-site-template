---
title: "构建具备技能的智能体：为专业化工作赋能智能体"
description: "过去一年发生了很多变化。MCP 已成为智能体连接的标准，并获得了行业领导者和开发者社区的快速采用。[Claude Code 发布](https://www.anthropic.com/news/claude-3-7-sonnet)，成为通用型编码智能体。我们还推出了 [Claude Agent SDK](https://www.anthropic.com/engineering/building-"
pubDate: "2026-01-22"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/building-agents-with-skills-equipping-agents-for-specialized-work"
---
过去一年发生了很多变化。MCP 已成为智能体连接的标准，并获得了行业领导者和开发者社区的快速采用。[Claude Code 发布](https://www.anthropic.com/news/claude-3-7-sonnet)，成为通用型编码智能体。我们还推出了 [Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)，它现在提供了一个开箱即用的生产级智能体。

但在构建和部署这些智能体的过程中，我们不断遇到同一个差距：智能体拥有智能和能力，但并不总是具备有效处理实际工作所需的专业知识。这促使我们[创建了 Agent Skills（智能体技能）](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)。技能是组织化的文件集合，将领域专业知识——工作流程、最佳实践、脚本——打包成智能体可以访问和应用的格式。它们将一个能干的全能型选手转变为知识渊博的专家。

在这篇文章中，我们将解释为什么我们停止构建专门的智能体，转而开始构建技能，以及这一转变如何改变我们对扩展智能体能力的思考方式。

## **新范式：代码即一切**

我们曾经认为不同领域的智能体会大相径庭。一个编码智能体、一个研究智能体、一个用于金融的智能体、一个用于营销的智能体——每个似乎都需要自己的工具和框架。行业最初接受了这种领域特定智能体的模式。但随着模型智能的提升和智能体能力的进步，我们汇聚到了一种不同的方法上。

我们开始将代码不仅仅视为一个用例，更视为智能体执行几乎所有数字工作的接口。Claude Code 是一个编码智能体，但同时也是一个恰好通过代码工作的通用智能体。

考虑使用 Claude Code 生成财务报告的场景。它可以调用 API 进行研究，将数据存储在文件系统中，用 Python 进行分析，并综合洞察。所有这些都通过代码完成。框架简化为 bash 和一个文件系统。

但通用能力并不等同于专业知识。当我们开始将 Claude Code 用于实际工作时，一个差距出现了。

## **缺失的部分：领域专业知识**

你希望谁来为你报税：一个从基本原理出发推理的数学天才，还是一个经验丰富、处理过数千份报税单的税务专业人士？大多数人会选择税务专业人士。不是因为他们更聪明，而是因为他们拥有正确的专业知识。

今天的智能体就像那个数学天才：在推理新颖情况方面非常出色，但往往缺乏经验丰富的专业人士积累的专业知识。在适当指导下，它们可以做出惊人的事情。然而，它们常常缺少重要的上下文，无法轻松吸收你组织的专业知识，也不会自动从重复任务中学习。

技能通过将领域专业知识打包成智能体可以渐进式访问和应用的格式，弥合了这一差距。

## **什么是 Agent Skills？**

技能将领域专业知识和程序性知识打包给智能体使用。

```
anthropic_brand/
├── SKILL.md
├── docs.md
├── slide-decks.md
└── apply_template.py
```

技能的简单性是刻意为之的。文件是一种通用的基本元素，可以与现有资源配合使用。你可以用 Git 进行版本控制，存储在 Google Drive 中，并与团队共享。这种简单性也意味着技能创建不限于工程师。产品经理、分析师和领域专家已经在构建技能，以编纂他们的工作流程。

## **渐进式披露**

技能可以包含大量信息。为了保护上下文窗口并使技能可组合，它们采用了渐进式披露：在运行时，只有元数据（来自 YAML 前置内容的名称和描述）会展示给模型。

```
---
name: Anthropic 品牌风格指南
description: Anthropic 的官方品牌颜色和排版…
---
```

如果 Claude 确定需要某项技能，它会读取完整的 SKILL.md 文件。如需更多细节，技能可以包含一个 references/ 目录，其中包含仅在需要时加载的支持文档。

这种三层方法意味着你可以用数百项技能装备一个智能体，而不会压垮其上下文窗口——元数据使用约 50 个令牌，完整的 SKILL.md 文件约 500 个令牌，参考文件 2,000+ 个令牌且仅在特定需要时加载。

## **技能可以将脚本作为工具包含**

传统工具有一些问题：有些指令写得不好，模型不能总是修改或扩展它们，而且它们常常使上下文窗口膨胀。另一方面，代码是自文档化的、可修改的，并且不需要始终存在于上下文中。

这是一个真实示例：我们不断看到 Claude 编写相同的脚本，用于将 Anthropic 样式应用于幻灯片。所以我们让 Claude 将其保存为自身的工具：

```
# anthropic/brand_styling/apply_template.py
import sys
from pptx import Presentation

if len(sys.argv) != 2:
    print("用法: apply_template.py <pptx>")
    sys.exit(1)

prs = Presentation(sys.argv[1])
for slide in prs.slides:
    ...
```

slide-decks.md 中相应的文档简单地引用了这个脚本：

```
## Anthropic 幻灯片
- 开场/结束幻灯片
  - 背景颜色: `#141413`
  - 前景颜色: oat
- 章节幻灯片:
  - 背景颜色: `#da7857`
  - 前景颜色: `#141413`

使用 `./apply_template.py` 脚本原地更新 pptx 文件。
```

## **技能生态系统**

技能生态系统迅速形成，到目前为止，我们看到了三种主要类型的技能正在被构建：

### **基础技能**

这些提供每个人都需要的核心能力：处理文档、电子表格、演示文稿等。它们编码了文档生成和操作的最佳实践。你可以通过探索我们[公共仓库中的基础技能](https://github.com/anthropics/skills/tree/main/skills/public)来了解实际应用情况。

### **合作伙伴技能**

随着技能标准化了智能体与专业能力交互的方式，公司正在构建技能，使其服务对智能体可访问。[K-Dense](https://github.com/K-Dense-AI/claude-scientific-skills)、[Browserbase](https://github.com/browserbase/agent-browse)、[Notion](https://www.notion.so/notiondevs/Notion-Skills-for-Claude-28da4445d27180c7af1df7d8615723d0) 和[许多其他公司](https://claude.com/blog/organization-skills-and-directory)正在创建直接集成其服务的技能，在保持技能格式简单性的同时，扩展 Claude 在特定领域的能力。

### **企业技能**

组织构建专有技能，编码其内部流程和领域专业知识。技能有助于捕捉使智能体对企业工作有用的特定工作流程、合规要求和机构知识。

## **我们看到的趋势**

随着技能采用的增长，一些模式正在浮现，指明了这一范式可能的发展方向。这些趋势塑造了我们对技能设计的思考方式，以及我们正在为支持技能开发者而构建的工具。

### **日益增长的复杂性**

早期的技能是简单的文档参考。现在我们看到复杂的多步骤工作流程，这些工作流程协调数据检索、复杂计算和跨多个工具的格式化输出。

* **简单**："状态报告编写器"（约 100 行）——模板和格式化
* **中级**："财务模型构建器"（约 800 行）——数据检索、使用 Python 的 Excel 建模
* **复杂**："RNA 测序流程"（2,500+ 行）——协调 HISAT2、StringTie、DESeq2 分析

### **技能与 MCP**

[技能和 MCP 服务器自然协同工作](https://claude.com/blog/extending-claude-capabilities-with-skills-mcp-servers)。一个竞争分析技能可能协调网络搜索、通过 MCP 连接内部数据库、Slack 消息历史和 Notion 页面，以综合生成一份全面的报告。

### **非开发者采用**

技能创建正在从工程师扩展到产品经理、分析师和跨学科领域的专家。他们可以使用技能创建工具在 30 分钟内创建并测试自己的第一个技能，该工具通过交互式方式引导他们完成整个过程。我们正在努力使技能创建更加便捷，通过改进的工具和模板，让任何人都能捕获和分享专业知识。

## **完整的架构**

综合来看，新兴的智能体架构看起来像是以下组件的组合：

1. **智能体循环**：决定下一步做什么的核心推理系统
2. **智能体运行时**：执行环境（代码、文件系统）
3. **MCP 服务器**：与外部工具和数据源的连接
4. **技能库**：领域专业知识和程序性知识

每一层都有明确的目的：循环负责推理，运行时负责执行，MCP 负责连接，技能负责指导。这种分离使系统易于理解，并允许每个部分独立演进。

考虑当你向这个架构添加一项技能时会发生什么。[前端设计技能](https://github.com/anthropics/claude-code/tree/main/plugins/frontend-design)瞬间改变了 Claude 的前端能力。它提供关于排版、色彩理论和动画的专业指导，仅在构建 Web 界面时激活。渐进式披露意味着它只在相关时加载。添加新功能变得简单直接。

## **将技能部署到新的垂直领域**

这种通用智能体配备 MCP 服务器和技能的新兴模式，已经在帮助我们向新的垂直领域部署 Claude。

### **金融服务**

在推出技能后不久，我们通过技能增强了 [Claude 在金融服务领域](https://www.anthropic.com/news/claude-for-financial-services)的能力，使 Claude 对金融专业人士更加有用：

* **DCF 模型构建器**：构建贴现现金流模型，包含正确的 WACC 计算和敏感性分析
* **可比公司分析**：生成包含相关倍数和基准测试的可比公司表
* **收益分析**：处理季度业绩并创建投资更新报告
* **首次覆盖**：构建包含财务模型的综合研究报告
* **尽职调查**：使用标准化框架构建并购分析
* **推介材料**：按照行业标准创建客户演示文稿

### **医疗保健与生命科学**

我们还通过技能增强了我们的[医疗保健和生命科学产品](https://www.anthropic.com/news/healthcare-life-sciences)，使 Claude 对研究人员、临床医生和医疗保健开发者更加有用：

* **生物信息学工具包**：用于 scVI-tools 和 Nextflow 部署的技能，对于管理基因组流程和单细胞 RNA 测序至关重要
* **临床试验方案生成**：加速临床研究的方案开发
* **科学问题选择**：帮助研究人员识别和构建有影响力的研究问题
* **FHIR 开发**：帮助开发者为健康数据互操作性编写更准确的代码，以更少的错误更快地连接医疗系统
* **事先授权审查**：通过交叉引用覆盖要求、临床指南和患者记录，减少行政负担并加速患者获得所需护理

## **标准化 Agent Skills**

为了实现这一愿景，我们将 [Agent Skills](https://agentskills.io) 作为开放标准发布。与 MCP 一样，我们相信技能应该跨工具和平台可移植。无论你使用的是 Claude 还是其他 AI 平台，同样的技能都应该有效。我们一直在与生态系统成员合作制定该标准，并对早期采用感到兴奋。

当有人第一次开始使用 AI 智能体时，它应该已经知道你和你团队关心什么，因为技能捕获并传递了这些专业知识。随着这个生态系统的发展，社区中其他人构建的技能可以使你的智能体更有用、更可靠、更有能力——无论他们使用哪个 AI 平台。

## **入门指南**

我们正在汇聚到一个通用智能体的架构上，而技能提供了一种交付和共享新能力的范式。真正的价值来自于我们共同构建的集体知识库：捕获专业知识、跨团队传递，并使每个智能体都比上一个更有能力。

**资源：**

* [不要构建智能体，转而构建技能](https://youtu.be/CEvIs9y1uog?si=yhYQH-ZTX0DfNdtm)（YouTube 视频）
* [技能文档](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
* [GitHub 仓库](https://github.com/anthropics/skills)
* [技能食谱](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction)
* [在 Claude 中使用技能](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
* [技能 API 快速入门](https://platform.claude.com/docs/en/build-with-claude/skills-guide)
* [技能最佳实践文档](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

### **致谢：**

Barry Zhang、Mahesh Murag、Keith Lazuka、Ryan Whitehead
