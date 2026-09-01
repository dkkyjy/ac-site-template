---
title: "Claude in Chrome 现已全面可用 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-26"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-in-chrome-generally-available"
---

# Claude in Chrome 现已全面可用 | Claude by Anthropic

---

Claude in Chrome 现已在所有付费版 Claude 方案中全面可用。Claude 现在还可以在浏览器中自主执行操作，而无需对每一步操作都进行审批。一个安全分类器会在每次操作执行前对其进行验证，以确保操作安全且符合您的请求。

您每天使用的许多工具都[已与 Claude 连接](http://claude.com/connectors)。但还有许多工具尚未接入，例如内部仪表盘、遗留系统和供应商门户。Claude in Chrome 使 Claude 能够访问这些工具。它可以查看您当前所在的页面，并使用您现有的登录凭据执行操作，例如阅读和输入文本、点击链接、在页面间导航以及填写表单。

我们去年首次宣布了 Claude in Chrome 的试点计划，以便在测试的同时加强我们抵御[提示注入](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks)的防御能力：隐藏在网页、电子邮件或文档中的恶意指令，试图欺骗 AI 代理做出违背用户意愿的行为。以下将介绍这些防御措施，正是它们让我们有信心将 Claude in Chrome 推向全面可用。

## 防范提示注入

正如我们在宣布试点时[所述](https://claude.com/blog/claude-for-chrome)，在浏览器中工作的 AI 代理同样容易受到提示注入的攻击。因此，在更广泛地发布 Claude in Chrome 之前，我们致力于改进安全防护措施。

在提示注入攻击中，攻击者将指令隐藏在网页内容中，例如某个网页、一封电子邮件或一个表单字段。您可能永远看不到这些指令，但它们可以将代理引导去做您从未要求的事情。例如，如果您已请 Claude 起草电子邮件回复，某封邮件中隐藏的指令可能会让 Claude 将您的其他邮件转发给攻击者。

在发布之初，我们描述了如何测试 Claude 对这些攻击的防御能力以及当时已实施的安全措施；之后我们发布了关于[浏览器使用安全防护](https://www.anthropic.com/research/prompt-injection-defenses)的更详细说明。此后，我们改进了对模型和[探针](https://www.anthropic.com/research/next-generation-constitutional-classifiers)的训练方式，并增加了一组额外的分类器，使 Claude 能够在 Chrome 中更安全地执行更多自主操作。在下一节中，我们将介绍评估结果，这些结果展示了上述安全措施的有效性。

**Claude 能识别更多攻击。** 我们利用不断增长的提示注入攻击库来训练 Claude，攻击样本来源于我们的内部自动化攻击工具、外部红队测试人员以及真实世界的监控。当一种新攻击对当前模型成功时，它会被添加到攻击库中，从而指导未来模型和已部署安全措施的训练，使其学会识别该攻击。自 2025 年 11 月我们首次撰写[浏览器使用的提示注入防御](https://www.anthropic.com/research/prompt-injection-defenses)以来，Claude 对这些攻击的抵御能力已大幅提升。

**探针在 Claude 执行操作前对网页内容进行筛查。** 网页内容通过工具结果传递给 Claude。要执行诸如阅读页面或打开邮件之类的操作，模型会发起一次工具调用；工具结果使模型能够读取输出（在本例中即为页面或邮件的内容）。我们训练探针来扫描这些结果中是否存在潜在的提示注入。当探针检测到可能的攻击时，Claude 会被提醒以怀疑态度对待该内容，并在必要时于执行操作前与您确认。我们首次随 Claude Opus 4.5 部署了这些探针，此后已扩展了它们所覆盖的攻击类型。

**操作在执行前经过验证**。在 Claude in Chrome 中，Claude 现在会自动批准其判定为安全的操作，使用的机制与 Claude Code 中的[自动模式](https://claude.com/blog/auto-mode-default-in-claude-code)相同。（如果您希望继续手动审批 Claude 的操作，可以在设置中关闭此功能。）一个分类器会审查 Claude 即将执行的操作，例如导航到新网站或在页面中输入文本，并将其与您最初的请求进行比对。如果操作与您的请求不匹配，则会被阻止。

## 衡量 Claude 抵御提示注入的稳健性

我们已对这些安全措施进行了测试，以确保 Claude in Chrome 在基于浏览器的工作中可以安全使用。以下是我们最新评估的结果。

在我们的[初始评估](https://claude.com/blog/claude-for-chrome)中，测试了 Claude Cowork 对提示注入攻击的抵御能力（该能力在发布 Claude in Chrome 试点时首次开发），在[Cowork 框架](https://claude.com/blog/cowork-chrome-side-panel)中，没有任何攻击对 Claude Fable 5、Claude Opus 5 或 Claude Sonnet 5 成功，即使未使用上述提到的探针和分类器也是如此。

*针对 Claude Opus 4.5、Sonnet 5、Opus 5 和 Fable 5 的提示注入攻击成功率。Opus 4.5 使用扩展思考模式运行，因为它不支持我们较新的默认自适应思考模式。所有其他模型均以中等努力的自适应思考模式作为默认设置运行。我们在* [*2025 年 11 月博文*](https://www.anthropic.com/research/prompt-injection-defenses) *中讨论的结果是在未启用扩展思考的情况下运行的，但由于 Fable 5 无法禁用思考模式，因此我们在此报告启用思考后的结果。11 月使用的评分模型也已不再可用，因此我们转向了功能更强的评分流程，并结合对成功攻击的人工审查，从而减少了误报。*

由于该评估已趋于饱和（如 0% 的成功率所示），我们决定将其退役。在我们的[当前评估](https://www-cdn.anthropic.com/b514064af1408018e64b1ad24e7d5e75850b4ffd/Claude%20Opus%205%20System%20Card.pdf#page=76.73)中，该评估使用由专业红队测试人员提供的更强攻击，在施加任何额外安全措施之前，到达模型的攻击对 Opus 4.5 的成功率为 17.6%，对 Opus 5 的成功率为 3.8%。在 2025 年 11 月可用的最强安全措施下，对运行探针的 Opus 4.5 的攻击成功率为 16.7%。对于 Opus 4.8 及之后的所有模型，在运行探针和安全分类器的情况下，没有任何攻击对 Claude Sonnet 5、Claude Opus 5 或 Claude Mythos 5 成功。我们观察到对 Fable 5 的攻击成功率为 0.3%。我们已人工验证所有成功突破均属于低严重性场景，并正在努力加以缓解。

*在探针加自动审批安全分类器的组合下，没有任何攻击对 Claude Sonnet 5 或 Opus 5 成功，对 Fable 5 的攻击成功率为 0.3%。Opus 4.5 的模型行为导致到达模型的攻击数量较少，但其成功攻击的百分比仍为最高。*

提示注入始终是一个不断变化的目标。虽然当前方案能够抵御现有攻击，但我们还需要确保安全防护措施始终领先于攻击者不断演变的攻击手段。随着每个新模型的发布，我们持续投入开发更先进的自动化攻击发现、红队测试以及构建更强分类器的系统。

## 开始使用

要开始使用 Claude in Chrome，请从 [Chrome 应用商店](https://chromewebstore.google.com/detail/claude/fcoeoabgfenejglbffodgkkbkcdhcgfn) 安装。在企业版方案中，管理员可以在组织设置中管理该功能，并将其限制在已批准的域名内。请参阅[管理员设置指南](https://support.claude.com/en/articles/13065128-claude-in-chrome-admin-controls#h_bdb63199e1)。

您仍然需要使用 Claude 桌面应用来处理计算机上的文件或其他应用程序。Claude in Chrome 目前尚不支持其他 Chromium 浏览器或移动设备。

 

*¹ 并非所有攻击都能到达——即被模型看到。在某些情况下，Claude 执行的操作使其从未遇到恶意指令。*
