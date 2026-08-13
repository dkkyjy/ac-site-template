---
title: "借助 Claude Opus 4.6 推动金融发展"
description: "[Claude Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 标志着人工智能在金融领域迈出的一步。它可用于帮助专业人士基于准确信息和清晰分析做出决策，并能生成真正精良的交付成果。该模型在金融推理、多任务处理以及保持对较长多步骤任务的专注力方面，显著优于市场上的其他模型。"
pubDate: "2026-02-05"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/opus-4-6-finance"
---
            [Claude Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 标志着人工智能在金融领域迈出的一步。它可用于帮助专业人士基于准确信息和清晰分析做出决策，并能生成真正精良的交付成果。该模型在金融推理、多任务处理以及保持对较长多步骤任务的专注力方面，显著优于市场上的其他模型。

与 Claude Opus 4.6 一同，我们正在更新一些现有产品——并推出一款新产品——以将这些能力部署到分析师花费大部分时间的工作场景中。[Cowork](https://claude.com/product/cowork) 现在能在首次生成时交付更精良的输出，例如财务模型和演示文稿。[Claude in Excel](https://claude.com/claude-in-excel) 现在能更好地处理长时间运行的任务，随着财务模型变得更加复杂，Claude Opus 4.6 能保持专注和准确。同时，我们正在发布 [Claude in PowerPoint](https://claude.com/claude-in-powerpoint) 作为研究预览版（测试阶段），用于原生构建和迭代幻灯片及演示文稿。

我们的内部真实世界金融评估测试了 Claude 在约 50 个投资和金融分析用例上的表现，涵盖电子表格、幻灯片以及文档生成和审阅。这些是分析师在投资银行、私募股权、公共投资和企业金融中常执行的任务。Claude Opus 4.6 相比几个月前我们最先进的模型 Claude Sonnet 4.5 提升了超过 23 个百分点。

*此评估测试了代码执行和工具使用代理框架的组合，并根据衡量金融领域知识、任务完整性和准确性以及演示文稿质量的评分标准和偏好进行评分。*

这些更新共同使 Claude 成为金融服务和企业金融领域从业者更强大的合作伙伴。

## 研究、分析、创作

金融专业人士使用人工智能跨多个数据源进行有效研究，支持金融分析，并创建其团队和客户可据此行动的交付成果。Claude Opus 4.6 在这三个维度上均属同类最佳。

在研究方面，Claude Opus 4.6 在 BrowseComp 和 DeepSearchQA 两个基准测试上均有提升，这两个基准测试评估模型从大型非结构化数据源中提取特定信息的能力。在实践中，这意味着用户可以向 Claude 提供一组密集的文档，并收到一个具体、有针对性的答案，而不是简单的摘要。

在分析方面，Claude Opus 4.6 在 [Finance Agent](https://www.vals.ai/benchmarks/finance_agent) 上达到 60.7% 的最先进水平（相比 Opus 4.5 提升了 5.47%），这是 Vals AI 的一个外部基准测试，评估模型对上市公司 SEC 文件的研究能力。Opus 4.6 在 Vals AI 的 [TaxEval](https://www.vals.ai/benchmarks/tax_eval_v2) 上也以 76.0% 的成绩达到最先进水平。

在创作方面，除了我们的真实世界金融评估外，我们还使用 GDPval-AA 来衡量 Claude 在复杂知识工作上的表现。借助 Claude Opus 4.6，像电子表格和演示文稿这样的结构化输出在首次生成时更常是正确的。下面的并排输出展示了输出质量从 Claude Opus 4.5 到 Opus 4.6 的提升。这些是 Claude 在商业尽职调查任务（评估潜在收购）中首次生成性能的示例——这类工作通常需要高级分析师两到三周才能完成。

> "借助 Claude Opus 4.6，过去需要数小时才能创建的金融 PowerPoint 现在只需几分钟。我们在细节关注度、空间布局和内容结构化方面看到了切实的改进。" - **Aabhas Sharma，Hebbia 首席技术官**

> "Claude Opus 4.6 的性能提升几乎令人难以置信。对 Opus [4.5] 来说具有挑战性的现实世界任务突然变得简单。这感觉像是 Shortcut 上电子表格代理的一个分水岭时刻。" - **Nico Christie，Shortcut AI 联合创始人兼首席技术官**

## 更好的多任务处理和初稿

Claude Opus 4.6 的金融能力可以通过 Cowork 轻松访问，这是我们在桌面应用中[使用 Claude 的新方式](https://claude.com/blog/cowork-research-preview)。

在 Cowork 中，你让 Claude 访问你选择的桌面文件夹。Claude 能够直接在该文件夹中读取、编辑和创建新文件。对于金融团队来说，这意味着你可以同时启动多个分析，同时引导 Claude 的思考过程，使其创建的每个交付成果符合你的标准。

Cowork 还可以[通过插件](https://claude.com/blog/cowork-plugins)进行定制——插件是技能包（指定如何完成任务）和与其他平台数据连接的组合。例如，通过[我们的企业金融插件](https://claude.com/plugins/finance)，Claude 立即知道如何完成常见工作流程，如日记账分录、差异分析和对账。你也可以[构建自己的插件](https://support.claude.com/en/articles/13345190-getting-started-with-cowork)来匹配你喜欢的工作方式。

Cowork [作为仅限桌面的研究预览版（测试阶段）](https://support.claude.com/en/articles/13345190-getting-started-with-cowork)在所有付费 Claude 计划中可用¹。

## 无需离开电子表格即可深入分析

Claude in Excel 将 Claude Opus 4.6 直接带入你的电子表格。我们现在使其在规划和澄清用户假设方面做得更好，尤其是当任务变得更加复杂时。它现在还支持数据透视表编辑、图表修改、条件格式、排序和筛选、数据验证以及金融级格式设置。

最后，我们增加了可用性改进，包括长对话的自动压缩以及拖放多文件支持。这意味着你将大大减少在选项卡之间复制和粘贴的工作量。你可以与 Claude 一起处理从财务模型到客户就绪工作簿的一切事务，全部在一个地方完成。

> "由 Claude Opus 4.6 驱动的 Claude in Excel 代表了一次重大飞跃。从尽职调查到财务建模，它被证明是我们团队非常强大的工具——获取非结构化数据并以最少的提示智能地工作，从而有意义地自动化复杂分析。这是一个极好的例子，展示了人工智能以切实、节省时间的方式增强投资专业人士的能力。" - **Lloyd Hilton，Hg Catalyst 负责人**

> "作为加拿大最大的机构投资者之一，我们不断创新，并将人工智能视为塑造我们未来的前沿。Claude Opus 4.6 增强的速度、精度以及处理复杂任务（如 Claude in Excel 中的多标签分析）的能力，为我们如何工作解锁了令人兴奋的可能性。" - **Ben Letalik，BCI 数字化转型与创新高级总监**

## 直接与 Claude 一起完善你的演示文稿

我们还推出了 Claude in PowerPoint 作为研究预览版（测试阶段）。就像 Claude in Excel 一样，这会将 Claude 引入你的 PowerPoint 侧边栏，使其能够读取你现有的布局、字体和母版，然后在线内创建新内容。Claude 可以根据客户模板构建幻灯片组，对现有幻灯片进行有针对性的编辑，并从零开始生成出色的首次生成演示文稿。

Claude in PowerPoint 现已作为研究预览版面向所有 Max、Team 或 Enterprise 计划用户提供。

## 入门指南

Claude Opus 4.6 和我们最新的产品更新使一系列全新的任务成为可能。但用于金融的人工智能仍然是一个活跃的前沿领域。用户应继续审阅 Claude 的输出，以确保其符合规格；特别是对于高风险工作，人类判断仍然至关重要。随着我们继续改进 Claude 的能力，我们的目标是为金融行业专业人士提供更强大的研究和分析工具，并帮助他们专注于最重要的工作。

Claude Opus 4.6、Cowork 和 Claude in Excel 在所有付费 Claude 计划中均可使用。要了解有关 Claude in Excel 的更多信息，请探索我们的[指南](https://support.claude.com/en/articles/12650343-claude-in-excel)和[视频教程](https://claude.com/resources/tutorials/getting-started-with-claude-in-excel)，并[在此处开始使用](https://claude.com/claude-in-excel)。Claude in PowerPoint 在所有 Max、Team 和 Enterprise 用户的研究预览版中可用，你可以[在此处开始使用](https://claude.com/claude-in-powerpoint)。

要了解组织如何实际使用这些新功能，请[注册我们的网络研讨会](https://anthropic.com/webinars/claude-in-excel-and-powerpoint)。

‍

###### *Cowork [作为仅限桌面的研究预览版](https://support.claude.com/en/articles/13345190-getting-started-with-cowork)在所有付费 Claude 计划中可用，从 Mac 开始（Windows 即将推出）。*

‍

‍

‍
