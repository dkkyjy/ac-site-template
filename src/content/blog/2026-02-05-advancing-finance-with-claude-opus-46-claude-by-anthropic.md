---
title: "借助 Claude Opus 4.6 推动金融业发展 | Claude by Anthropic"
description: "---"
pubDate: "2026-02-05"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/opus-4-6-finance"
---

# 借助 Claude Opus 4.6 推动金融业发展 | Claude by Anthropic

---

[Claude Opus 4.6](https://www.anthropic.com/news/claude-opus-4-6) 标志着人工智能在金融领域迈出了一步。它可以帮助专业人士基于准确的信息和清晰的分析做出决策，并产出真正精良的交付成果。该模型在金融推理、多任务处理以及在更长的多步骤任务中保持专注方面，明显优于市场上的其他模型。

与 Claude Opus 4.6 一同，我们正在更新一些现有产品——并推出一款新产品——将这些能力部署到分析师花费大部分时间的地方。[Cowork](https://claude.com/product/cowork) 现在能在首次生成时提供更精良的输出，例如财务模型和演示文稿。[Claude in Excel](https://claude.com/claude-in-excel) 现在更擅长处理长时间运行的任务，随着财务模型变得更加复杂，Claude Opus 4.6 能保持专注和准确。此外，我们正在以研究预览的形式在测试版中发布 [Claude in PowerPoint](https://claude.com/claude-in-powerpoint)，用于原生构建和迭代幻灯片及演示文稿。

我们的内部真实世界金融评估衡量 Claude 在约 50 个投资和财务分析用例中的表现，涵盖电子表格、幻灯片和 Word 文档的生成与审阅。这些是分析师在投资银行、私募股权、公开投资和企业金融中常见的工作。Claude Opus 4.6 相比几个月前我们最先进的模型 Claude Sonnet 4.5，提升了超过 23 个百分点。

*该评估测试了代码执行和工具使用的代理化框架的组合，并根据结合了评分标准和偏好的标准进行评分，以衡量金融领域知识、任务完整性和准确性以及演示质量。*

这些更新共同使 Claude 成为金融服务和企业金融领域更强大的合作伙伴。

## 研究、分析、创作

金融专业人士使用人工智能跨多个数据源进行有效研究、支持财务分析，并创建其团队和客户可以据此行动的交付成果。Claude Opus 4.6 在这三个维度上均处于同类最佳水平。

在研究方面，Claude Opus 4.6 在 BrowseComp 和 DeepSearchQA 上均有提升，这两个基准测试模型从大型非结构化数据源中提取特定信息的能力。在实践中，这意味着用户可以将大量文档交给 Claude，并获得具体、有针对性的答案，而非简单的摘要。

在分析方面，Claude Opus 4.6 在 [Finance Agent](https://www.vals.ai/benchmarks/finance_agent) 上达到 60.7% 的业界领先水平（较 Opus 4.5 提升 5.47%），这是 Vals AI 的一个外部基准，用于评估模型对上市公司 SEC 文件的研究能力。Opus 4.6 在 Vals AI 的 [TaxEval](https://www.vals.ai/benchmarks/tax_eval_v2) 上也以 76.0% 的成绩达到业界领先。

在创作方面，除了我们的真实世界金融评估外，我们还使用 GDPval-AA 来衡量 Claude 在复杂知识工作上的表现。借助 Claude Opus 4.6，电子表格和演示文稿等结构化输出在首次生成时更常是正确的。下面的并排输出展示了从 Claude Opus 4.5 到 Opus 4.6 输出质量的提升。这些是 Claude 在商业尽职调查任务（评估潜在收购）中的首次生成表现示例——这类工作通常需要高级分析师两到三周才能完成。

> “借助 Claude Opus 4.6，创建过去需要数小时的金融 PowerPoint 现在只需几分钟。我们在细节关注、空间布局和内容结构方面看到了切实的改进。” - **Aabhas Sharma，Hebbia 首席技术官**

> “Claude Opus 4.6 的性能飞跃几乎令人难以置信。过去对 Opus [4.5] 具有挑战性的真实任务突然变得容易了。这感觉像是 Shortcut 上电子表格代理的一个分水岭时刻。” - **Nico Christie，Shortcut AI 联合创始人兼首席技术官**

## 更好的多任务处理和初稿质量

Claude Opus 4.6 的金融能力可以通过 Cowork 轻松访问，这是我们在桌面应用中 [使用 Claude 的新方式](https://claude.com/blog/cowork-research-preview)。

在 Cowork 中，你可以让 Claude 访问你选择的桌面文件夹。Claude 能够直接读取、编辑和在该文件夹中创建新文件。对于金融团队来说，这意味着你可以同时启动多项分析，同时引导 Claude 的思考过程，使其创建的每个交付成果都符合你的标准。

Cowork 还可以 [通过插件进行定制](https://claude.com/blog/cowork-plugins)——插件是技能（指定如何完成任务）和与其他平台数据连接的捆绑包。例如，使用 [我们的企业金融插件](https://claude.com/plugins/finance)，Claude 能立即知道如何完成常见工作流程，如日记账分录、差异分析和对账。你也可以 [构建自己的插件](https://support.claude.com/en/articles/13345190-getting-started-with-cowork) 以匹配你的工作方式。

Cowork [现已可用](https://support.claude.com/en/articles/13345190-getting-started-with-cowork)，作为仅限桌面的研究预览，在所有付费 Claude 计划中以测试版形式提供1。

## 无需离开电子表格即可深入工作

Claude in Excel 将 Claude Opus 4.6 直接带入你的电子表格。我们现在使其更擅长与用户进行规划和澄清假设，尤其是在任务变得更加复杂时。它还支持数据透视表编辑、图表修改、条件格式、排序和筛选、数据验证以及金融级格式设置。

最后，我们增加了可用性改进，包括长对话的自动压缩和拖放多文件支持。这意味着你不再需要在标签页之间大量复制粘贴。你可以在一个地方与 Claude 处理从财务模型到客户就绪工作簿的所有工作。

> “由 Claude Opus 4.6 驱动的 Claude in Excel 代表了一次重大飞跃。从尽职调查到财务建模，它已被证明是我们团队极其强大的工具——获取非结构化数据，以最少的提示智能处理，从而有意义地自动化复杂分析。这是人工智能以切实、节省时间的方式增强投资专业人士能力的绝佳示例。” - **Lloyd Hilton，Hg Catalyst 负责人**

> “作为加拿大最大的机构投资者之一，我们不断创新，并看到人工智能处于塑造我们未来的前沿。Claude Opus 4.6 增强的速度、精度和处理复杂任务（如 Claude in Excel 中的多标签页分析）的能力，为我们的工作方式解锁了令人兴奋的可能性。” - **Ben Letalik，BCI 数字化转型与创新高级总监**

## 直接与 Claude 完善你的演示文稿

我们还以研究预览的形式在测试版中推出 Claude in PowerPoint。与 Claude in Excel 类似，它将 Claude 带入你的 PowerPoint 侧边栏，使其能够读取你现有的布局、字体和母版，然后直接创建新内容。Claude 可以从客户模板构建演示文稿，对现有幻灯片进行有针对性的编辑，并从零开始生成出色的首次演示文稿。

Claude in PowerPoint 现以研究预览形式向所有 Max、Team 或 Enterprise 计划用户提供。

## 开始使用

Claude Opus 4.6 和我们最新的产品更新使一系列全新任务成为可能。但金融领域的人工智能仍是一个活跃的前沿。用户应继续审阅 Claude 的输出，以确保其符合要求；特别是对于高风险工作，人类判断仍然至关重要。随着我们继续改进 Claude 的能力，我们的目标是为金融行业专业人士提供更强大的研究和分析工具，并帮助他们专注于最重要的工作。

Claude Opus 4.6、Cowork 和 Claude in Excel 可在所有付费 Claude 计划中使用。要了解更多关于 Claude in Excel 的信息，请探索我们的 [指南](https://support.claude.com/en/articles/12650343-claude-in-excel) 和 [视频教程](https://claude.com/resources/tutorials/getting-started-with-claude-in-excel)，并 [在此开始](https://claude.com/claude-in-excel)。Claude in PowerPoint 以研究预览形式向所有 Max、Team 和 Enterprise 用户提供，你可以 [在此开始](https://claude.com/claude-in-powerpoint)。

要了解组织如何实际使用这些新功能，[请注册我们的网络研讨会](https://anthropic.com/webinars/claude-in-excel-and-powerpoint)。

‍

###### *Cowork 是* [*可用的*](https://support.claude.com/en/articles/13345190-getting-started-with-cowork) *作为仅限桌面的研究预览，适用于所有付费 Claude 计划，从 Mac 开始（Windows 即将推出）。*

‍

‍

‍
