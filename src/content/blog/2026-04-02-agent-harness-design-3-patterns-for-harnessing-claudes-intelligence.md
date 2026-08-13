---
title: "代理框架设计：驾驭克劳德智能的三种模式"
description: "Anthropic 联合创始人之一 Chris Olah [表示](https://www.darioamodei.com/post/the-urgency-of-interpretability)，像 Claude 这样的生成式 AI 系统更多是被培育出来的，而非构建出来的。研究人员设定条件来引导其发展，但最终涌现出的具体结构或能力并不总是可预测的。"
pubDate: "2026-04-02"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/harnessing-claudes-intelligence"
---
Anthropic 联合创始人之一 Chris Olah [表示](https://www.darioamodei.com/post/the-urgency-of-interpretability)，像 Claude 这样的生成式 AI 系统更多是被培育出来的，而非构建出来的。研究人员设定条件来引导其发展，但最终涌现出的具体结构或能力并不总是可预测的。

这给基于 Claude 构建应用带来了挑战：[代理框架编码了假设](https://www.anthropic.com/engineering/harness-design-long-running-apps)，这些假设涉及 Claude 无法独立完成的事情，但随着 Claude 能力的增强，这些假设会变得过时。

代理框架是围绕模型构建的软件脚手架：包括循环、工具、上下文管理和护栏，它们将原始智能转化为可工作的代理。[代理框架设计](https://claude.com/blog/harnessing-claudes-intelligence)是一种实践，决定哪些内容属于该脚手架，以及随着模型改进，哪些内容可以移除。

在本文中，我们分享了团队在构建能够跟上 Claude 不断发展的智能水平，同时平衡延迟和成本的应用时应采用的三种模式：利用它已知的内容，询问可以停止做什么，以及在代理框架中谨慎设定边界。

### **1. 依赖模型而非框架：利用 Claude 已知的内容**

我们建议使用 Claude 能够很好理解的工具来构建应用程序。

2024 年底，Claude 3.5 Sonnet 在 SWE-bench Verified 上达到了 49%——当时是[最先进的水平](https://www.anthropic.com/engineering/swe-bench-sonnet)——仅使用了一个 [bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 和一个用于查看、创建和编辑文件的[文本编辑器工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/text-editor-tool)。Claude Code 也基于这些相同的工具。[Bash](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 并非为构建代理而设计，但它是 Claude *知道*如何使用，并且会随着时间推移而变得更擅长的工具。

*SWE-bench Verified 基准测试中不同 Claude 模型版本的得分突显了其演变过程。*

我们观察到 Claude 将这些通用工具组合成解决不同问题的模式。例如，[Agent Skills](https://agentskills.io/home)、[程序化工具调用](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) 和[记忆工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool) 都是基于 bash 和文本编辑器工具构建的。

*程序化工具调用、技能和记忆是我们 bash 和文本编辑器工具的组合。*

### **2. 精简你的代理框架：询问可以停止做什么**

[代理框架编码了假设](https://www.anthropic.com/engineering/harness-design-long-running-apps)，这些假设涉及 Claude 无法独立完成的事情。随着 Claude 能力的增强，这些假设应该被检验。

**让 Claude 编排自己的行动**

一个常见的假设是，每个工具的结果都应该流回 Claude 的[上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows)，以告知下一步行动。如果结果只需要传递给下一个工具，或者 Claude 只关心输出的一小部分，那么将工具结果作为 token 处理可能会缓慢、昂贵且不必要。

*Claude 调用工具，这些工具在环境中执行。*

考虑读取一个大表格以推理单个列：整个表格进入上下文，Claude 为它不需要的每一行支付 token 成本。可以在工具设计中解决这个问题，使用[硬编码过滤器](https://platform.claude.com/docs/en/about-claude/models/migration-guide#additional-recommended-changes)。但这并没有解决代理框架正在做出一个 Claude 更适合做出的*编排决策*这一事实。

给 Claude 一个[代码执行](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)工具（例如，[bash 工具](https://platform.claude.com/docs/en/agents-and-tools/tool-use/bash-tool) 或[特定语言的 REPL](https://platform.claude.com/docs/en/agents-and-tools/tool-use/code-execution-tool)）解决了这个问题：它允许 Claude 编写代码来表达工具调用及它们之间的逻辑。框架不再决定每个工具调用的结果都被作为 token 处理，而是由 Claude 决定哪些结果需要传递、过滤或通过管道输入到下一个调用，而无需触及上下文窗口。只有代码执行的输出才会到达 Claude 的上下文窗口。

*Claude 可以编写代码来表达工具调用及它们之间的逻辑。*

编排决策从框架转移到了模型。由于代码是 Claude 编排行动的一种通用方式，一个强大的编码模型也是一个强大的*通用*代理。Claude 在使用这种模式的[非编码评估](https://claude.com/blog/improved-web-search-with-dynamic-filtering)中表现出色：在 BrowseComp（一个[基准测试](https://arxiv.org/abs/2504.12516)，测试代理浏览网页的能力）上，让 Opus 4.6 能够过滤自己的工具输出，将准确率从 45.3% 提升到了 61.6%。

**让 Claude 管理自己的上下文**

特定任务的上下文引导 Claude 使用 bash 和文本编辑器工具等通用工具。一个常见的假设是，[系统提示](https://platform.claude.com/docs/en/release-notes/system-prompts)应该手动制作，包含特定任务的指令。问题在于，预加载带有指令的提示无法跨多个任务扩展：每增加一个 token 都会消耗 [Claude 的注意力预算](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)，并且预加载很少使用的指令是浪费的。

让 Claude 能够访问[技能](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)解决了这个问题：每个技能的 YAML 前置元数据是一段预加载到上下文窗口中的简短描述，提供了技能内容的概览。如果任务需要，Claude 可以通过调用读取文件工具来逐步披露完整的技能内容。

*Claude 可以使用技能逐步披露与任务相关的上下文。*

虽然技能让 Claude 可以自由地组装自己的上下文窗口，但[上下文编辑](https://platform.claude.com/docs/en/build-with-claude/context-editing)则是相反的，它提供了一种选择性移除已过时或不相关上下文的方法，例如旧的工具结果或思考块。

通过[子代理](https://code.claude.com/docs/en/sub-agents)，Claude 在知道何时分叉到一个新的上下文窗口以隔离特定任务的工作方面变得越来越好。[使用 Opus 4.6](https://www-cdn.anthropic.com/0dd865075ad3132672ee0ab40b05a53f14cf5288.pdf)，生成子代理的能力使 BrowseComp 的结果比最佳单代理运行提高了 2.8%。

**让 Claude 持久化自己的上下文**

长时间运行的代理可能会超过单个[上下文窗口](https://platform.claude.com/docs/en/build-with-claude/context-windows)的限制。一个常见的假设是，记忆系统应该依赖于模型周围的检索基础设施。我们的许多工作都集中在给 Claude 简单的方法来*自行选择*要持久化的内容。

例如，[压缩](https://platform.claude.com/docs/en/build-with-claude/compaction)让 Claude 总结其过去的上下文，以便在长周期任务中保持连续性。经过几次版本发布，Claude 在选择要记住的内容方面变得更好了。例如，在 [BrowseComp](https://www-cdn.anthropic.com/14e4fb01875d2a69f646fa5e574dea2b1c0ff7b5.pdf)（一个代理搜索任务）上，无论我们给 Sonnet 4.5 多少压缩预算，其表现都持平在 43%。然而，Opus 4.5 在相同设置下扩展到了 68%，而 Opus 4.6 达到了 84%。

一个[记忆文件夹](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool)是另一种方法，允许 Claude 将上下文写入文件，并在需要时稍后读取。我们观察到 Claude 将其用于代理搜索。在 BrowseComp-Plus 上，给 Sonnet 4.5 一个记忆文件夹[将准确率从 60.4% 提升到了 67.2%](https://www-cdn.anthropic.com/bf10f64990cfda0ba858290be7b8cc6317685f47.pdf)。

*Claude 可以将上下文持久化到记忆文件夹。*

[长周期游戏](https://www.youtube.com/watch?v=CXhYDOvgpuU)，例如 Pokémon，是 Claude 改进使用记忆文件夹能力的一个例子。Sonnet 3.5 将记忆视为转录本，记下非玩家角色（NPC）说了什么，而不是什么重要。经过 14,000 步后，它有 31 个文件——包括两个关于毛毛虫宝可梦的近乎重复的文件——并且仍然在第二个城镇：

```
caterpie_weedle_info:
- Caterpie 和 Weedle 都是毛毛虫宝可梦。
- Caterpie 是一种没有毒的毛毛虫宝可梦。
- Weedle 是一种有毒的毛毛虫宝可梦。
- 这些信息对于未来的遭遇和战斗至关重要。
- 如果我们的宝可梦中毒，我们应该尽快在宝可梦中心寻求治疗。
```

后来的模型写了战术笔记。Opus 4.6，在相同的步数下，有 10 个文件组织成目录，三个道馆徽章，以及一个从自身失败中提炼出的学习文件：

```
/gameplay/learnings.md:
- Bellsprout 睡眠+缠绕组合：在睡眠粉落地前用 BITE 快速 KO。不要让它设局！
- 第一代背包限制：最多 20 个物品。在进入地牢前丢弃不需要的 TM。
- 旋转瓷砖迷宫：不同的入口 y 位置会导致不同的目的地。尝试所有入口并连锁穿越多个口袋。
- B1F y=16 墙壁在所有 x=9-28 处确认是实心的（步骤 14557）
```

### **3. 在框架设计中谨慎设定边界**

代理框架为 Claude 提供结构，以强制执行用户体验、成本或安全性。

**设计上下文以最大化缓存命中率**

[Messages API](https://platform.claude.com/docs/en/build-with-claude/working-with-messages) 是无状态的。Claude 无法看到先前轮次的对话历史。这意味着代理框架需要在每一轮为 Claude 打包新的上下文以及所有过去的行动、工具描述和指令。

提示可以基于设定的[断点](https://platform.claude.com/docs/en/build-with-claude/prompt-caching)进行缓存。换句话说，Claude API 将上下文写入缓存直到一个断点，并检查该上下文是否与任何先前的缓存条目匹配。

由于缓存的 token [成本是基础输入 token 的 10%](https://platform.claude.com/docs/en/about-claude/pricing)，以下是代理框架中帮助最大化缓存命中率的一些原则：

| 原则 | 描述 |
| --- | --- |
| 静态在前，动态在后 | 对请求进行排序，使稳定内容（系统提示、工具）放在前面。 |
| 使用消息进行更新 | 在消息中附加 `<system-reminder>`，而不是编辑提示。 |
| 不要更换模型 | 避免在会话期间切换模型。缓存是特定于模型的；切换会破坏缓存。如果你需要更便宜的模型，请使用子代理。 |
| 谨慎管理工具 | 工具位于缓存前缀中。添加或删除一个会使缓存失效。对于动态发现，使用**工具搜索**，它会附加而不破坏缓存。 |
| 更新断点 | 对于多轮应用（例如代理），将断点移动到最新消息以保持缓存最新。使用**自动缓存**来实现这一点。 |

**使用声明式工具来实现用户体验、可观测性或安全边界**

Claude 不一定知道应用的安全边界或用户体验界面。Claude 发出工具调用，由框架处理。bash 工具给 Claude 提供了广泛的程序化杠杆来执行操作，但它只给框架一个命令字符串——每个操作都是相同的形状。将操作提升为专用工具，为框架提供了一个特定于操作的钩子，带有类型化参数，可以拦截、门控、渲染或审计。

需要安全边界的操作是专用工具的自然候选。可逆性通常是一个好的标准，而难以逆转的操作，例如外部 API 调用，可以通过用户确认来门控。像 `edit` 这样的写入工具可以包含一个陈旧性检查，这样 Claude 就不会覆盖自上次读取以来已更改的文件。

*专用工具可用于基于安全性、用户体验或可观测性考虑的操作。*

当某个操作需要呈现给用户时，工具也很有用。例如，它们可以渲染为一个模态框，以清晰地向用户显示问题，给用户多个选项，或者阻塞代理循环直到用户提供反馈。

最后，工具对于可观测性很有用。当操作是一个类型化工具时，框架会获得结构化的参数，可以记录、追踪和重放。

将操作提升为工具的决定应该持续重新评估。例如，Claude Code 的[自动模式](https://www.anthropic.com/engineering/claude-code-auto-mode)（在发布时处于研究模式）为 bash 工具提供了一个安全边界：它让第二个 Claude 读取命令字符串并判断其是否安全。这种模式可以*限制*对专用工具的需求，并且只应用于用户信任总体方向的任务。对于某些高风险操作，专用工具仍然可以发挥其作用。

### 代理框架设计的未来

Claude 智能的前沿总是在变化。关于 Claude 不能做什么的假设需要随着其能力的每一步变化而重新检验。

我们看到这种模式反复出现。在我们[为长周期任务构建的代理](https://www.anthropic.com/engineering/harness-design-long-running-apps)中，Sonnet 4.5 会因感知到上下文限制即将到来而过早结束。我们添加了重置来清除上下文窗口，以解决这种"上下文焦虑"。到了 Opus 4.5，这种行为消失了。我们为补偿而构建的上下文重置变成了代理框架中的死重。

移除这种死重很重要，[因为它可能成为瓶颈](http://www.incompleteideas.net/IncIdeas/BitterLesson.html)，限制 Claude 的性能。随着时间的推移，我们应用中的结构或边界应该基于以下问题被修剪：*我可以停止做什么？*

*要使用这里讨论的所有工具和模式，请查看* [*我们的 claude-api 技能*](https://github.com/anthropics/skills/tree/main/skills/claude-api)*。*

### 致谢

由 Claude 平台团队技术成员 Lance Martin 撰写。特别感谢 Thariq Shihipar、Barry Zhang、Mike Lambert、David Hershey 和 Daliang Li 对所涵盖主题的有益讨论。感谢 Lydia Hallie、Lexi Ross、Katelyn Lesse、Andy Schumeister、Rebecca Hiscott、Jake Eaton、Pedram Navid 和 Molly Vorwerck 的编辑审查和反馈。
