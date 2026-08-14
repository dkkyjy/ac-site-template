---
title: "人工智能对劳动力市场的影响：一种新的衡量指标与初步证据"
description: "人工智能对劳动力市场的影响：一种新的衡量指标与初步证据"
pubDate: "2026-03-05"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/labor-market-impacts"
---

# 人工智能对劳动力市场的影响：一种新的衡量指标与初步证据
**标签：** 经济研究

---

[阅读 PDF 版本](https://cdn.sanity.io/files/4zrzovbb/website/2b5bbaf2c1eb81dbf6e6fb813c1a24e35a64d376.pdf)

## 主要发现

* 我们引入了一种衡量人工智能替代风险的新指标——*观测暴露度*，该指标结合了理论上的大语言模型能力与实际使用数据，并对自动化（而非增强型）及工作相关的使用场景赋予更高权重
* 人工智能远未达到其理论能力：实际覆盖范围仅为可行范围的一小部分
* 观测暴露度较高的职业，美国劳工统计局预测其到 2034 年的增长幅度较小
* 暴露度最高职业中的从业者更可能是年龄较大、女性、受教育程度较高且收入较高的人群
* 自 2022 年末以来，我们未发现高暴露度工人的失业率出现系统性上升，但发现了暗示性证据表明，在暴露度较高的职业中，年轻工人的招聘速度有所放缓

## 引言

人工智能的快速扩散催生了一波测量和预测其对劳动力市场影响的研究。但过往方法的记录让我们有理由保持谦逊。

例如，一项衡量工作可离岸外包性的著名研究曾认定美国约四分之一的工作面临风险，但十年过后，这些工作大多保持了健康的就业增长。政府自身的职业增长预测虽然方向正确，但在线性外推过去趋势之外并未增加多少预测价值。即便是事后回顾，重大经济冲击对劳动力市场的影响也往往不明确。关于工业机器人对就业影响的研究得出了相反的结论，而中国贸易冲击所导致的失业规模仍在争论之中。¹

在本文中，我们提出了一个理解人工智能对劳动力市场影响的新框架，并基于早期数据对其进行了检验，发现迄今为止人工智能对就业产生影响的证据有限。我们的目标是建立一套衡量人工智能如何影响就业的方法，并定期重复这些分析。这种方法无法捕捉人工智能可能重塑劳动力市场的每一种渠道，但通过在显著影响出现之前就奠定这一基础，我们希望未来的研究能够比事后分析更可靠地识别经济冲击。

人工智能的影响有可能是显而易见的。该框架在影响不明确时最为有用——并且可以在替代效应可见之前帮助识别最脆弱的工作。

## 反事实

当影响规模大且突然时，因果推断更为容易。新冠疫情及伴随的政策措施造成的经济冲击如此显著，以至于对于许多问题而言，复杂的统计方法都显得多余。例如，失业率在大流行初期急剧飙升，这让其他解释几乎没有了空间。

然而，人工智能的影响可能更像互联网或对华贸易，而非新冠疫情。其影响可能不会立即从总体失业数据中显现出来；贸易政策和商业周期等因素可能会干扰对趋势线的解读。

一种常见的方法是比较受人工智能影响程度不同（高或低）的工人、企业或行业之间的结果，以便将人工智能的影响与其他混杂因素分离开来。² 暴露度通常在任务层面定义：例如，人工智能可以批改作业，但不能管理课堂，因此教师的暴露度被认为低于那些整个工作都可以远程完成的工人。

我们的工作遵循这种基于任务的方法，纳入了理论人工智能能力和实际使用情况的度量，然后再汇总到职业层面。³

## 衡量暴露度

我们的方法结合了三个来源的数据。

1. [O\*NET 数据库](https://www.onetcenter.org/database.html)，该数据库列举了美国约 800 个独特职业相关的任务。
2. 我们自己的使用数据（根据 [Anthropic 经济指数](https://www.anthropic.com/economic-index) 测量）。
3. Eloundou 等人（2023）的任务级暴露度估计，该估计衡量大语言模型是否有可能将任务速度至少提高一倍。

Eloundou 等人的指标 β 以简单尺度对任务进行评分：如果一项任务能通过大语言模型单独将速度提高一倍，则得 1 分；如果需要基于大语言模型构建的额外工具或软件，则得 0.5 分；否则为 0 分。⁴

为什么实际使用可能达不到理论能力？一些理论上可行的任务可能由于模型限制而未在使用中出现。另一些任务可能因法律约束、特定软件要求、人工验证步骤或其他障碍而扩散缓慢。例如，Eloundou 等人将“授权药物续方并向药房提供处方信息”标记为完全暴露（β=1）。我们尚未观察到 Claude 执行此项任务，尽管该评估似乎是正确的——理论上它可以被大语言模型加速。

尽管如此，这些理论能力和实际使用的度量之间存在高度相关性。如图 1 所示，在前四期经济指数报告中观测到的任务中，有 97% 属于 Eloundou 等人评定为理论上可行（β=0.5 或 β=1.0）的类别。

**图 1：Claude 使用量按 Eloundou 等人任务暴露度评分的分布**本图显示了 Claude 使用量在不同理论人工智能暴露度评分的 O\*NET 任务中的分布。β=1（完全可由大语言模型单独完成）的任务占观测到的 Claude 使用量的 68%，而 β=0（不可行）的任务仅占 3%。Claude 使用数据来自前四期经济指数报告。

### 一种新的职业暴露度度量

我们的新度量——*观测暴露度*——旨在量化：在大语言模型理论上可以加速的那些任务中，哪些实际上在工作场景中出现了自动化使用？理论能力涵盖的任务范围要广泛得多。通过追踪这一差距如何缩小，观测暴露度能够提供经济变化初现时的洞察。

我们的度量定性地捕捉了我们预计与工作影响相关的几个 AI 使用方面。一项工作的暴露度较高，如果：

* 其任务理论上可由 AI 完成
* 其任务在 Anthropic 经济指数中使用量显著⁵
* 其任务在工作相关环境中执行
* 其具有相对较高的自动化使用模式或 API 实施比例
* 其受 AI 影响的任务在整个角色中占比较大⁶

我们在[附录](https://cdn.sanity.io/files/4zrzovbb/website/e5f77fc0e77c0185110b5e4b909602791ae76eae.pdf)中给出了数学细节。我们将理论上可由大语言模型完成的任务视为“已覆盖”，前提是它们在 Claude 流量中出现了足够的工作相关使用。然后我们根据任务的具体执行方式进行调整：完全自动化的实施获得全权重，而增强型使用获得半权重。最后，将任务级的覆盖度量按每个任务上花费的时间比例加权平均到职业层面。

图 2 显示了观测暴露度（红色）与 Eloundou 等人的 β（蓝色）的对比，说明了理论和实际在我们平台上的使用之间的差异，并按广泛的职业类别分组。我们首先按时间分数度量加权平均到职业层面，然后按总就业人数加权平均到职业类别。例如，β 度量显示大语言模型在计算机与数学（94%）以及办公室与行政（90%）职业中的大多数任务中具有渗透空间。

**图 2：按职业类别划分的理论能力和观测暴露度**大语言模型理论上能够完成的工作任务比例（蓝色区域）以及我们基于使用数据得出的工作覆盖度量（红色区域）。

红色区域描绘了来自 Anthropic 经济指数的 Claude 使用情况，展示了人们在工作场景中如何使用 Claude。覆盖度表明人工智能远未达到其理论能力。例如，Claude 目前仅覆盖了计算机与数学类别中所有任务的 33%。

随着能力提升、应用扩散和部署深化，红色区域将逐渐覆盖蓝色区域。还存在大量未覆盖区域；当然，许多任务仍然超出人工智能的触及范围——从修剪树木和操作农业机械等体力农业劳动，到代表客户出庭等法律任务。

图 3 显示了在此度量下暴露度最高的十个职业。与其他数据显示 Claude 广泛用于编码的情况一致，计算机程序员位居榜首，覆盖率达 75%，其次是客户服务代表，其主要任务我们越来越多地在第一方 API 流量中看到。最后，数据录入员的主要任务——读取源文件并输入数据——正在实现显著自动化，覆盖率为 67%。

**图 3：暴露度最高的职业**使用我们的任务覆盖度量，暴露度最高的十大职业。

在最低端，30% 的工人覆盖率为零，因为他们的任务在我们的数据中出现频率太低，未达到最低阈值。这一群体包括例如厨师、摩托车修理工、救生员、调酒师、洗碗工和更衣室服务员。

## 暴露度如何与预测的就业增长和工人特征相关联

美国劳工统计局（BLS）定期发布就业预测，最新一期于 2025 年发布，涵盖了 2024 年至 2034 年每个职业的[预测](https://data.bls.gov/projections/occupationProj)就业变化。在图 4 中，我们将我们职业层面的覆盖度量与这些预测进行了比较。

以当前就业人数加权的职业层面回归发现，观测暴露度较高的工作，其增长预测略弱。覆盖度每增加 10 个百分点，BLS 的增长预测就下降 0.6 个百分点。这在一定程度上验证了我们的度量与劳动力市场分析师的独立估计相吻合，尽管这种关系较弱。有趣的是，仅使用 Eloundou 等人的度量则不存在这种相关性。

**图 4：BLS 预测的 2024—2034 年就业增长 vs. 观测暴露度**包含 25 个等距分箱的分箱散点图。每个实心圆点显示一个分箱的平均观测暴露度和预测就业变化。虚线显示以当前就业水平加权的简单线性回归拟合。小菱形标出了作为示例的个别职业。

图 5 显示了暴露度最高四分位数的工人以及暴露度为零的工人（在 ChatGPT 发布前三个月，即 2022 年 8 月至 10 月）的特征，数据来自当前人口调查。⁷ 这些群体差异很大。暴露度较高的群体中，女性比例高出 16 个百分点，白人比例高出 11 个百分点，亚裔比例几乎翻倍。他们平均收入高出 47%，教育水平也更高。例如，拥有研究生学历的人在未暴露群体中占 4.5%，但在暴露度最高的群体中占 17.4%，相差近四倍。

**图 5：高暴露度与低暴露度工人之间的差异，当前人口调查**

## 结果优先级

有了这些暴露度度量，问题在于要关注什么。研究者们采取了不同的方法。例如，Gimbel 等人（2025）利用当前人口调查追踪职业结构的变化。他们的论点是，人工智能导致的任何重要经济重组都会表现为工作分布的变化。¹（他们发现，迄今为止，变化并不显著。）Brynjolfsson 等人（2025）利用薪资处理公司 ADP 的数据，按年龄组划分就业水平；而 Acemoglu 等人（2022）和 Hampole 等人（2025）则分别使用 Burning Glass（现为 Lightcast）和 Revelio 的职位发布数据。

我们将失业作为优先结果，因为它最直接地捕捉了经济损害的可能性——一个失业的工人想要工作但尚未找到。在这种情况下，职位发布和就业并不一定表明需要政策回应；高暴露度职位的职位发布下降可能会被相关职位的职位发布增加所抵消。人工智能最具危害性的劳动力市场发展，按理说应包含一段失业增加的时期，因为被替代的工人正在寻找替代方案。当前人口调查非常适合追踪这一点，因为失业受访者会报告他们之前的工作和行业。

## 初步结果

接下来，我们研究失业趋势，将我们的职业层面度量与当前人口调查中的受访者进行匹配。

解释我们的覆盖度量的一个关键问题是，哪些工人应被视为受影响群体？仅 10% 的任务覆盖度就应预期就业变化吗？Gans 和 Goldfarb（2025）表明，如果 O 型环模型最能描述工作，那么只有当所有任务都有一定程度的人工智能渗透时，才可能看到就业效应。Hampole 等人（2025）认为，平均暴露度会降低劳动力需求，但暴露度*集中*在特定任务上可以抵消这种效应。而 Autor 和 Thompson（2025）则强调了剩余任务所需的专业水平。

着眼于简洁性，并注意到我们最关心的是重大影响，我们将分析集中于这样一个想法：影响应该在最暴露的群体中感受最深。我们将时间加权任务覆盖度最高四分位数的工人与最低四分位数的工人进行比较。如果人工智能能力快速进步，较低覆盖度百分位数可能也会很高，这可能使绝对阈值更有用。但我们假设影响应首先作用于最暴露的工人，并展示了定义处理组时改变截断值的结果。

图 6 的上方面板显示了自 2016 年以来，暴露度最高四分位数工人和未暴露群体的失业率原始趋势。在疫情期间，人工智能暴露度较低的工人——更可能从事面对面工作——失业率上升幅度更大。此后，两组之间的趋势大致相似。下方面板在双重差分框架中衡量了最暴露和最不暴露工人之间的差距大小，反映了原始数据中的发现。自 ChatGPT 发布以来，该差距的平均变化很小且不显著，表明较暴露群体的失业率略有上升，但该效应与零无显著差异。⁸

**图 6：观测暴露度最高四分位数工人和无人工智能暴露工人的失业率趋势，当前人口调查**上方面板显示了暴露度最高四分位数工人（红线）和暴露度为零的 30% 工人（蓝线）的失业率。下方面板在双重差分框架中衡量了这两个序列之间的差距。

这个框架能够识别什么样的情景？基于合并估计的置信区间，大约 1 个百分点的失业率差异增加将是可检测的（随着新数据的加入，这可能会变化，因此仅是一个粗略估计）。如果前 10% 的工人全部被裁，最高四分位数群体内的失业率将从 3% 升至 43%，总体失业率将从 4% 升至 13%。

一个较小但仍令人担忧的影响是“白领大衰退”情景。在 2007-2009 年大衰退期间，美国失业率从 5% 翻倍至 10%。暴露度最高四分位数群体中这样的翻倍将使其失业率从 3% 升至 6%。这也应在我们的分析中可见。注意，我们的核心估计基于暴露群体与较低暴露群体之间失业率的*差异*变化。如果所有工人的失业率平行上升，我们不会将其归因于仍然留下许多任务未受影响的人工智能进步。

一个特别值得关注的群体是年轻工人。Brynjolfsson 等人报告称，在暴露职业中，22 至 25 岁工人的就业下降了 6-16%。他们将这一下降主要归因于招聘放缓而非离职增加。⁹

我们发现，暴露职业中年轻工人的失业率保持平稳（见[附录](https://cdn.sanity.io/files/4zrzovbb/website/e5f77fc0e77c0185110b5e4b909602791ae76eae.pdf)）。但招聘放缓不一定表现为失业率上升，因为许多年轻工人是劳动力市场新进入者，在 CPS 数据中没有列出职业，并且可能退出劳动力市场而不是显示为失业。为了直接处理招聘问题，我们利用 CPS 的面板维度，计算在更暴露与更不暴露的职业中开始新工作的年轻工人（22-25 岁）的百分比随时间的变化。图 7 显示了年轻工人的月度工作找到率（即工人报告了一份上个月没有的工作），按他们进入高暴露度还是低暴露度职业进行划分。

**图 7：22-25 岁工人在观测暴露度高和无人工智能暴露职业中的新工作开始情况，当前人口调查**上方面板显示了年轻工人在高暴露度与无暴露度职业中开始新工作的百分比。下方面板在双重差分框架中衡量了这两个序列之间的差距。

除了 2020-2021 年的一些大幅波动外，这些序列在 2024 年发生视觉上的偏离，年轻工人相对不太可能被招聘到暴露度高的职业。低暴露度职业的工作找到率保持在每月 2%，而进入最暴露工作的比率下降了约 0.5 个百分点。在 ChatGPT 时代之后，平均估计显示暴露职业的工作找到率相比 2022 年下降了 14%，尽管这只是勉强具有统计显著性。（对于 25 岁以上的工人，没有这样的下降。）

这可能提供了人工智能对就业早期影响的一些信号，并且与 Brynjolfsson 等人的发现相呼应。但存在若干其他解释。未被招聘的年轻工人可能留在现有工作、接受其他工作或返回学校。还有一个与数据相关的注意事项是，工作转换可能在调查中更容易被错误测量。¹⁰

## 讨论

本报告介绍了一种衡量人工智能对劳动力市场影响的新度量，并研究了其对失业和招聘的影响。工作越容易受到人工智能影响，其任务就越能在理论上由大语言模型完成，并且在我们平台上以自动化、工作相关的用例被观察到。我们发现，计算机程序员、客户服务代表和财务分析师是暴露度最高的职业之一。利用美国的调查数据，我们未发现最暴露职业工人的失业率受到影响，尽管有初步证据表明，22-25 岁工人进入这些职业的招聘略有放缓。

我们的工作是迈向记录人工智能对劳动力市场影响的第一步。我们希望本报告中所采取的分析步骤，特别是关于覆盖度和反事实的步骤，能够随着就业和人工智能使用的新数据出现而易于更新。一个成熟的方法可能有助于未来观察者区分信号与噪声。

当前工作还有若干改进空间。我们的使用数据将在未来更新中纳入，形成经济中任务和工作覆盖的演变图景。Eloundou 等人的指标也可以更新，因为其与 2023 年初的大语言模型能力挂钩。鉴于关于年轻工人和劳动力市场新进入者的提示性结果，下一个关键步骤可能是研究近期在暴露领域拥有教育资格的毕业生如何导航劳动力市场。

## 附录

可在此[获取](https://cdn.sanity.io/files/4zrzovbb/website/e5f77fc0e77c0185110b5e4b909602791ae76eae.pdf)。

### 致谢

作者：Maxim Massenkoff 和 Peter McCrory。

感谢：Ruth Appel、Tim Belonax、Keir Bradwell、Andy Braden、Dexter Callender III、Miriam Chaum、Madison Clark、Jake Eaton、Deep Ganguli、Kunal Handa、Ryan Heller、Lara Karadogan、Jennifer Martinez、Jared Mueller、Sarah Pollack、David Saunders、Carl De Torres、Kim Withee 和 Jack Clark。

我们同时感谢 Martha Gimbel、Anders Humlum、Evan Rose 和 Nathan Wilmers 对本报告早期版本的反馈。

### 引用

```
@online{massenkoffmccrory2026labor,
 author = {Maxim Massenkoff and Peter McCrory},
 title = {Labor market impacts of AI: A new measure and early evidence},
 date = {2026-03-05},
 year = {2026},
 url = {https://www.anthropic.com/research/labor-market-impacts},
}
```

复制

## 参考文献

Acemoglu, Daron and Pascual Restrepo, "Robots and Jobs: Evidence from US Labor Markets," *Journal of Political Economy*, 2020, 128 (6), 2188–2244.

Acemoglu, Daron, David Autor, Jonathon Hazell, and Pascual Restrepo, "Artificial intelligence and jobs: Evidence from online vacancies," *Journal of Labor Economics*, 2022, 40 (S1), S293–S340.

Appel, Ruth, Maxim Massenkoff, Peter McCrory, Miles McCain, Ryan Heller, Tyler Neylon, and Alex Tamkin, "Anthropic Economic Index report: economic primitives," 2026.

Autor, David H, David Dorn, and Gordon H Hanson, "The China syndrome: Local labor market effects of import competition in the United States," *American Economic Review*, 2013, 103 (6), 2121–2168.

Autor, David H, & Thompson, N. (2025). Expertise. NBER Working Paper, (w33941).

Blinder, Alan S et al., "How many US jobs might be offshorable?," *World Economics*, 2009, 10 (2), 41.

Borusyak, Kirill, Peter Hull, and Xavier Jaravel, "Quasi-experimental shift-share research designs," *The Review of Economic Studies*, 2022, 89 (1), 181–213.

Brynjolfsson, Erik, Bharat Chandar, and Ruyu Chen, "Canaries in the coal mine? six facts about the recent employment effects of artificial intelligence," *Digital Economy*, 2025.

Eckhardt, Sarah and Nathan Goldschlag, "AI and Jobs: The Final Word (Until the Next One)," Economic Innovation Group (EIG), August 2025. Available at: <https://eig.org/ai-and-jobs-the-final-word/>

Eloundou, Tyna, Sam Manning, Pamela Mishkin, and Daniel Rock, "Gpts are gpts: An early look at the labor market impact potential of large language models," arXiv preprint arXiv:2303.10130, 2023, 10.

Fujita, S., Moscarini, G., & Postel-Vinay, F. (2024). Measuring employer-to-employer reallocation. *American Economic Journal: Macroeconomics*, 16(3), 1-51.

Gans, Joshua S. and Goldfarb, Avi, "O-Ring Automation," NBER Working Paper No. 34639, December 2025. Available at SSRN: <https://ssrn.com/abstract=5962594>

Gimbel, Martha, Molly Kinder, Joshua Kendall, and Maddie Lee, "Evaluating the Impact of AI on the Labor Market: Current State of Affairs," Research Report, The Budget Lab at Yale, New Haven, CT October 2025. Available at: <https://budgetlab.yale.edu>.

Graetz, Georg and Guy Michaels, "Robots at Work," *Review of Economics and Statistics*, 2018, 100 (5), 753–768.

Hampole, Menaka, Dimitris Papanikolaou, Lawrence DW Schmidt, and Bryan Seegmiller, "Artificial intelligence and the labor market," Technical Report, National Bureau of Economic Research 2025.

Handa, Kunal, Alex Tamkin, Miles McCain, Saffron Huang, Esin Durmus, Sarah Heck, Jared Mueller, Jerry Hong, Stuart Ritchie, Tim Belonax, Kevin K. Troy, Dario Amodei, Jared Kaplan, Jack Clark, and Deep Ganguli, "Which Economic Tasks are Performed with AI? Evidence from Millions of Claude Conversations," 2025.

Hui, Xiang, Oren Reshef, and Luofeng Zhou, "The short-term effects of generative artificial intelligence on employment: Evidence from an online labor market," *Organization Science*, 2024, 35 (6), 1977–1989.

Johnston, Andrew and Christos Makridis, "The labor market effects of generative AI: A difference-in-differences analysis of AI exposure," Available at SSRN 5375017, 2025.

Massenkoff, Maxim, "How predictable is job destruction? Evidence from the Occupational Outlook," 2025. *Working Paper.*

Ozimek, Adam, "Overboard on Offshore Fears," 2019. <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3777307>

Tamkin, Alex and Peter McCrory, "Estimating AI productivity gains from Claude conversations," 2025.

Tomlinson, K., Jaffe, S., Wang, W., Counts, S., & Suri, S. (2025). Working with AI: measuring the applicability of generative AI to occupations. arXiv preprint arXiv:2507.07935.

## 脚注

1. 工作可离岸外包性：Blinder 等人（2009）和 Ozimek（2019）；政府增长预测：Massenkoff（2025）；机器人：Graetz 和 Michaels（2018）以及 Acemoglu 和 Restrepo（2020）；中国冲击：Autor 等人（2013）以及 Borusyak 等人（2022）。
2. Brynjolfsson 等人（2025）比较了 AI 暴露程度较高与较低职业中工人的就业趋势，使用了 Eloundou 等人（2023）的任务暴露度量以及 ADP 的工资数据。Johnston 和 Makridis（2025）使用美国行政数据进行了类似的基于任务的分析，但他们将处理汇总到行业层面。Hui 等人（2024）研究了 Upwork 上的自由职业工作如何响应 ChatGPT 和高级图像生成工具的发布，比较了直接受影响类别与未受影响类别中工人在每次工具发布前后的情况。Hampole 等人（2025）利用历史大学招聘网络作为企业采用 AI 的工具变量：那些历史上从毕业生后来进入 AI 相关岗位的大学招聘的企业，面临着较低的采用成本。
3. 我们的任务级和职业级暴露度量可以轻松纳入其他使用数据，并扩展到不同国家。我们打算随着时间的推移将此方法应用于新的环境。
4. 在他们的框架中，“直接暴露”的任务是指那些使用大语言模型可以在一半时间内完成的任务（输入限制为 2000 词，且无法访问最新事实）。“借助工具暴露”的任务是指那些在相同加速条件下，大语言模型可以访问用于信息检索和图像处理等软件的任务。未暴露的任务无法通过使用大语言模型将其持续时间减少 50% 或更多。
5. 我们使用了前两期 Anthropic 经济指数数据集，覆盖 2025 年 8 月和 11 月的使用情况。对于语义高度相似的 ONET 任务，我们将计数在它们之间进行分配。
6. 每一步都涉及判断。Eloundou 等人（2023）的度量是否应取 {0, 0.5, 1} 或其他值？什么构成“显著”使用？如何处理那些与高使用率任务非常相似但因太罕见而未在经济指数抽样中具体捕捉到的任务？自动化工作流应比增强型工作流获得多少额外权重？一个令人安心的发现（附录中会有更多阐述）是，在这些问题的许多不同解决方法下，工作暴露度的斯皮尔曼（秩-秩）相关性极高。
7. 为了将 O\*NET-SOC 代码与 CPS 中的 occ1990 代码匹配，我们使用了 [Eckhart 和 Goldschlag（2025）](https://eig.org/ai-and-jobs-the-final-word/) 提供的交叉对照表。
8. 我们在[附录](https://cdn.sanity.io/files/4zrzovbb/website/e5f77fc0e77c0185110b5e4b909602791ae76eae.pdf)中通过三种方式进一步探讨了这一点。首先，我们询问用于定义处理的分位数截断点是否重要，从中位数到第 95 百分位数进行了变化。在所有情况下，影响都是持平或负的（意味着暴露群体的失业率下降）。接下来，我们特别关注年轻工人，即如 Brynjolfsson 等人（2025）中所指的 22 至 25 岁工人。最后，我们使用劳工部的失业保险索赔数据来衡量失业，而非 CPS 调查回复。在任何扩展中，我们都没有发现对暴露工作有明显影响。
9. 这个范围很宽，因为作者提供了针对多个反事实的估计。6 个百分点的下降是与就业增长持平的反事实相比较。16 个百分点的估计来自一项比较同一公司内具有不同职业的相似工人的设计。
10. 参见 Fujita 等人（2024）。

### 更正

*更新于 2026 年 3 月 8 日：更正了图 7，该图错误地颠倒标注了最高四分位数和零暴露组流入率。*

## 相关内容

### 语言模型中的全局工作空间

新的可解释性研究揭示了 Claude 中一个新兴的心理工作空间，它容纳了模型输出中未出现的内部想法。

[阅读更多](/research/global-workspace)

### Anthropic 经济指数报告：节奏

在我们最新的经济指数报告中，我们首次按小时采样，以回答：人们何时使用 Claude？他们用它产生什么？以及他们如何看待人工智能对其工作的影响？

[阅读更多](/research/economic-index-june-2026-report)

### Project Fetch：第二阶段

我们报告了最新测试的结果，测试了 Claude 是否可以帮助 Anthropic 员工执行复杂的机器人任务。我们发现，Claude Opus 4.7 在无人协助的情况下运行，在不到一年前参与者完成的所有任务中，速度大约是最快人类团队的 20 倍。

[阅读更多](/research/project-fetch-phase-two)
