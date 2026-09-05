---
title: "Claude 成本可见性与控制指南 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-04"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/a-guide-to-cost-visibility-and-control-in-claude"
---

# Claude 成本可见性与控制指南 | Claude by Anthropic

---

企业以多种方式使用 Claude，从向数千名员工推广，到初创公司和单一团队在 Claude Platform 上构建应用程序。成本对所有这些用户都很重要。

在本文中，我们将解释 IT 管理员如何利用现有的控制功能来查看和管理 Claude 的成本，以及一些关于如何决定支出方向的实用建议。

### **思考成本的有用方式**

将 AI 的“每次成果成本”而非 token 消耗量作为价值的主要衡量指标会更有帮助。以下是关于项目需要思考的两个问题：

1. 如果没有 AI，这项工作需要多少成本，无论是资源、时间，还是根本不会尝试这个项目？
2. 模型是在完成一项困难且需要判断和推理的任务，还是仅仅因为工作量大——即大量简单直接的工作？

第一个问题的答案取决于你的业务和需求——没有供应商能替你衡量。第二个问题可以通过将模型与工作匹配来解决。将较便宜的模型用于复杂推理往往会让最终任务更昂贵，因为它会在重试上消耗 token，并且需要更多人工修正。将前沿模型用于基础文档处理，则是为任务从未使用的能力付费。

Claude 的[模型家族](https://claude.com/blog/claude-models-explained)为你提供了选择：

* **Fable** 用于最困难的问题；
* **Opus** 用于长期任务和编码；
* **Sonnet** 用于日常工作和分析；
* **Haiku** 用于高容量和常规任务。

对于以上任何模型，[effort 控制](https://platform.claude.com/docs/en/build-with-claude/effort)可以调节模型在解决问题时“思考”的程度，而[advisor 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool)允许较小的模型仅在遇到瓶颈时咨询前沿模型。

许多组织使用多种模型，通常在同一项目上。例如，一家保险公司可能会让前沿模型帮助理赔员评估复杂的商业索赔，而 Haiku 则负责对输入其中的文档进行标记和分类。

### **如何查看和控制你的支出**

你可使用的控制功能取决于 Claude 是作为面向员工的产品运行，还是作为应用程序背后的 API 运行。前者将控制权交给管理员，后者则交给在其上构建的工程师，而大多数大型客户两者都使用。

**Claude Enterprise 的成本控制**

我们通常建议按顺序处理这些事项，因为在看到一个月实际使用情况之前，很难设定合理的限制。

* [**访问控制**](https://support.claude.com/en/articles/13930452-manage-custom-roles-on-enterprise-plans)允许管理员确定可以使用 Claude Code 和 Claude Cowork 等产品的组和自定义角色，而不是一次性全部开放。从一个团队开始，观察结果，然后逐部门扩展。
* **模型控制**在两个层面运作。[权限](https://support.claude.com/en/articles/15694740-manage-model-access-for-your-organization)决定团队可以访问哪些模型，而[默认设置](https://support.claude.com/en/articles/15330088-set-a-default-model-for-your-organization)则设定新对话开始的模型。管理员可以为从事最困难工作的团队授予最强大模型的权限，并将其他所有人默认设置为 Sonnet。
* [**硬性支出上限**](https://support.claude.com/en/articles/11526368-how-am-i-billed-for-my-enterprise-plan#h_deb29b5a4f)对使用量设置上限。一旦你了解了整个组织、单个用户或某个组的基线，就可以设置这些上限——对于组而言，每个成员都会获得该限制。上限会立即生效。

管理员还可以自动审查支出上限增加请求，识别接近支出上限的成员，并发现使用量快速变化的成员。

**观察 Claude 使用情况的工具**

使用数据可以在管理仪表板中查看、发送到你的系统，或直接向 Claude 询问。以下是 IT 管理员可以用来更好地了解其组织 Claude 使用情况的三个功能：

* [**使用分析**](https://support.claude.com/en/articles/12883420-view-usage-analytics-for-team-and-enterprise-plans)按人员、团队和模型细分支出。数据导出与账单高度匹配，以便你更好地将使用情况与账单进行核对。
* [**Analytics API**](https://platform.claude.com/docs/en/manage-claude/analytics-api)将相同的数据提供给团队已在使用的系统。将其连接到商业智能工具、财务系统和内部仪表板，以便将 Claude 支出与预算和预测等其他成本一起评估。
* [**通过 analytics chat 进行分析**](https://support.claude.com/en/articles/14729354-use-analytics-chat-to-ask-claude-about-usage)允许管理员用自然语言询问使用情况。直接问“本月支出最高的是谁？”或“本季度哪个团队的使用量增长最快？”，无需拉取完整报告。

### **在 API 上构建的控制功能**

Claude Console 为在 Claude Platform 上构建的组织和开发者提供控制功能。Workspaces 按产品、团队或环境分离 API 使用情况，并在你的成本和用量报告中拥有独立的条目。

Claude Platform 上有用的成本杠杆包括：

* [**Prompt 缓存**](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)存储跨请求重复使用的内容，这样模型就不必每次都重新处理。如果你每次调用都发送相同的参考资料，请启用此功能，缓存命中时成本可降至正常输入费率的 10%。
* [**批量处理**](https://platform.claude.com/docs/en/build-with-claude/batch-processing)以半价运行不需要即时响应的作业，例如电子商务公司夜间对目录进行分类。将任何可以等待的内容转移过来；批量折扣可与缓存叠加。
* [**effort 参数**](https://platform.claude.com/docs/en/build-with-claude/effort)控制模型在给定调用中执行多少推理。在路由和提取时调低它，但在最终推荐时调高它，这样你只为需要峰值费率的调用付费。
* [**advisor 策略**](https://platform.claude.com/docs/en/build-with-claude/effort)让 Sonnet 等较小模型在关键时刻调用前沿模型，例如在交付前评估工作。在较小模型上运行大部分任务，仅在需要其判断力时支付更大模型的费用。

结合使用这些功能，通常可以在任何人触及预算线之前大幅降低生产工作负载的成本。

### **开始使用**

成本控制功能现已可在 Claude Enterprise 中使用。要查看方案和定价，请访问 [claude.com/pricing](https://claude.com/pricing)。企业组织可以通过 [Claude Enterprise](https://support.claude.com/en/articles/9797531-what-is-the-enterprise-plan) 产品[直接开始使用](http://claude.ai/create/enterprise)。开发者可以在 [docs.claude.com](https://docs.claude.com) 找到 Workspaces、缓存和批处理的文档。
