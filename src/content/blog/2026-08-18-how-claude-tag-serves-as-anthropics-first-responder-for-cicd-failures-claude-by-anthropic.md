---
title: "Claude Tag 如何充当 Anthropic CI/CD 故障的第一响应者 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-18"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/ai-ci-cd-on-call"
---

# Claude Tag 如何充当 Anthropic CI/CD 故障的第一响应者 | Claude by Anthropic

---

[*使用我们的设置套件搭建你自己的 Claude 值班系统*](https://github.com/anthropics/oncall-kit)*.*

## AI 事件响应助力 CI/CD：Claude 在 Anthropic 值班

几周前，我正值值班，同事在晚上 10 点通过 Slack 给我发消息：一个新服务上大约 44 个测试没有触发。

在过去，我会放下手头的事，坐到笔记本电脑前，疲惫地叹口气，然后开始长达一小时的排查修复流程。但现在，我的工作流程完全不同了：我把 @Claude 拉进来，问它看到了什么。

在这个案例中，Claude 发现这些测试是在当天早上某个功能开关被打开后消失的，并且还确认了回滚是安全的。我请同事回滚了该开关。3 分钟后，Claude 在 Slack 上通知我，确认跳过规则确实已被移除，错误率也已恢复到基线水平。

为清晰起见，根据真实对话重新演绎。

在过去的几个月里，Claude Tag 一直是 Anthropic CI/CD 故障的值班第一响应者。这不仅改善了我们的社交生活，还让每一个 CI 事件都拥有了即时的第一响应者：Claude 撰写了近期每一个有事件报告的事故的首份情况报告，**通常在 15 分钟内发布其首次分析。**

在本文中，我们将介绍我们构建了什么以及它是如何工作的，以便你可以自己搭建，不再惧怕轮到你值班。

## **我们的 Claude 值班设置**

在深入事件响应流程的每个阶段之前，我先在这里提供我们设置的整体概览，这样你在了解细节时能有一个全局视角。

一个值班代理需要**记忆**，以便记住已完成的工作；需要**连接和访问权限**，以便进行调查、理解和采取行动；需要**日程安排**，以便知道何时恢复工作；还需要**指令**，以便知道该做什么。

[Claude Tag](https://claude.com/product/tag) 是我们值班代理的骨干。Claude Tag 在我们的值班 Slack 频道中保存跨会话记忆，并提供事件期间逐轮指令的界面。Claude 还会实时响应值班频道及其他频道中的事件。例行任务的调度，即 Claude 定期执行的操作，也在这个频道上通过自然语言提示完成，例如“每周一上午 9:00（美国东部时间）运行 CI 交接”。

[Claude Tag 拥有自己的服务账户](https://claude.com/blog/agent-identity-access-model)，并拥有 Anthropic CI 工程师所需的工具访问权限，例如 Datadog 或 Grafana。这是由管理员为频道一次性设置的（[方法如下](https://claude.com/docs/claude-tag/admins/setup-overview#choose-which-tools-to-connect)）。

除了值班频道之外，我们还设置了 Claude 来监视其他相关频道（这些频道也添加了 Claude Tag 作为成员），以便获取额外上下文，如服务告警、配置更改或 PR 更新。

常驻指令以技能的形式存放在 Markdown 文件中，并提交到 GitHub 仓库中。这样多个团队成员可以共同迭代这些指令，我们可以像管理代码一样管理更改。其中还包含关键信息，如路由指令、策略以及作为自我改进循环一部分的经验教训日志。

这个设置花了我们几个小时，而不是几天。我们在 GitHub 上创建了一个通用的[值班设置套件](https://github.com/anthropics/oncall-kit)，可以帮助你开始构建类似的代理。它会将团队自身的事件历史转化为分诊剧本，并在你的事件频道中留下一个只读的 Claude，用于诊断、升级和学习。[你可以在大约十分钟内观看它针对一个虚构团队的历史运行](https://github.com/anthropics/oncall-kit/blob/main/test-fixtures/RUNBOOK.md)。

用 TL;DR 的方式总结步骤如下：

* 你需要一个 [Claude Team 或 Claude Enterprise](https://support.claude.com/en/collections/9387370-team-and-enterprise-plans) 套餐
* 组织所有者需要通过 Claude Tag 将 Claude 添加到值班 Slack 频道
* 组织所有者还需要帮助将值班 Slack 频道中的 Claude 连接到相应的连接器、GitHub 仓库，并设置 [Claude Code Remote](https://code.claude.com/docs/en/remote-control)。
* 将 Claude 添加到你的事件频道，并指示其监视事件并立即进行分诊

现在，让我们深入了解事件每个阶段中这一转变的具体细节。

## **检测**

Claude 不仅改变了你响应事件的方式，还改变了你首先发现事件的方式。此前，事件检测存在两大主要失败模式。

人类很难始终具有远见来设置完美的规则和完美的阈值。当没有足够的数据来分析流量模式时，这尤其困难。

为了解决这个问题，我们让 Claude 在新服务上线后的头几天分析数据和传入的告警，以建议额外的规则，并微调那些过于宽泛或过于狭窄的规则。

事件检测的第二大失败模式是告警疲劳：检查和验证每一个触发的告警非常繁琐。然而，Claude 不会像人类那样感到疲劳。

Claude 监视每个告警频道中的所有相关告警，并根据 [root oncall.md 文件](https://github.com/anthropics/oncall-kit/blob/main/templates/ONCALL.md)中的标准来判断是否可以等到早上再处理，还是需要立即呼叫值班人员。例如，在通过数据分析进行调优后，文件中的一条规则可能是：“如果错误率超过 2% 且持续超过 5 分钟，并且不在已知的部署窗口内，则呼叫值班人员；否则将其记录到 lessons.md 中。”

Claude 值班告警流程还有另外两种触发方式：

* CI 团队成员可以在值班频道中报告问题，就像开头提到的 44 个缺失测试的例子那样；或者
* 公司中的任何人都可以通过内部页面开启一个事件。如果该事件被标记为 CI 基础设施事件，则会为该事件创建一个 Slack 频道，我们的值班 Claude 会接手处理。

这里的关键要点是：告警流程是确定性的，而值班升级则同时具有确定性和代理式路径。

## **分诊**

让 Claude 过滤告警噪音是一回事，但真正的节省来自于调查环节。Claude 在事件开启后中位时间 14 分钟内发布其首份基于证据的分析，而在最快的案例中，它在首份报告中 4 分钟内就指出了根本原因。

当告警升级为事件时，Claude 通常已经在我们的 Slack 频道中准备好了一个基于证据的假设供我们审查。Claude Tag 会启动一个[动态工作流](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)，其中包含一个编排代理，它会启动执行子代理来调查每个依赖项和数据源。

对我们来说，这些数据源包括 Grafana、我们的日志存储、PagerDuty、GitHub、Kubernetes 和 Slack 事件频道——全部通过 [MCP 连接器](https://code.claude.com/docs/en/mcp)接入。Claude 可以并行追踪多条线索，有助于缩短 MTTR（平均修复时间）。

执行代理将发现结果报告给编排代理，编排代理进行综合并以连贯的 SITREP（情况报告）形式呈现信息。

编排代理和执行代理并不是盲目搜索。它们由一个调查技能引导，该技能针对每种缺陷类别提供了[更详细的参考 Markdown 文件](https://github.com/anthropics/oncall-kit/tree/main/skills/triage)。

例如，一个针对影子偏差缺陷的 617 行调查技能，编码了我在典型调查中采取的每一个步骤。我是在一次事件处理过程中与 Claude 逐步排查时构建了它，然后让它根据那次经验创建了这个文件。

Lessons.md 也引导 Claude 的故障排查。这个 Markdown 文件是我们解决过的每个事件的持续日志：发生了什么、根本原因、修复方法以及值得记住的坑。Claude 会自动自行追加内容。每次新的调查都会先读取它，因此 Claude 的第一个假设会从最近发生的事情开始。

如果同一模式出现足够多次，我们就会将其提升到调查技能本身中。我最喜欢的一条记录是 Claude 写关于我的。我在检查指标之前根据配置文件做了一个假设，现在 lessons.md 文件中写着：“先查询数据，再提出理论。配置告诉你可能出什么问题；指标告诉你实际出了什么问题。”

即使有了这些工具和上下文，Claude 也并不总能第一次就做对。人类的直觉和经验仍然很重要。Claude Tag 允许团队以多人模式排查事件。我们中的任何一方都可以实时引导调查或添加假设，共同协作。

为清晰起见，根据真实对话重新演绎。

## **解决**

如果 Claude 能够升级和处理告警，那它也能修复它们吗？这个问题的答案会因团队而异，但以下是我们做法。

我们团队中的大多数部署都在功能开关后面进行。我在 Claude Code 中创建了一个独立的代理，使用我的权限，能够在每个功能开关后面进行渐进式部署。

我们发布流程的第一阶段通常涉及 Claude 管理金丝雀流量、监视问题，并自动上调或下调某个功能开关的流量比例。这本身就可以单独写一篇文章，所以我在这里不再赘述。

Claude Tag 帮助我的团队解决的其他路径包括：

* 告知我们是否需要排空或隔离 Kubernetes 集群的某些部分；
* 提供如何扩展部分基础设施以应对需求激增的指令（这种情况很少见，但当 Claude 返回确切可行的缓解措施时非常有帮助）；以及最频繁的，
* 以 PR 形式提供修复方案，值班人员可以审查、合并，然后部署以快速解决问题。

## **验证、沟通和交接**

Claude 使用许多与调查时相同的 MCP 连接器和工具来验证修复是否按预期生效。作为 oncall.md 中常驻指令的一部分，它会将事后分析写入 lessons.md，并生成交接用的 SITREP。

为了在多个事件之间传达全局情况，我们创建了一个名为 ci-weather 的代理。它汇总来自每个事件 Slack 频道、构建指标、合并队列统计和部署延迟的信息，然后向公司任何人都可以阅读的一个公共频道发布新闻编辑室风格的报告。现在，我们的工程师可以查看该频道，而不是在决定是否应该暂缓合并或试图回答“CI 出了什么问题？”时来打扰我们。

一句实话：我们不得不反复调整报告格式好几次。Claude 可以一次性生成一个能产出状态报告的技能，但让它变得易读的是团队特有的偏好。这是人际沟通，不是管道工程。

最后，虽然 Claude 会在 `lessons.md` 中为自己保留日志，我们也希望每周一为人类生成交接报告。Claude 会产出每日和每周摘要，以便团队成员能无缝接续彼此的工作。

## **从监控事件到监控事件响应系统**

我们的软件工程师平均每个季度[交付的代码量是2021年至2025年期间的8倍](https://www.anthropic.com/institute/recursive-self-improvement)。虽然我们一直保持着高质量标准（每个PR都有指定的人类负责人，每项变更都需要批准才能合并，每项变更都经过同一套CI门禁），但跟上智能体编码的唯一方式就是智能体CI。

Claude已经接管了我工作中繁琐的部分——下班后的干扰和事件沟通，同时让我专注于真正提升系统可靠性的中长期架构变革。

我们构建的系统最棒的一点是它并不显得零散。我们的值班流程运行在Slack中，但现在Claude也加入了频道。

如何开始：

* 你需要一个[Claude Team或Claude Enterprise](https://support.claude.com/en/collections/9387370-team-and-enterprise-plans)计划
* 组织所有者需要通过Claude Tag将Claude添加到值班Slack频道
* 组织所有者还需要帮助将值班Slack频道中的Claude连接到相应的连接器、GitHub仓库，并设置[Claude Code Remote](https://code.claude.com/docs/en/remote-control)。
* 将Claude添加到你的事件频道，并指示它监控事件并立即进行分诊

[*使用我们的设置套件配置你自己的Claude值班系统*](https://github.com/anthropics/oncall-kit)*。*

*本文由Anthropic技术成员Sachin Malhotra撰写，Michael Segner（Anthropic员工）亦有贡献。*
