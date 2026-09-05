---
title: "初创企业 Claude Code 指南 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-guide-for-startups"
---

# 初创企业 Claude Code 指南 | Claude by Anthropic

---

需要 PDF 版本？

**本指南也提供下载** —— 同样的五条规则、创始人洞见和检查清单，排版便于离线阅读或与团队分享。

[下载 PDF ↓](https://cdn.prod.website-files.com/6889473510b50328dbb70ae6/6a85f3ce3623355a8a44ca36_Claude_Code_Guide_For_Startups_Final_web.pdf)

## 在前沿工作的 AI 原住民

如果你想一窥工作的未来，不妨问问初创企业今天是如何运作的。于是我们这么做了。

我们与十多家快速成长的初创企业进行了交流，了解他们如何使用智能体编码工具来构建产品并扩大公司规模。这些初创企业正在改变游戏规则：谁能参与构建、什么会被舍弃，以及如何在“如何构建”与“构建什么”之间创造飞轮效应。

而他们的交付速度堪比规模十倍于己的组织。

ClickHouse**30%**更多功能交付

Omni**2–3 倍**工程生产力

Clay**100%**的缺陷分类实现自动化

Artemis Security**每周 6,000+** 个 PR

在本指南中，我们将深入探讨这些组织的独特部署方式，学习他们快速交付并保持竞争优势所遵循的规则。

在此过程中，我们也将开始探寻一个问题的答案：如果一个组织从零开始用 Claude Code 构建其产品开发生命周期，那会是什么样子？

五条规则

1. [人人皆可交付](#rule-1)
2. [自动化繁琐工作](#rule-2)
3. [信任，但需验证](#rule-3)
4. [为重建而构建](#rule-4)
5. [原型、自用、产品化](#rule-5)

收录以下创始人的洞见

[Artemis Security](https://artemissecurity.com/)[Cainex](https://www.cainex.com/)[Clay](https://www.clay.com/)[ClickHouse](https://clickhouse.com/)[Cognition](https://cognition.ai/)[Commure](https://www.commure.com/)[Crosby](https://crosby.ai/)[Emergent](https://emergent.sh/)[Harvey](https://www.harvey.ai/)[Heidi](https://www.heidihealth.com/)[Higgsfield](https://higgsfield.ai/)[Omni](https://omni.co/)[Parahelp](https://www.parahelp.com/)[Translucent](https://www.translucent.co/)[Zingage](https://zingage.com/)

**提示：** 只对实际下一步感兴趣？我们在[本指南末尾提供了一份检查清单](#checklist)，汇总了各章节中的关键技术要点。

**01**

## 人人皆可交付

智能体编码降低了准入门槛，因此理解问题的人可以交付修复方案的首个版本。

智能体编码降低了非技术员工构建产品的准入门槛。借助 Claude Code，即使不精通编程语言或不熟悉 IDE 的使用，你也能创建功能完备的特性。

> “不仅工程师的交付量大幅提升，非技术人员（比如我）也能突然交付 UI 改动和其他产品改进了。”**[Mads Lunau Liechti](https://www.linkedin.com/in/mads-lunau-liechti/)** · [Parahelp](https://www.parahelp.com/) 联合创始人

对初创企业创始人而言，这有着明显的优势。首先，他们没有大型竞争对手那样的人力规模，因此需要“全员上阵”。但创始人追求的不仅仅是纯粹的产能——这些非技术团队成员还带来了领域专业知识。

> “Claude Code 改变了在 Crosby 做律师的意义。律师拥有最好的产品洞察，因为他们就是用户。看着他们大展身手，真是太棒了。”**[Ryan Daniels](https://www.linkedin.com/in/crosbyryan/)** · [Crosby](https://crosby.ai/) 联合创始人兼 CEO

我们从 [Heidi](#) 的联合创始人兼 CEO Thomas Kelly 博士那里也听到了同样的声音。

> “对我们来说，Claude Code 解决了‘传话游戏’的问题。过去，一个新想法在团队中的流转方式是：有想法的人告诉产品经理，产品经理告诉设计师，设计师再告诉工程师……而不可避免地，想法的核心往往在这一链条中丢失。等到最终交付时，往往已经不像那个人最初设想的样子了，而且耗时数周。Claude Code 压缩了这条链条。真正理解问题的人可以直接交付一个 PR，在需要设计师和工程师专业知识的环节再让他们介入。”**[Thomas Kelly 博士](https://www.linkedin.com/in/tomkeykong/)** · [Heidi](https://www.heidihealth.com/) 联合创始人兼 CEO

说“人人皆可交付”很适合发一条 LinkedIn 帖子，但在现实中如何运作？是市场团队在审批 pull request 吗？是法务团队在处理二分定位不稳定测试的复杂问题吗？

我们得到的答案是：分工仍然存在。市场人员仍专注于市场工作，开发人员仍专注于开发工作。但最关键的第一步——将一个想法变成可工作的原型，实现从 0 到 1——对所有人开放。

我们还发现，最有效的初创企业会建立机制，让这些贡献系统化，而不是靠偶然或个人积极性。

### 建立连接

要求员工使用 AI 是一回事，让他们真正获得 Claude Code 和他们所需的工具则是另一回事。

> “我们实际上并没有回避（让非技术员工参与贡献），而是在主动拥抱它。我们的观点是，每个角色都在变成工程角色，因为你可以为它构建软件……所以我们招聘那些喜欢动手捣鼓、对构建感兴趣的人。”**[Kareem Amin](https://www.linkedin.com/in/kareemamin/)** · [Clay](https://www.clay.com/) 联合创始人兼 CEO

在 Crosby，团队没有把律师带到 Claude Code 面前，而是把 Claude Code 带到了律师面前——将它连接到他们熟悉且每天使用的工具和操作系统中。

**提示：** Claude 无法理解它看不到的东西。扩展 Claude 价值的最有效方式之一，就是将它连接到数据源以及你的团队每天使用的工具。

[MCP](https://code.claude.com/docs/en/mcp) 是一个用于 AI 工具集成的开源标准，可让 Claude Code 访问你的工具、数据库和 API。当你的团队发现自己在将工具中的信息复制粘贴到 Claude 时，不妨探索添加这些连接。

当已有成熟的命令行工具（`gh`、`kubectl`、`bq`、`psql`）且你希望 Claude 基于与工程师相同的真实数据源工作时，通过 CLI 连接可能更具 token 效率。

Claude Code 桌面版中的 MCP 连接器目录。

### 站会展示

在某个节点，想法需要有机会被排定优先级，以便组织资源帮助其推向市场。这条路对产品经理来说是清晰的——毕竟那是他们的本职工作——但对非技术员工来说就没那么清晰了。

Clay 设立了季度评审机制，原型可以在评审中被考虑并进入正式路线图。正是通过这种方式，Clay 的一位市场团队成员构建了一个自主智能体，它可以访问你的网站、填写你的潜在客户收集表单、计时响应速度、评估体验并生成绩效报告。

Omni 有一个专门的 Slack 频道用于展示 Claude 生成的原型，包括高级技术人员在内的所有人都可以贡献。他们还践行“人人皆可交付”的推论——“人人皆与客户交流”。

> 尽管工程师天然不倾向于参加客户电话会议，但 Omni 刻意让他们直面客户，因为这能更快地闭环反馈循环。**[Chris Merrick](https://www.linkedin.com/in/merrickchristopher/)** · [Omni](https://omni.co/) 联合创始人兼 CTO

### 共享技能

“人人皆可交付”与“零散拼凑”之间往往只有一线之隔。无论功能原型来自谁，仍然需要被整合到一个具有整体感的产品中。这时，技能（skills）——编码团队标准和上下文的可复用指令文件——就能帮助确保即使在流程日益民主化的过程中，开发仍保持一致。

“团队中的任何人都可以通过 Claude Code，以我们的设计系统为参考，起草产品组件、市场物料或演示文稿内容。凡是触及产品的 AI 必须通过更高的标准，而 Claude Code 帮助我们以更高的精度达到这一标准。”Heidi 的 Thomas Kelly 博士表示。

技能还可以帮助新开发人员和非技术员工快速上手并投入工作。

> “……我们还有一个 Claude Code 技能的 GitHub 仓库，作为一个共享知识库，可以快速启动一个 Claude Code 会话，其中包含已知的 Emergent 细节，比如数据库[和数据仓库]位置、一些模式[信息]、整体公司背景……与其追求完美，不如接受稍微过时的上下文文件，只要智能体能够快速验证并纠正方向即可。”**[Mukund Jha](https://www.linkedin.com/in/mukund-jha-a1596413/)** · [Emergent](https://emergent.sh/) 联合创始人兼 CEO

> “我们的工程师使用 Claude Code 搭建了一个内部市场，提供按角色组织的专业化内部智能体，让工程、交付和销售团队各自获得针对其实际工作方式构建的工具。”**[Jack O'Hara](https://www.linkedin.com/in/jack-o-hara-/)** · [Translucent](https://www.translucent.co/) 创始人兼 CEO

**提示：** 技能[可以通过目录在公司内共享](https://code.claude.com/docs/en/plugin-marketplaces)，这样一位员工的最佳实践可以即时传递给另一位员工。在仓库的每个子目录中使用 `CLAUDE.md` 文件，以规定适用于该子目录的编码约定。使用技能来处理按需的程序性工作流。更多信息，请阅读：[引导 Claude Code：何时使用 CLAUDE.md、技能、钩子和子智能体](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more)。

**02**

## 自动化繁琐工作

智能体负责生命周期中机械性的 80%，让工程师将时间花在真正需要判断力的案例上。

自工业革命以来，所有公司都在寻求通过技术提升效率，但这些初创企业凭借其采用的速度和深度脱颖而出。

这些创始人相信 AI 是其使命的核心组成部分。许多人明确表示，智能体负责机械性的 80%，以便工程师将时间花在真正需要判断力的案例上。

> “每个人都在竞相构建 AI 产品。但很少有人真正重建公司的运营方式。后者才是更大的解锁点。Artemis Security 是一家以 AI 原生方式运营的公司，而不是一家碰巧使用 AI 的公司。这极大地加速了我们的速度，使我们能够帮助客户以机器速度阻止攻击。”**[Shachar Hirshberg](https://www.linkedin.com/in/shachar-hirshberg/)** · [Artemis Security](https://artemissecurity.com/) 联合创始人兼 CEO

具体来说，我们看到AI在他们SDLC各阶段的整合程度比其他公司更深，同时也有更多专门构建的代理，旨在端到端处理重复性任务。让我们看几个这两类情况的例子。

### AI原生的SDLC

许多这些入选的初创公司已经实施了加速团队入职其代理编码流程的方法。例如，在Emergent，Mukund告诉我们，“新员工在第一天就通过将Claude指向正确的markdown文件来引导整个开发环境。如果Claude在入职过程中遇到任何损坏或过时的内容，它会更新该文件。”

**提示：** [代码审查](https://code.claude.com/docs/en/code-review)（研究预览版）是Claude Code中一项托管的的多代理服务。它会在你启用的仓库中对PR运行自动审查。你可以手动修复发现的问题并推送，或通过在发现上评论`@Claude`来闭环（如果你已设置并配置了GitHub Actions）。

代码审查会为每个发现标记严重级别。

这些工程师需要快速入职，因为这些团队交付速度很快。

> “这里的工程师正在编排代理集群，在发现生产数据问题的当天就修复并推送，同时并行处理多个PR。一位工程师使用Claude子代理并行运行了一个约13个工单的计划，每个子代理负责一个工单及其PR。”**[Tanay Tandon](https://www.linkedin.com/in/tanaytandon/)** · CEO兼创始人，[Commure](https://www.commure.com/)

在这些组织中，Claude Code不仅帮助生成代码，还负责审查代码。“我们针对经过验证的技术和合规框架运行自动化代码审查，在任何内容发布前标记关键问题并将建议的更改路由给正确的审查者，”Heidi的Kelly博士说。

其中一些组织还构建了用于代码审查、测试和CI的自定义代理。这些初创公司非常重视[构建循环](https://claude.com/blog/getting-started-with-loops)，而不仅仅是部署代码。

“我最喜欢的[代理]是‘Translucent代码审查器’，它会分散到整个更改中，从多个角度进行审查，并像我们的一位资深工程师那样综合结果，但比任何个人都快，”Translucent创始人Jack说。

Clay“……构建了一个处理……缺陷分类的代理，从初步审查到为修复建议代码更改，”Kareem说。

**提示：** 在过去的几个月里，[Claude Tag](https://claude.com/product/tag)一直是Anthropic CI/CD故障的on-call第一响应者。Claude在最近每个有情况报告的突发事件中都撰写了第一份情况报告，通常在15分钟内发布其首次分析。

[Claude Tag拥有自己的服务账户](https://claude.com/blog/agent-identity-access-model)，并可以访问Anthropic CI工程师所需的工具，如Datadog或Grafana。固定指令以技能形式存储在markdown文件中，并提交到GitHub仓库。这样多个团队成员可以对其进行迭代，我们可以像管理代码一样管理更改。

Claude Tag会在Slack中接起on-call线程，并在频道中报告进度。

‍

> 这在[ClickHouse](https://clickhouse.com/)最为明显，**联合创始人兼CTO Alexey Milovidov报告称**，这家数据库公司已将几乎每个SDLC阶段转变为自主循环。两个专门构建的代理，用于修复不稳定的测试和发现缺失的测试覆盖，现在是ClickHouse仓库的第2和第3大贡献者。另一个代理系列负责运维，团队使用Claude Code来构建和迭代这些代理本身。

### 用代理加速流程

另一个一致的模式是，这些初创公司不仅使用Claude Code中的代理循环来加速开发工作，还创建代理来加速重复且往往繁琐的流程。

这通常是例行工作，以便更多注意力可以集中在他们的竞争优势、客户关系和收入增长上。我们看到Claude加速的最常见流程之一是自助式数据分析。

这些公司几乎每家都建立了某种流程，以便用新鲜数据（包括非结构化数据）做出快速决策，这些数据驱动着初创公司生命周期中至关重要的快速转向。

例如，Clay构建了一个内部分析代理，Heidi使用Claude Code对客户和临床医生的反馈以及使用数据进行分类，以揭示对产品洞察重要的信号。

ClickHouse和Omni都交付了将这类AI数据分析打包在产品中的方案，全部由Claude提供支持。

其他例子包括使用子代理总结数千份法律文件（Crosby）、扫描索赔数据以标记跨站点异常（Commure），以及持续挖掘医院财务数据以发现任何分析师团队都无法及时捕捉的警告信号（Translucent）。

**提示：** [动态工作流](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)可用于分散多个子代理并行分析大量数据，或对另一个代理的工作进行对抗性审查。当使用像Claude Opus或Claude Fable这样的模型时，可以说“分散多个子代理”或“使用工作流”。

**03**

## 信任，但需核实

除非你有可靠的手段来监控和验证结果，否则你无法自动化一个流程。

这条规则是规则2“自动化繁琐工作”的必要推论。除非你有可靠的手段来监控和验证结果，否则你无法自动化一个流程。

> Artemis Security联合创始人Dan Shiebler表示，他们之所以能提高部署速度，是因为……“我们在测试基础设施、代码库组织和团队知识系统上投入了大量精力，这让智能体能够端到端交付。这就是我们与Claude构建的飞轮效应：以正确的方式组织代码库、知识库和团队，每一次贡献都会产生复利。”**[Dan Shiebler](https://www.linkedin.com/in/dan-shiebler-10219b42/)** · 联合创始人，[Artemis Security](https://artemissecurity.com/)

> “早期我们给了Claude完全自主权，它做了AI会做的事——快速交付看似合理的代码。问题在于它会以看似正确实则错误的方式偏离我们的架构。于是我们……写下了每一条不变量：我们如何定义问题；无论发生什么都必须成立的条件；如何证明某件事有效，而不是相信一个自信的回答。567行关于这个团队如何思考的准则。”**[Victor Hunt](https://www.linkedin.com/in/victor-c-hunt)** · 联合创始人兼CEO，[Zingage](https://zingage.com/)

**提示：** 将不可更改的内容放在仓库根目录的`CLAUDE.md`中。Claude在每次会话开始时都会读取它，因此你的架构规则、安全边界和不可妥协项会随每次会话一同生效。

需要明确的是，这些初创公司都不是让智能体直接合并到主分支然后听天由命。其中许多公司运营在高度监管的行业，需要强大的治理框架。Cainex是一个特别有说明力的例子，它结合了智能体与确定性检查来读取医疗记录并生成指导医院计费的代码。

> “在医疗编码中，错误的代码不是打字错误，而是一次计费和合规事件。这一事实决定了我们如何构建产品。”**[Uriah Israel](https://www.linkedin.com/in/uriah-israel/)** · 联合创始人兼CTO，[Cainex](https://www.cainex.com/)

“这就是Claude Code为我们运行的循环。我们用智能体处理一批数据，然后审计员在内部应用中审查输出。他们不仅看到代码，还看到模型的推理过程，并对两者进行评论……所有内容都有版本记录且可审计，”他说。

“然后Claude Code接手。它直接从数据库中读取原始预测以及每一条修正和评论。每条修正都按涉及的代码类型打上标签，因此Claude Code知道它面对的是诊断问题、手术问题还是其他类别，并能直接跳转到管辖该类编码的指南。

接下来，它会找到智能体指令中产生错误的部分并加以修订，或者在案例确实全新时编写新的指南。每项更改都基于有版本管理的指令集，并针对失败的记录进行测试。我们执行的规则是：修正原则，而非修正个例，”他继续说道。

“然后是回测。一条记录可能有多个可接受的编码，所以不是简单的字符串匹配。检查结合了针对我们已接受集合的语义匹配，以及一个判断器来问‘这是真正的错误还是只是另一条有效的路径’，Claude Code还会在此基础上添加自己的比较。

它会在黄金集加上随机样本上运行候选更改，并在任何内容上线前暴露回归问题。返回的是一个简短列表：建议的编辑、无法解决的记录，以及它想要得到解答的问题。工程师将时间花在真正困难的案例上，而不是机械化的80%，”他说。

创始人们可以从这个医疗计费特定工作流中提炼出许多通用经验。

例如，Cainex使用领域专家定期审查和指导Claude的推理，并确保这些指导成为自我改进循环的一部分。然而，这些专家不是来逐例修正的，他们的指导被用作自我改进循环的一部分。正如Uriah所说：“修正原则，而非修正个例。”

**提示：** 循环（Loops）是重复执行工作周期直到满足停止条件的智能体。它们可以成为使用Claude Code进行更自主或长期工作的有效方式。[你可以使用技能（skills）来定义智能体需要满足的标准](https://claude.com/blog/building-verification-loops-in-claude-code-with-skills)（定义越清晰越好），并让智能体迭代直到达成目标。

例如，许多组织会创建“不稳定测试智能体”或循环，因为停止条件清晰且自包含：智能体可以通过重新运行测试来验证自己的修复，直到测试通过。

循环重复执行工作周期，直到满足停止条件。

另一个经验是维护强大评估“黄金集”的严谨态度——即一组经过验证的问答对，团队用它来验证智能体的准确性。每家初创公司都应为其关键用例维护多套评估集，并定期更新，以便防止漂移并评估未来模型。

> “[Claude Code]还改变了我们管理模型迭代速度的方式。新的视频和图像模型不断涌现。每个模型在部署前都需要新的技能、评估、路由逻辑和生产测试。Claude Code将这个周期从几天压缩到几小时，让我们能在同一会话中发现生产问题并部署修复……当你与拥有10倍人力的公司竞争时，这种杠杆效应改变了一切。”**[Alex Mashrabov](https://www.linkedin.com/in/amashrabov)** · 联合创始人兼CEO，[Higgsfield](https://higgsfield.ai/)

**提示：** 当团队刚开始构建智能体时，通过手动测试、自用测试和直觉的组合，他们能走得出奇地远。突破点通常出现在用户反馈智能体在更改后感觉变差了，而团队在“盲飞”——除了猜测和检查之外没有验证手段。团队无法区分真正的回归与噪声，无法在上线前自动针对数百个场景测试更改，也无法衡量改进。更多信息请阅读：[为AI智能体揭开评估的神秘面纱](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)。

Uriah最后指出，这个过程需要付出努力。“它一开始并不这么干净。我们的第一版过拟合了。它会通过编码特定案例来‘修复’问题，我们积累的是补丁而不是变得更聪明。我们改变了方法，强制推行通用原则，并限制一个更改中能包含多少具体细节。”

**提示：** AI智能体不是确定性的，但大量高度监管的工作要求流程每次都按相同方式执行。Claude Code具有可以将前沿智能与确定性流程结合的功能。

[Hooks](https://code.claude.com/docs/en/hooks)是用户定义的命令，在Claude Code生命周期的固定节点触发，可以作为硬性门禁。它们每次都会执行，无论模型做出什么决定。例如，它们可以用于阻止未通过lint的写入、要求提交前测试通过，或在任何内容离开沙箱前剥离机密信息。

[动态工作流](https://code.claude.com/docs/en/workflows#orchestrate-subagents-at-scale-with-dynamic-workflows)以确定性顺序、独立上下文窗口和聚焦目标来编排子智能体。`/goal`对于Claude可能过早宣布任务完成、审查时偏向自己的发现、或偏离原始目标的长时间复杂任务非常有用。

**04**

## 为重建而构建

模型能力在这些团队脚下不断变化，因此很少有东西被视为永久性的。

这些AI原生初创公司中的许多都处于持续重塑的状态。

AI往往既是他们构建内容的核心，也是他们构建方式的核心。由于模型能力不断演进，突破性功能和关键支撑框架在成为沉没成本的那一刻就被舍弃了。这些组织中的许多将这种持续重建视为其竞争优势的一部分。

“我们在Clay的做法是：你构建它，然后再次构建它，然后再构建一次。第四次构建时，你知道了所有需要的东西，就能做对了。所以我们不一定抛弃东西，我们只是重建它——而这一次带着更清晰的认知，”Kareem说。

“一次重建不是在新的路径上线时完成的，而是在旧路径消失时完成的。以前，拆除工作总是输在优先级争夺上——它枯燥乏味，又不交付任何功能，”Commure联合创始人Tanay说。“现在Commure的一位工程师只需调用一个Claude技能，大意是‘对于每个已向所有人发布的功能开关，创建一个PR来移除它及相关代码’，然后工程师审查返回的结果。过去消耗大量开发周期的迁移，现在只是一个计划和一次分发，几小时内就能完成。”

**提示：** 使用[git worktrees](https://code.claude.com/docs/en/worktrees)在仓库的隔离副本中运行重建，同时保持当前版本不受影响。Claude Code可以为你启动一个——你可以让v2与v1并行运行，对两者运行评估，只有在新版本胜出时才合并。这就是让“构建四次”变得廉价的原因。

一个仓库，一个对象存储——三个你可以同时工作的检出目录，各自位于独立分支上。

每个关联的worktree都是一个普通目录，拥有自己检出的分支；三个目录共享acme-web内的同一个.git对象存储。

Kareem还将Clay护城河的一部分描述为持续重建、演进和创建自我改进循环的能力。

“我认为当下任何公司的护城河在于它必须能够自我改进。所以Clay是一个自学习营收引擎。你使用得越多，我们就越了解谁是你最好的客户、你应该说什么、什么有效、什么无效，而这些都在随时间变化，”他说。“这场竞赛实际上是谁能最快获得分发……这样你就能帮助每个[客户]，从而实现自我改进。”

在[2026年5月的Code with Claude活动](https://www.youtube.com/live/OFDm3T7pVlc?si=Z_RENcJSqm8H79aj)上，Harvey的应用AI主管Niko Grupen谈到了每一波新的模型能力——涌现推理、智能体自动化、规划与编排——如何要求对平台进行全面重新架构。

> “如果你六个月前问我我们的架构是什么样的，我会给出一个与今天截然不同的答案。如果我们不愿意说‘嘿，我们需要抛弃这个，转向智能体原生’，我们现在根本不可能在平台中拥有这些能力。”**[Niko Grupen](https://www.linkedin.com/in/nikogrupen)** · 应用AI主管，[Harvey](https://www.harvey.ai/)

在同一场活动中，Cognition 联合创始人 Walden Yan 表示：

> “当前构建 AI 的生活方式是接受这样一个事实：你今天构建的东西很可能在六个月到一年内就会被废弃…… [Devin] 在两年前我们拥有的模型组合下是完全不可能实现的，[但当时的赌注是] 今天可能行不通，但很快会实现。”**[Walden Yan](https://www.linkedin.com/in/waldenyan)** · 联合创始人，[Cognition](https://cognition.ai/)

**提示：** 对于非平凡的改写，请在[计划模式](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode)下启动 Claude Code（`--plan` 或按 Shift+Tab）。Claude 将先探索代码库并提出重建方案，然后再编写任何代码——您可以批准或调整方向。这是捕捉即将偏离您架构的重建方案最经济的方式。

**05**

## 原型、内部试用、产品化

借助AI进行构建，帮助这些初创公司用AI打造颠覆性产品——这是他们流程核心的飞轮。

许多这类初创公司的开发流程核心都有一个关键飞轮。借助AI进行构建，帮助他们用AI打造颠覆性产品。

当开发者推进他们的智能体编码实践时，他们能更深入地掌握模型的能力，并洞察前沿的“工具框架”（harness）设计如何演进。然后，他们可以将这些灵感应用到自己的智能体和产品中。

“我们从[Anthropic的]文件与嵌入方法中汲取灵感，这鼓励我们在自己的产品中保持简单。我们避免了RAG流水线会带来的大量复杂性，”Omni的Chris说。“我们还看到Claude Code的工具框架如何让用户并行做事，并将其中一些概念改编到了我们自己的UI中。”

这也有助于他们保持对自己产品性能的敏锐感知。

“因为我们的应用构建器在后台也使用Anthropic模型，如果我们在产品上看到任何行为……我们可以通过Claude Code在本地快速调试，判断是模型行为还是工具框架问题。这极大地改善了我们的分诊周期，”Emergent的Mukund说。

我们反复听到的模式是：先用Claude Code构建一个内部智能体，在内部使用（dogfood），然后根据反馈，通常使用Claude API、SDK或Claude托管智能体，将其推广为面向客户的产品。

“我们在[产品中]构建了自己的AI智能体，供团队直接交互，包括SQL控制台中的一个智能体和AI SRE。我们使用Claude Code来构建和迭代这些智能体本身。为客户AI体验提供支持的工具，部分是由AI构建的，”ClickHouse的Alexey说。

## 清单

本指南涵盖了大量内容。以下是将关键提示汇总在一页上：

#### 第1章：人人皆可交付

Claude无法理解它看不见的东西。通过[MCP](https://code.claude.com/docs/en/mcp)或CLI将其连接到事实来源以及团队日常使用的工具。创建一个[公司插件市场](https://code.claude.com/docs/en/plugin-marketplaces)，让一位员工的最佳实践可以通过技能即时传递给另一位员工。在仓库的每个子目录中使用CLAUDE.md文件，以规定适用于该子目录的编码约定。使用技能来处理按需的程序化工作流。

#### 第2章：自动化繁琐工作

在仓库上设置[代码审查](https://code.claude.com/docs/en/code-review)（研究预览），对PR进行自动化审查。将[Claude Tag](https://claude.com/product/tag)（公开测试版）纳入CI/CD的待命响应和缺陷分诊流程。[动态工作流](https://code.claude.com/docs/en/workflows#orchestrate-subagents-at-scale-with-dynamic-workflows)可用于扇出多个子智能体，并行分析大量数据，或对另一个智能体的工作进行对抗性审查。

#### 第3章：信任，但需验证

将不可更改的内容放入仓库根目录的CLAUDE.md中。使用[循环](https://code.claude.com/docs/en/workflows)——即重复工作周期直到满足停止条件的智能体——来处理更自主或长期的任务。建立[创建和维护智能体评估](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents)的流程。[钩子](https://code.claude.com/docs/en/hooks)是用户定义的命令，在Claude Code生命周期的固定点触发，可作为硬性门禁。当工作的某些部分需要确定性时，请使用这些钩子。

#### 第4章：为重建而构建

使用[git worktrees](https://code.claude.com/docs/en/worktrees)在仓库的隔离副本中运行重建，同时保持当前版本不变。这就是让“构建四次”变得廉价的原因。对于非平凡的重写，以[计划模式](https://code.claude.com/docs/en/permission-modes#analyze-before-you-edit-with-plan-mode)（/plan或按Shift+Tab）启动Claude Code。Claude将探索代码库并在编写任何代码之前提出重建方案——由您批准或调整方向。这是捕捉可能偏离架构的重建方案的最廉价时机。

## 前沿的初创公司在前沿构建

这些见解来自您在前沿构建的同行的经验，我们希望您觉得它们实用且可操作。Claude初创社区是灵感、最佳实践和建议的持续来源。您可以通过以下方式加入这个社区：

* [订阅初创通讯并加入初创计划](https://claude.com/programs/startups)。
* [收藏即将举行的Claude Code网络研讨会](https://academy.claude.com/code/webinars)。
* [参加您附近的活动](https://luma.com/claudecommunity)
* 在[Reddit](https://www.reddit.com/r/ClaudeAI/)和[Discord](https://discord.com/invite/6PPFFzqPDZ)上贡献内容。
* 早期公司还可以申请[Claude for Startups计划](https://claude.com/programs/startups)以获得积分和支持。
