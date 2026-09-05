---
title: "支持关于人们如何使用Claude的独立研究"
description: "---"
pubDate: "2026-08-26"
heroImage: "/post_img.png"
tags: ["anthropic-research"]
originalLink: "https://www.anthropic.com/research/enabling-independent-research"
---

# 支持关于人们如何使用Claude的独立研究

---

社会影响

# 支持关于人们如何使用Claude的独立研究

2026年8月26日


*今年早些时候，我们开展了一项试点项目，允许外部研究人员访问聚合的、真实的Claude使用数据。三个研究团队使用Anthropic Insights（我们的隐私保护分析工具）自行设计了研究方案；我们代表他们进行数据收集，而他们则进行独立的分析。在这篇文章中，我们分享了这些研究的高层结果以及我们在运行该试点项目中学到的经验。我们还为未来可能希望与我们合作的研究人员提供了一份[意向表达表格](https://docs.google.com/forms/d/e/1FAIpQLScujFA2mJXnLhrvE7wazD7drfHlfRmfnrjhMZRpeIFW_b6I7Q/viewform?usp=sharing&ouid=108982467493638405621)。*

确保向变革性AI的过渡顺利进行，需要理解其对人类和社会的影响。目前，关于AI真实世界交互的数据集中在少数实验室手中。我们认为，让更多数据广泛可用——供研究人员、政策制定者和公众使用——将是一件好事。

实验室之外的研究人员有两个选择。他们可以利用实验室发布的分析结果，这些分析反映了真实使用情况，但往往回答的是实验室自身的问题，而非他们自己的问题。或者，他们可以使用公共数据集，这些数据可以按自己的意愿进行研究，但偏向于更随意的使用场景，可能无法反映大多数人实际使用AI的方式。对于关于AI实际使用方式的独立研究而言，这两种选择都不够充分。

今年春天，我们试点了一个项目，让三家外部研究机构通过[Anthropic Insights](https://www.anthropic.com/research/clio)（原名为“Clio”）设计并运行他们自己的Claude使用数据研究。Anthropic Insights是我们自己的团队用来分析数百万次Claude对话使用模式的隐私保护工具。我们希望未来能扩大这一项目，因此我们还对所有与第三方研究人员共享的数据进行了额外的隐私审计，以验证我们的隐私保护措施是否有效（见[附录](https://www-cdn.anthropic.com/files/4zrzovbb/website/8a665c85eec3a63b4d86287b9255657016f50e29.pdf)）。

我们相信这是外部研究人员首次在AI公司自身的使用数据上开展公开的独立研究。下面，我们将讨论外部团队发现了什么、我们在运行试点项目中学到了什么，以及我们在决定如何更广泛地扩展该项目时所权衡的因素。我们还公开发布了[每个项目的聚合数据](https://huggingface.co/datasets/Anthropic/enabling-independent-research)。

## 研究人员发现了什么

我们与三个研究团队合作：斯坦福大学的[社会与语言技术（SALT）实验室](https://cs.stanford.edu/~diyiy/group.html)、牛津大学的[人类信息处理实验室](https://humaninformationprocessing.com/)以及评估前沿AI模型的非营利组织[METR](https://metr.org/)。每个团队都制定了各自的研究问题，并使用Anthropic Insights对2026年4月至5月期间约25万次Claude.ai或Claude Code对话进行了隐私保护分析。

我们希望外部合作伙伴拥有尽可能多的独立性，因此我们的合同审查权仅限于用户隐私、可能帮助人们违反我们使用政策的信息、Anthropic的机密信息以及研究准确性。除此之外，Anthropic对研究结果的内容没有发言权，研究人员可以自由发表他们的结果，即使这些结果对Anthropic不利。以下是部分早期结果。我们对这些研究方向感到兴奋，也期待现在数据公开后其他人能发现什么。

**社会与语言技术实验室**研究了人类如何与AI协作。他们考察了人们将哪些类型的工作交给AI、人类在完成这些工作时保留了什么角色，以及人机协作在哪些环节会出现问题。他们发现：

* **人们将高风险工作交给AI的比例超出预期。** 此前的研究表明，人们大多将低责任任务委托给AI，而将重要任务（即影响他人或难以撤销的工作）留给自己。但SALT实验室发现，超过一半的Claude对话涉及人们将重要任务委托给AI。人们在寻求专业指导时最有可能将重要工作交给Claude，尤其是在法律或财务问题上。
* **人们在与Claude协作时通常会进行指导和监督。** 在近四分之三的对话中，人们设定方向而Claude提供协助，并且人们通常会调整其输出而非直接使用。但即使在指导Claude产生所需输出时，人们对Claude所产生内容的理解和学习程度也各不相同。
* **人们在AI协作中遇到摩擦是常见现象。** 然而，这种摩擦往往是有成效的。人们花时间和精力去观察Claude如何尝试任务、识别请求中不清晰或被误解的地方，并反复调整他们的指示，最终会带来更好的结果——这促使人们澄清意图、完善输出或持续参与问题解决。

阅读他们的完整报告[此处](https://www.alphaxiv.org/abs/2608.human-ai-collaboration-at-scalev1)。

**人类信息处理实验室**正在研究人们在使用Claude时的感受，以及这种感受与Claude行为之间的关系。他们的早期结果表明：

* **人们使用AI时的感受与AI的行为相关。** 研究人员发现，对话中人类和AI的行为模式会同时出现：Claude表现温暖时，人们往往更加积极。Claude拒绝或持不同意见时，人们往往会反驳。Claude表现古怪时，人们往往在智力上更加投入。而Claude单纯提供帮助时，人们似乎感到满意。
* **人们使用AI的体验与在互联网其他部分上的体验非常相似。** 研究人员发现，Claude对话中沉浸感、挫败感和愉悦感等状态之间的模式，与另一项关于日常互联网浏览的研究中的模式高度相似，这表明人们与AI互动的方式与其他数字活动之间存在相似性。

他们仍在完成报告。公开后，我们将在此处添加链接。

**METR**正在估算编码代理带来的真实生产力提升，以及这些生产力增长如何随模型代际变化。他们对Claude Code对话的分析仍在进行中，但早期结果表明：

* **更强的模型可能为用户节省更多时间。** METR比较了Claude对没有AI时任务所需时间的估计与实际使用不同Claude模型所花费的时间。他们的初步发现表明，较新的模型相比旧模型带来了显著的加速效果。METR计划随着分析的深入分享更多内容。
* **AI可以相当准确地估计所需时间。** 由于该分析依赖于Claude判断任务所需时间，METR将这些判断与此前开发者研究中的已知完成时间进行了比较。Claude的估计与实际开发者所用时间相关。
* **下一步：衡量AI在研究方面的加速程度。** METR正在继续研究他们的分析如何为AI加速研究人员工作的程度提供洞见，随着[AI承担更多自身开发工作](https://www.anthropic.com/institute/recursive-self-improvement)，这可能变得越来越重要。

他们仍在完成报告。公开后，我们将在此处添加链接。

## 我们团队学到的经验

共享使用数据在AI领域几乎是前所未有的，因此这次试点既是运行此类项目的实验，也是以隐私保护方式支持第三方研究的手段。保护用户隐私和研究人员的独立性都至关重要，而我们都做到了。Anthropic Insights正是为此设计的——研究人员从未访问过原始对话，只访问经过与我们内部工作相同的法律和隐私审查后的聚合输出。然而，所有这些都使得试点项目相对于AI实验室正常的研究速度而言较为缓慢，且资源密集。这两个因素都对有效扩展该项目构成了挑战。关于我们如何运行该试点的更多细节，请参阅[附录](https://www-cdn.anthropic.com/files/4zrzovbb/website/8a665c85eec3a63b4d86287b9255657016f50e29.pdf)。下面我们讨论我们学到的经验以及如何应对出现的挑战。

**从不同角度研究同一问题是有价值的。** 我们一些合作伙伴的研究问题与内部正在进行的工作有所重叠。例如，METR的提案与我们关于“[代理式编码与专业技能的持续回报](https://www.anthropic.com/research/claude-code-expertise)”的经济学研究类似。我们发现这种重叠很有价值：它让外部研究人员有机会检查类似数据并得出自己的结论。这些结论是否与我们的一致，我们将在他们的研究继续时加以关注。我们还将METR与我们的经济学团队联系起来，发现这种联系改善了两个研究团队的工作。

**内部有效的研究方法需要为外部合作伙伴进行调整。** 使用Anthropic Insights时，研究人员会提出诸如“此人在寻求什么类型的指导？”这样的问题，Claude会为研究中的每次对话作答。答案随后被聚合为类别；研究人员只能看到最终类别以及每个类别下对话的百分比。由于我们依赖Claude的判断，该工具对问题的措辞很敏感；措辞不当的问题可能会将对话归入不具代表性的类别。由于没有人能阅读底层对话，这些错误很难被发现。

在内部，我们通过数周内反复迭代问题来管理这一过程。外部合作伙伴无法做到这一点，因为在共享每个数据集之前进行重复的隐私审查会使研究不可行。相反，我们让他们在WildChat（一个公开的人机对话数据集）上测试问题，他们可以在那里对照底层对话本身检查答案。但WildChat偏向于随意和创造性的使用，与Claude流量不同，因此一些在WildChat上表现良好的问题一旦应用于实际的Claude对话，就会产生误导性的类别。我们通过提供关于如何解读Anthropic Insight输出的指导来解决这一问题（见[附录](https://www-cdn.anthropic.com/files/4zrzovbb/website/8a665c85eec3a63b4d86287b9255657016f50e29.pdf)）。展望未来，我们正在探索外部研究人员如何更有效地提前开发他们的问题和类别。

**在保持透明度关于滥用行为的同时不助长其发生。** 我们的合作伙伴在Anthropic Insights输出中的某些类别暴露了违反我们可接受使用政策或服务条款的行为——例如，一个寻求禁止活动指导的人群类别。我们认为公众应该了解我们平台上的滥用情况，因此我们分享了这些违规行为中的大部分。例外情况是那些描述用户*如何*绕过我们的保障措施而非*尝试了什么*的类别。每项研究中受影响的类别和对话不到5%，在每种情况下，我们都告知研究人员我们更改或删除了哪些聚类以及原因。作为标准做法，当Anthropic Insights浮现此类违规行为时，我们会将聚合数据分享给我们的保障团队进行审查。这也是我们未来与外部研究人员合作的重要流程。

## 展望未来

理解AI对社会的影响，单靠AI公司自身是远远不够的。真正的监督需要外部研究人员基于真实世界的使用数据提出自己的问题，并独立发表他们的发现。

这次试点是一次实验：外部研究人员能否在不损害用户隐私的前提下，在我们的平台上开展独立研究？这项工作比我们预想的更具挑战性，我们也学到了很多，但到目前为止，答案似乎是肯定的。我们的合作伙伴开展了我们未曾想到要自行设计的研究，每个人都向我们揭示了AI在现实世界影响的新情况。这是充满希望的第一步，但还有更多工作要做。

下一步，我们要确定能否扩大这一项目的规模，既包括在上述约束条件下能支持哪些类型的研究，也包括能同时开展多少项研究。我们正稳步推进，以确保隐私、安全和研究质量。我们想了解研究人员的兴趣所在，以及他们希望研究什么。如果您是研究人员，并且访问Anthropic Insights能让您开展目前无法进行的工作，请填写[此表格](https://docs.google.com/forms/d/1PyyC-GU4Jra2WoXWcJjuFXZhM-lRxeBNnlh1tJv_2Jg/edit)。

## 附录

完整附录可[在此处获取](https://www-cdn.anthropic.com/files/4zrzovbb/website/8a665c85eec3a63b4d86287b9255657016f50e29.pdf)。其中描述了我们的项目运作方式，包括如何选择三位合作伙伴、我们撰写的研究入门指南（解释项目目标及Anthropic Insights的功能），以及每个项目如何从提案推进到研究设计再到分析。附录还包含我们合作协议的细节，其中明确说明，即使研究结果对Anthropic不利，合作伙伴也可自由发表。此外，我们还介绍了由伦敦帝国理工学院进行的第三方隐私审计，以及我们对所有发布数据所遵循的隐私威胁模型。同时，我们也提供了关于如何解读合作伙伴Anthropic Insights研究数据的指南。

## 贡献与致谢

Kunal Handa领导了该项目，与合作伙伴协作制定研究提案，起草研究指南和合同，运行合作伙伴的Anthropic Insights研究，帮助构建支持合作伙伴研究的技术基础设施，参与Anthropic Insights成果的内部审查，并撰写了这篇博客文章。Miranda Zhang协调了合作伙伴关系和沟通工作，并参与了项目的所有部分。Gabriel Nicholas协调了第三方隐私审计和Anthropic Insights成果的内部审查，并参与了项目的所有部分。Miles McCain撰写了关于解读Anthropic Insights成果的指南，开发了支持合作伙伴研究的技术基础设施，参与了Anthropic Insights成果的内部审查，并对博客文章和隐私威胁模型提供了反馈。Ryan Heller为支持合作伙伴的研究贡献了技术基础设施。Saffron Huang参与了Anthropic Insights成果的内部审查，并提供了关键反馈和讨论。Thomas Millar和Suzanne Wang为支持合作伙伴的研究和Anthropic Insights成果的内部审查贡献了技术基础设施。Shan Carter、Mo Julapalli、Matt Kearney、Sarah Pollack和Judy Shen参与了Anthropic Insights成果的内部审查。Matthew Jagielski为隐私威胁模型做出了贡献。Shaoyi Zhang为支持合作伙伴的研究贡献了技术基础设施。Heather Whitney、Ankur Rathi、Aisling Keenan和David Saunders在整个项目中提供了法律和隐私方面的指导。Jake Eaton和Sylvie Carr为博客文章的框架和撰写做出了贡献。Jack Clark和Michael Stern在整个过程中提供了宝贵的指导、支持和讨论。Deep Ganguli在项目的各个阶段提供了详细的指导、组织支持和反馈。

此外，我们还要感谢Miriam Chaum、Ishita Dasgupta、Esin Durmus、Adam Farina、Zoe Hitzig、Jerry Hong、Devin Kuokka、Hendson Lin、Maxim Massenkoff、Peter McCrory、Maryam Quasto、Nitarshan Rajkumar、Amie Rotherham、Divya Siddarth、Taylor Sorensen、Jerome Swannack、Alex Tamkin、Molly Villagra、Scott White和Charles Yang，感谢他们的宝贵想法、讨论、反馈和支持。

感谢以下合作伙伴对本项目的支持：斯坦福大学社会与语言技术实验室的Vishakh Padmakumar、Yijia Shao、Jennifer Wang、Diyi Yang和Dora Zhao；牛津大学人类信息处理实验室的Tsvetomira Dumbalska、Hannah Rose Kirk和Christopher Summerfield；以及METR的Joel Becker（现任职于Anthropic）、Daniel Paleka和Parker Whitfill。

感谢Zexi Yao、Bozhidar Stevanoski、Peter Romov、Euodia Dodd、Xiaoxue Yang和Nataša Krčo进行了第三方隐私审计。

## 引用

```
@online{handa2026enablingindependentresearch,
author = {Kunal Handa and Miranda Zhang and Gabriel Nicholas and Miles McCain and Ryan Heller and Saffron Huang and Thomas Millar and Suzanne Wang and Shan Carter and Mo Julapalli and Matt Kearney and Sarah Pollack and Judy Shen and Matthew Jagielski and Shaoyi Zhang and Heather Whitney and Ankur Rathi and Aisling Keenan and David Saunders and Jake Eaton and Sylvie Carr and Jack Clark and Michael Stern and Deep Ganguli},
title = {Enabling independent research on how people use Claude},
date = {2026-08-26},
year = {2026},
url = {www.anthropic.com/research/enabling-independent-research},
}
```

复制

## 相关内容

### Claude如何加速蛋白质设计与分析化学

在这篇文章中，我们分享了两项成果，展示Claude如何帮助生命科学家加快研究步伐。

[阅读更多](/research/Claude-accelerates-protein-design)

### 新兴多智能体系统中的模式与问题

在此，我们列举了当前前沿模型中一些行为倾向的例子，并展示它们如何导致意想不到的系统性故障，希望以此引发关于降低这些风险的讨论。

[阅读更多](/research/multiagent-systems)

### 审视工人再培训计划的证据

我们分享了一份关于工人再培训计划证据的审查报告，该报告由独立研究员David Roodman与Anthropic的Maxim Massenkoff共同撰写。

[阅读更多](/research/reviewing-the-evidence-on-worker-retraining-programs)
