---
title: "认识我们 Built with Opus 4.6 Claude Code 黑客马拉松的获奖者 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/meet-the-winners-of-our-built-with-opus-4-6-claude-code-hackathon"
---

# 认识我们 Built with Opus 4.6 Claude Code 黑客马拉松的获奖者 | Claude by Anthropic

---

上周，我们宣布了 [*Built with Opus 4.7* 虚拟 Claude Code 黑客马拉松](https://cerebralvalley.ai/e/built-with-4-7-hackathon)，邀请社区使用[我们最新的](https://www.anthropic.com/news/claude-opus-4-7) Opus 模型进行构建。与之前以 Opus 4.6 为特色的黑客马拉松一样，我们与 Cerebral Valley 合作，选出 500 名参与者，为每人提供 500 美元的 API 积分，并给予一周时间使用 Claude Code 进行构建。Claude 团队的评委将选出六名获奖者，并从总计 100,000 美元的 Claude API 积分奖池中为他们颁奖，用于项目开发。

我们 Opus 4.6 黑客马拉松（本系列的首届）的获奖者包括一名人身伤害律师、一名心脏病专家、一名道路与基础设施专家、一名电子音乐家和一名专业软件工程师。他们着手开展的项目旨在改善住房、医疗保健、基础设施、音乐和教育。五位获奖者中有四位并非专业开发人员。

我们希望他们的项目能激励您去构建有意义的东西。

## 第一名：[CrossBeam](https://www.youtube.com/watch?v=jHwBkFSvyk0)，Mike Brown

图片由 Mike Brown 提供。

加利福尼亚州的住房许可证首次提交的驳回率超过 90%，平均延迟六个月，给房主造成 30,000 美元的损失。大多数情况下，问题出在官僚程序上：签名缺失、法规引用编号错误、表格填写不完整。

“每个人都认为加利福尼亚存在住房危机。其实没有。我们面临的是许可证危机，”人身伤害律师 Mike Brown 说。获得建造新住宅所需的许可证可能比建造本身耗时更长。Mike 的黑客马拉松项目 [CrossBeam](https://github.com/mikeOnBreeze/cc-crossbeam) 利用 AI 帮助打破加利福尼亚的许可证审批瓶颈。

使用 CrossBeam，建筑商可以将蓝图和整改函拖放到工具中；并行子代理解析文档，构建空间索引，并为每个独立的整改项分配针对性的代理。二十分钟后，建筑商就能获得一份精确的审批行动计划。在审批方一侧，CrossBeam 允许市政当局批量处理提交的许可证，并自动生成整改函草稿。

Buena Park 是南加州的一个城市，需要在 2029 年前批准超过 8,900 套住房，但在 2024 年仅批准了约 120 套。该市正在考虑采用 CrossBeam 来加快许可流程，不仅惠及建筑商，也惠及审阅堆积如山文件的行政人员。

“如果我们能同时解决加州许可证危机双方的问题，”Mike 说，“我们或许真的能解决加州的住房危机。”

Mike 使用提示 Claude Code 的工作流程构建了 CrossBeam，然后让 Claude 创建测试。“我最终赢得了这场比赛，而我没有写一行代码，这对我来说太不可思议了，”他说。“我甚至没有读过一行代码。”

[*在 GitHub 上查看 CrossBeam。*](https://github.com/mikeOnBreeze/cc-crossbeam)

## 第二名：[Elisa](https://www.youtube.com/watch?v=rsUaz_QAK6o)，Jon McBee

图片由 Jon McBee 提供。

当 Jon McBee 12 岁的女儿需要为七年级科学展项目编写微控制器程序时，他希望她能使用他作为软件工程师每天使用的同一工具：Claude Code。但终端界面并非为初中生设计的——所以他构建了一个适合他们的工具。

Elisa 是一个基于积木的可视化集成开发环境（IDE），用户可以通过拼接原语（目标、需求、代理、技能、规则、门户、部署）来设计软件，而 AI 在幕后编写真正的代码。用户用可视化语言编写规格说明，元规划器将其分解为任务图，然后由代理处理其余部分。内置的教学引擎会以适合年龄的方式解释所使用的编程概念，将每次构建都变成一堂课。Jon 的女儿用它为她的项目将代码刷入微控制器，而没有编写一行代码。

Jon 使用 Claude Code 在 30 小时内构建了 Elisa，完成了 76 次提交，包含超过 39,000 行代码和超过 1,500 个测试。“我了解系统架构。我知道如何集成硬件。我知道如何定义和测试软件，”他说。“Claude Code 帮助我在短短六天内将所有知识转化为可交付的产品。”

教育工作者已联系 Jon，希望在课堂上使用 Elisa，Jon 正努力用他获得的第二名 30,000 美元 Claude API 积分来资助这项工作。他相信软件创作者很快将不再编写源代码，而是使用定义明确的测试和规格说明进行工作。*Elisa* 将这个想法包装在一个适合儿童的可视化界面中。“我以女儿的名字命名这个项目，”Jon 说，“因为她正是这个项目的目标用户。”

[*在 GitHub 上查看 Elisa。*](https://github.com/zoidbergclawd/elisa)

## 第三名：[PostVisit.ai](https://youtu.be/V29UCOii2jE)，Michał Nedoszytko

图片由 Michał Nedoszytko 提供。

Michał Nedoszytko 是布鲁塞尔的一名心脏病专家，他在从事医疗工作的同时，花了 20 年时间构建医疗软件。他之前的项目 Previsit.AI 是一个 AI 患者分诊系统，已在比利时、希腊和波兰部署。但他过去两年真正想构建的产品是其对应部分：就诊后会发生什么。“我在心导管室做过数千次手术，”他说。“但真正的挑战在你离开房间的那一刻才开始。”

PostVisit 是一套工具，用通俗易懂的语言解释诊断结果，分析就诊记录和 AI 抄录的转录文本，并在医生监督下从科学资源和完整健康记录中提取相关的临床证据。患者能更清楚地了解自己的护理情况，医生也能了解患者在两次就诊之间的状况。该助手围绕隐私、安全和临床最佳实践而构建。

为了构建 PostVisit，Michał 进行了一次黑客马拉松公路旅行，在从布鲁塞尔前往旧金山的途中进行开发。“在路上是我最好的想法诞生的地方，”他说。一周和几千英里之后，他拥有了自己多年来一直想象的产品。“医学基于证据，”他说。“现在，通过结合健康记录、证据和就诊数据，患者能够完全掌控并理解就诊后发生的一切。”

[*在 postvisit.ai 了解更多。*](http://postvisit.ai)

## “持续思考”奖：[TARA](https://www.youtube.com/watch?v=GFCrXehS1DE)，Kyeyune Kazibwe

图片由 Kyeyune Kazibwe 提供。

乌干达的道路基础设施需求远远超出其预算所能解决的范围，而传统的可行性研究也无济于事：它们耗资 100 万至 400 万美元，需要 9 到 14 个月，并且只关注经济预测，而没有考虑道路所服务的社区。当时在乌干达工程与交通部工作的 Kyeyune Kazibwe 构建了 TARA，以改变这些决策的制定方式。

TARA 将行车记录仪拍摄的道路视频转化为完整的投资评估。该工具利用 Opus 4.6 的视觉能力分析每一帧，识别路面状况、损坏模式以及路边活动，包括行人、自行车骑手和集市摊位。系统将道路划分为不同的状况路段，自动填充干预成本，并生成完整的经济评估，包括净现值、现金流预测和敏感性分析。它还会生成一个公平性评分，评估谁真正从投资中受益，并考虑行驶过程中识别出的附近设施和高关注区域。

在黑客马拉松中，Kyeyune 上传了来自 Kira - Matugga 公路（目前正在乌干达建设中）的实际行车记录仪视频。“一键即可生成完整的 PDF 报告：状况评估、经济分析、公平性调查结果、敏感性解读，全部集中在一份文档中，”他说。“这个过程过去需要数周。TARA 只需五小时即可完成。”

[在 GitHub 上查看 *TARA。*](https://github.com/Kye256/tara-transport-assessment)

## 特别奖——创意探索：[Conductr](https://www.youtube.com/watch?v=X6CqJoyj0kI)，Asep Bagja Priandana

图片由 Asep Bagja Priandana 提供。

Asep Bagja Priandana 构建了 Conductr，将 Claude 变成现场虚拟乐队伙伴。这款基于浏览器的 MIDI 乐器会聆听您在控制器上弹奏的和弦，分析您的演奏，并实时生成四个音轨——鼓、贝斯、旋律和和声。输入 *“make it funky”* 或 *“build to a climax”*，编曲会在即兴演奏中即时改变。

他说，技术挑战在于保持音乐不中断。一个编译为 WebAssembly 的 C 引擎每 15 毫秒生成一次音符，因此 AI 的决策可以重塑编曲而不会中断流畅性。正如 Asep 所说，延迟是“音乐上不可感知的”。

Conductr 运行在约 4,800 行 JavaScript 和 WebAssembly 代码上：这是一个精简的构建，用于一个能聆听、思考并与您实时合奏的乐器。

[*在 GitHub 上查看 Conductr。*](https://github.com/nanassound/conductr)

请继续关注我们 Built with Opus 4.7 黑客马拉松获奖者的最新消息。

[***了解***](http://claude.com/community) ***我们的 Claude 社区项目，包括聚会、黑客马拉松等。***
