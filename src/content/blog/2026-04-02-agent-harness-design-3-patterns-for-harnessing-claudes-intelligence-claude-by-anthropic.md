---
title: "智能体框架设计：利用Claude智能的三种模式 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-02"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/harnessing-claudes-intelligence"
---

# 智能体框架设计：利用Claude智能的三种模式 | Claude by Anthropic

---

Anthropic 联合创始人之一 Chris Olah [说](https://www.darioamodei.com/post/the-urgency-of-interpretability)，像 Claude 这样的生成式 AI 系统更多是被培养出来的，而非被构建出来的。研究人员设定了引导成长的条件，但最终出现的具体结构或能力并不总是可预测的。

这为用 Claude 构建应用带来了挑战：[智能体框架编码了对 Claude 无法独立完成的任务的假设](https://www.anthropic.com/engineering/harness-design-long-running-apps)，但随着 Claude 能力越来越强，这些假设会变得过时。

智能体框架是模型周围的软件脚手架：包括循环、工具、上下文管理和护栏，它们将原始智能转化为可工作的智能体。[智能体框架设计](https://claude.com/blog/harnessing-claudes-intelligence)是决定该脚手架中应包含什么，以及随着模型改进，哪些可以移除的实践。

在本文中，我们分享了团队在构建能够跟上 Claude 不断进化的智能、同时平衡延迟和成本的应用时应使用的三种模式：利用它已知的内容，询问你可以停止做什么，以及谨慎地通过智能体框架设定边界。

### **1. 依赖模型，而非框架：使用 Claude 已知的内容**

我们建议使用 Claude 能很好理解的工具来构建应用。

2024 年底，Claude 3.5 Sonnet 在 SWE-bench Verified 上达到了 49%——当时的[最先进水平](https://www.anthropic.com/engineering/swe-bench-sonnet）——仅使用了一个 [bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 和一个 [文本编辑器工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool) 来查看、创建和编辑文件。Claude Code 也基于这些同样的工具。[Bash](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 并非为构建智能体而设计，但它是 Claude *知道* 如何使用的工具，并且随着时间的推移，它使用得越来越好。

*SWE-bench Verified 基准测试上不同 Claude 模型版本的成绩，突出了其进化过程。*

我们看到 Claude 将这些通用工具组合成解决不同问题的模式。例如，[Agent Skills](https://agentskills.io/home)、[程序化工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) 和 [记忆工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool) 都是基于 bash 和文本编辑器工具构建的。

*程序化工具调用、技能和记忆是我们 bash 和文本编辑器工具的组合。*

### **2. 精简你的智能体框架：询问你可以停止做什么**

[智能体框架编码了对 Claude 无法独立完成的任务的假设](https://www.anthropic.com/engineering/harness-design-long-running-apps)。随着 Claude 越来越强大，这些假设应该被检验。

**让 Claude 自己编排动作**

一个常见的假设是，每个工具结果都应该流回 Claude 的[上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows)以指导下一步行动。将工具结果作为 token 处理可能会缓慢且昂贵，如果它只需要传递给下一个工具，或者 Claude 只关心输出的一小部分，那么这样做是不必要的。

*Claude 调用工具，这些工具在环境中执行。*

考虑读取一个大表格以推理某一列：整个表格落入上下文，Claude 为每个不需要的行支付 token 成本。可以在工具设计中处理这个问题，使用[硬编码过滤器](https://platform.claude.com/docs/en/about-claude/models/migration-guide#additional-recommended-changes)。但这并不能解决智能体框架正在做出一个 *编排决策* 的问题，而这个决策由 Claude 来做更合适。

给 Claude 一个[代码执行](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)工具（例如，[bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 或 [特定语言 REPL](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)）解决了这个问题：它允许 Claude 编写代码来表达工具调用以及它们之间的逻辑。不是由框架决定每个工具调用结果都作为 token 处理，而是由 Claude 决定哪些结果需要传递、过滤或输入到下一个调用，而不接触上下文窗口。只有代码执行的输出到达 Claude 的上下文窗口。

*Claude 可以编写代码来表达工具调用及其之间的逻辑。*

编排决策从框架转移到模型。由于代码是 Claude 编排动作的通用方式，一个强大的编码模型也是一个强大的*通用*智能体。Claude 在[非编码评估](https://claude.com/blog/improved-web-search-with-dynamic-filtering)中使用这种模式表现出色：在 BrowseComp 上，这是一个测试智能体浏览网页能力的[基准测试](https://arxiv.org/abs/2504.12516)，让 Opus 4.6 能够过滤自己的工具输出，准确率从 45.3% 提升到 61.6%。

**让 Claude 管理自己的上下文**

任务特定的上下文指导 Claude 如何使用 bash 和文本编辑器工具等通用工具。一个常见的假设是，[系统提示词](https://platform.claude.com/docs/en/release-notes/system-prompts)应该手工编写，包含任务特定的指令。问题在于，预加载带有指令的提示词无法扩展到许多任务：每增加一个 token 都会消耗 [Claude 的注意力预算](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)，并且预加载很少使用的指令是一种浪费。

让 Claude 能够访问 [skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) 解决了这个问题：每个技能的 YAML 前置内容是一个短描述，预加载到上下文窗口中，提供技能内容的概览。如果任务需要，Claude 可以调用读取文件工具逐步披露完整的技能。

*Claude 可以使用技能逐步披露与任务相关的上下文。*

技能让 Claude 自由地组装自己的上下文窗口，而[上下文编辑](https://platform.claude.com/docs/en/build-with-claude/context-editing)则相反，它提供了一种选择性移除已过时或不相关上下文的方法，例如旧的工具结果或思考块。

通过[子智能体](https://code.claude.com/docs/en/sub-agents)，Claude 越来越擅长知道何时分叉到一个新的上下文窗口以隔离特定任务的工作。[对于 Opus 4.6](https://www-cdn.anthropic.com/0dd865075ad3132672ee0ab40b05a53f14cf5288.pdf)，生成子智能体的能力使 BrowseComp 的结果比最佳单智能体运行提高了 2.8%。

**让 Claude 持久化自己的上下文**

长时间运行的智能体可能超过单个[上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows)的限制。一个常见的假设是，记忆系统应该依赖模型周围的检索基础设施。我们的很多工作都集中在给 Claude 提供简单的*自主选择*持久化哪些内容的方式。

例如，[压缩](https://platform.claude.com/docs/en/build-with-claude/compaction)让 Claude 总结其过去的上下文，以在长期任务中保持连续性。在几个版本中，Claude 在选择记住什么方面变得更好了。例如，在 [BrowseComp](https://www-cdn.anthropic.com/14e4fb01875d2a69f646fa5e574dea2b1c0ff7b5.pdf) 上，这是一个智能搜索任务，Sonnet 4.5 无论我们给它多少压缩预算都稳定在 43%。然而，Opus 4.5 在相同设置下扩展到了 68%，而 Opus 4.6 达到了 84%。

[记忆文件夹](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)是另一种方法，允许 Claude 将上下文写入文件，然后在需要时读取它们。我们看到 Claude 将其用于智能搜索。在 BrowseComp-Plus 上，给 Sonnet 4.5 一个记忆文件夹[将准确率从 60.4% 提升到 67.2%](https://www-cdn.anthropic.com/bf10f64990cfda0ba858290be7b8cc6317685f47.pdf)。

*Claude 可以将上下文持久化到记忆文件夹。*

[长期游戏](https://www.youtube.com/watch?v=CXhYDOvgpuU)，例如宝可梦，是 Claude 使用记忆文件夹能力提升的一个例子。Sonnet 3.5 把记忆当作转录，写下非玩家角色（NPC）说了什么，而不是什么重要。在 14,000 步后，它有 31 个文件——包括两个关于毛毛虫宝可梦的几乎重复的文件——并且还在第二个城镇：

```
caterpie_weedle_info:
- 绿毛虫和独角虫都是毛毛虫宝可梦。
- 绿毛虫是一种没有毒的毛毛虫宝可梦。
- 独角虫是一种有毒的毛毛虫宝可梦。
- 这些信息对于未来的遭遇和战斗至关重要。
- 如果我们的宝可梦中毒了，我们应该尽快去宝可梦中心治疗。
```

后来的模型则写了战术笔记。Opus 4.6 在相同步数下，有 10 个文件，组织成目录，三个道馆徽章，以及一个从自身失败中提炼的学习文件：

```
/gameplay/learnings.md:
- 喇叭芽睡眠+缠绕 combo：在睡眠粉落地前用咬住快速击杀。不要让它建立优势！
- 第一世代背包限制：最多 20 个物品。在进入地牢前丢弃不需要的技能机。
- 旋转瓷砖迷宫：不同的进入 y 位置会导致不同的目的地。尝试所有入口并连锁通过多个口袋。
- B1F y=16 墙壁确认在所有 x=9-28 处是实心的（步骤 14557）
```

### **3. 在框架设计中谨慎设定边界**

智能体框架围绕 Claude 提供结构，以强制执行用户体验、成本或安全。

**设计上下文以最大化缓存命中率**

[Messages API](https://platform.claude.com/docs/en/build-with-claude/working-with-messages) 是无状态的。Claude 无法看到之前轮次的对话历史。这意味着智能体框架需要在每次轮次中将新上下文与所有过去的动作、工具描述和指令打包在一起。

提示词可以基于设定的[断点](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)进行缓存。换句话说，Claude API 将断点之前的上下文写入缓存，并检查上下文是否匹配任何先前的缓存条目。

由于缓存 token [成本为基础输入 token 的 10%](https://platform.claude.com/docs/en/about-claude/pricing)，智能体框架中的以下几条原则有助于最大化缓存命中率：

| 原则 | 说明 |
| --- | --- |
| 静态在前，动态在后 | 对请求排序，使稳定内容（系统提示词、工具）排在前面。 |
| 使用消息进行更新 | 在消息中追加 `<system-reminder>`，而不是编辑提示词。 |
| 不要更换模型 | 避免在会话期间切换模型。缓存是模型特定的；切换会破坏缓存。如果需要更便宜的模型，请使用子智能体。 |
| 谨慎管理工具 | 工具位于缓存前缀中。添加或移除一个工具会使缓存失效。对于动态发现，请使用**工具搜索**，它会在不破坏缓存的情况下追加。 |
| 更新断点 | 对于多轮次应用（例如智能体），将断点移动到最新消息以保持缓存最新。为此使用**自动缓存**。 |

**使用声明式工具处理用户体验、可观测性或安全边界**

Claude 不一定知道应用的安全边界或用户体验表面。Claude 发出工具调用，由框架处理。bash 工具为 Claude 提供了广泛的程序化杠杆来执行操作，但它只给框架一个命令字符串——每个操作都是相同的形状。将操作提升为专用工具会给框架一个特定于操作、带有类型化参数的钩子，它可以拦截、门控、渲染或审计。

需要安全边界的操作是专用工具的天然候选。可逆性通常是一个好的标准，难以撤销的操作（如外部 API 调用）可以通过用户确认来门控。像 `edit` 这样的写入工具可以包含过期检查，这样 Claude 就不会覆盖自从上次读取后已更改的文件。

*专用工具可以用于基于安全、用户体验或可观测性考虑的操作。*

当操作需要呈现给用户时，工具也很有用。例如，它们可以渲染为模态框，以清晰地向用户显示问题，给用户提供多个选项，或者阻塞智能体循环直到用户提供反馈。

最后，工具对于可观测性很有用。当操作是类型化工具时，框架会获得结构化的参数，可以记录、追踪和重放。

是否将操作提升为工具的决定应该不断重新评估。例如，Claude Code 的[自动模式](https://www.anthropic.com/engineering/claude-code-auto-mode)（在本文发布时处于研究模式）为 bash 工具提供了一个安全边界：它让第二个 Claude 读取命令字符串并判断是否安全。这种模式可以*限制*对专用工具的需求，并且应该只用于用户信任大致方向的任务。对于某些高风险操作，专用工具仍然有其存在的价值。

### 智能体框架设计的未来

Claude 智能的前沿始终在变化。关于 Claude 不能做什么的假设需要随着其能力的每一步变化而重新检验。

我们看到了这种模式的重复。在我们为长期任务构建的[智能体](https://www.anthropic.com/engineering/harness-design-long-running-apps)中，Sonnet 4.5 会在感知到上下文限制接近时过早结束。我们添加了重置来清除上下文窗口，以解决这种“上下文焦虑”。到了 Opus 4.5，这种行为消失了。我们为补偿而构建的上下文重置变成了智能体框架中的死重。

移除这种死重很重要，[因为它可能成为 Claude 性能的瓶颈](http://www.incompleteideas.net/IncIdeas/BitterLesson.html)。随着时间的推移，我们应用中的结构或边界应该根据以下问题来修剪：*我能停止做什么？*

*要使用这里讨论的所有工具和模式，请查看* [*我们的 claude-api skill*](https://github.com/anthropics/skills/tree/main/skills/claude-api)*。*

### 致谢

本文由 Claude 平台团队技术成员 Lance Martin 撰写。特别感谢 Thariq Shihipar、Barry Zhang、Mike Lambert、David Hershey 和 Daliang Li 对所涉主题的有益讨论。感谢 Lydia Hallie、Lexi Ross、Katelyn Lesse、Andy Schumeister、Rebecca Hiscott、Jake Eaton、Pedram Navid 和 Molly Vorwerck 的编辑审阅和反馈。
