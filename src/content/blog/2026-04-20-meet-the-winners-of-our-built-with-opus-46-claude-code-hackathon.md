---
title: "欢迎了解我们的 Built with Opus 4.6 Claude Code 黑客马拉松获奖者"
description: "上周，我们宣布了 [*Built with Opus 4.7* 虚拟 Claude Code 黑客马拉松](https://cerebralvalley.ai/e/built-with-4-7-hackathon)，邀请社区使用[我们最新的](https://www.anthropic.com/news/claude-opus-4-7) Opus 模型进行构建。与我们之前以 Opus 4.6 为特"
pubDate: "2026-04-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/meet-the-winners-of-our-built-with-opus-4-6-claude-code-hackathon"
---
            上周，我们宣布了 [*Built with Opus 4.7* 虚拟 Claude Code 黑客马拉松](https://cerebralvalley.ai/e/built-with-4-7-hackathon)，邀请社区使用[我们最新的](https://www.anthropic.com/news/claude-opus-4-7) Opus 模型进行构建。与我们之前以 Opus 4.6 为特色的黑客马拉松一样，我们与 Cerebral Valley 合作，选出 500 名参与者，并为每位参与者提供 500 美元的 API 积分和一周时间使用 Claude Code 进行构建。然后，Claude 团队的评委将选出六名获奖者，并从总计 100,000 美元的 Claude API 积分奖池中向他们颁发奖金，用于项目开发。

我们 Opus 4.6 黑客马拉松（本系列的第一届）的获奖者包括一名人身伤害律师、一名心脏病专家、一名道路和基础设施专家、一名电子音乐人和一名专业软件工程师。他们着手开展项目，以改善住房、医疗保健、基础设施、音乐和教育。其中四名获奖者并非专业开发人员。

我们希望他们的项目能激励您构建有意义的东西。

## 第一名：[CrossBeam](https://www.youtube.com/watch?v=jHwBkFSvyk0)，Mike Brown

图片由 Mike Brown 提供。

加利福尼亚州的住房许可证首次提交的拒绝率超过 90%，平均延迟六个月，给房主造成 30,000 美元的损失。大多数情况下，问题出在官僚程序上：缺少签名、错误的法规引用编号、不完整的表格。

"每个人都认为加利福尼亚州存在住房危机。其实不然。我们存在许可证危机，"人身伤害律师 Mike Brown 说。获得建造新住宅所需的许可证所需的时间可能比施工本身还要长。Mike 的黑客马拉松项目 [CrossBeam](https://github.com/mikeOnBreeze/cc-crossbeam) 利用人工智能帮助打破加利福尼亚州的许可证审批瓶颈。

使用 CrossBeam，建筑商可以将他们的蓝图和更正函拖放到工具中；并行子代理解析文档，构建空间索引，并为每个单独的更正分配目标代理。二十分钟后，建筑商就获得了一份精确的批准行动计划。在桌子的另一边，CrossBeam 允许市政当局批量处理提交的许可证，并自动生成更正函草稿。

布埃纳帕克是南加州的一个城市，到 2029 年需要批准超过 8,900 套住房单元，但在 2024 年仅批准了约 120 套，该市正考虑采用 Crossbeam 来加快许可证审批流程，不仅为建筑商，也为审查堆积如山的文件的管理人员。

"如果我们能解决加利福尼亚州许可证危机双方的问题，"Mike 说，"我们也许真的能解决加利福尼亚州的住房危机。"

Mike 使用一个工作流程构建了 CrossBeam：向 Claude Code 发出提示，然后让 Claude 创建测试。"我最终赢得了这场比赛，这对我来说太疯狂了，而且我没有编写一行代码，"他说。"我甚至没有读过一行代码。"

[*在 GitHub 上查看 CrossBeam。*](https://github.com/mikeOnBreeze/cc-crossbeam)

## 第二名：[Elisa](https://www.youtube.com/watch?v=rsUaz_QAK6o)，Jon McBee

图片由 Jon McBee 提供。

当 Jon McBee 12 岁的女儿需要为她的七年级科学博览会项目编写微控制器程序时，他希望她能使用他作为软件工程师每天使用的相同工具：Claude Code。但终端界面并非为中学生设计的——所以他构建了一个适合中学生的界面。

Elisa 是一个基于块的集成开发环境 (IDE)，用户可以通过拼接基本元素（目标、需求、代理、技能、规则、门户、部署）来设计软件，同时人工智能在后台编写真正的代码。用户用可视化语言编写规范，元规划器将其分解为任务图，然后由代理处理其余部分。内置的教学引擎会提供适合年龄的、关于所使用编程概念的解释，将每次构建都变成一堂课。Jon 的女儿用它为她的项目将代码刷写到微控制器上，而没有编写一行代码。

使用 Claude Code，Jon 在 30 小时内构建了 Elisa，进行了 76 次提交，包含超过 39,000 行代码和超过 1,500 个测试。"我了解系统架构。我知道如何集成硬件。我知道如何定义和测试软件，"他说。"Claude Code 帮助我在短短六天内将所有这些知识转化为一个可交付的产品。"

教育工作者已经联系了 Jon，希望能在课堂上使用 Elisa，Jon 正努力用他获得的第二名奖金（价值 30,000 美元的 Claude API 积分）来资助这项工作。他相信软件创作者很快将完全停止编写源代码，转而使用定义明确的测试和规范进行工作。*Elisa* 将这个想法封装在一个适合儿童的可视化界面中。"我以我女儿的名字命名了这个项目，"Jon 说，"因为她正是这个项目的目标用户。"

[*在 GitHub 上查看 Elisa。*](https://github.com/zoidbergclawd/elisa)

## 第三名：[PostVisit.ai](https://youtu.be/V29UCOii2jE)，Michał Nedoszytko

图片由 Michał Nedoszytko 提供。

Michał Nedoszytko 是布鲁塞尔的一名心脏病专家，他在行医之余花了 20 年时间构建医疗保健软件。他之前的项目 Previsit.AI 是一个人工智能患者接诊系统，已在比利时、希腊和波兰部署。但他过去两年真正想构建的产品是其对应部分：预约后会发生什么。"我在导管室做过数千次手术，"他说。"但真正的挣扎始于你离开房间的那一刻。"

PostVisit 是一套工具，它用通俗易懂的语言解释诊断结果，分析就诊记录和人工智能听写转录文本，并在医生的监督下，从科学资源和完整的健康记录中提取相关的临床证据。患者可以更清楚地了解自己的护理情况，医生则可以了解患者在两次就诊之间的状况。该助手围绕隐私、安全和临床最佳实践而构建。

为了构建 PostVisit，Michał 踏上了一段黑客马拉松公路之旅，在从布鲁塞尔前往旧金山的途中进行构建。"在路上是我最佳创意诞生的地方，"他说。一周和几千英里之后，他拥有了自己多年来一直想象的产品。"医学基于证据，"他说。"现在，通过结合健康记录、证据和就诊数据，患者可以完全控制和理解就诊后发生的事情。"

[*在 postvisit.ai 了解更多信息。*](http://postvisit.ai)

## "持续思考"奖：[TARA](https://www.youtube.com/watch?v=GFCrXehS1DE)，Kyeyune Kazibwe

图片由 Kyeyune Kazibwe 提供。

乌干达的道路基础设施需求远远超出其预算能力，而传统的可行性研究也无济于事：它们耗资 100 万至 400 万美元，需要 9 到 14 个月，并且侧重于经济预测，而没有考虑道路所服务的社区。当时在乌干达工程和交通部工作的 Kyeyune Kazibwe 构建了 TARA，以改变这些决策的制定方式。

TARA 将行车记录仪的道路录像转化为完整的投资评估。该工具利用 Opus 4.6 的视觉能力分析每一帧画面，识别路面状况、损坏模式以及路边活动，包括行人、自行车和集市摊位。该系统将道路划分为不同的状况路段，自动填充干预成本，并生成完整的经济评估，包括净现值、现金流预测和敏感性分析。它还会生成一个公平性评分，评估谁真正从投资中受益，并考虑在行驶过程中识别出的附近设施和高关注区域。

为了参加黑客马拉松，Kyeyune 上传了乌干达目前正在建设的 Kira - Matugga 路的实际行车记录仪录像。"一键点击即可生成完整的 PDF 报告：状况评估、经济分析、公平性调查结果、敏感性解释，全部在一个文档中，"他说。"这个过程过去需要数周时间。TARA 在五小时内就能完成。"

[在 GitHub 上查看 *TARA。*](https://github.com/Kye256/tara-transport-assessment)

## 特别奖——创意探索：[Conductr](https://www.youtube.com/watch?v=X6CqJoyj0kI)，Asep Bagja Priandana

图片由 Asep Bagja Priandana 提供。

Asep Bagja Priandana 构建了 Conductr，将 Claude 变成一个实时的虚拟乐队伙伴。这个基于浏览器的 MIDI 乐器会监听您在控制器上弹奏的和弦，分析您的演奏，并实时生成四个音轨——鼓、贝斯、旋律和和声。输入 *"让它变得有放克味"* 或 *"推向高潮"*，编曲会在即兴演奏中发生变化。

他说，技术挑战在于保持音乐不中断。一个编译为 WebAssembly 的 C 语言引擎每 15 毫秒生成一次音符，因此人工智能的决策可以重塑编曲而不会中断流程。正如 Asep 所说，延迟是"音乐上不可察觉的"。

Conductr 运行在大约 4,800 行 JavaScript 和 WebAssembly 代码上：对于一个能够实时聆听、思考并与您一起演奏的乐器来说，这是一个精简的构建。

[*在 GitHub 上查看 Conductr。*](https://github.com/nanassound/conductr)

请继续关注我们从 Built with Opus 4.7 黑客马拉松中选出的获奖者的最新消息。

[***了解***](http://claude.com/community) ***我们的 Claude 社区项目，包括聚会、黑客马拉松等。***

‍
