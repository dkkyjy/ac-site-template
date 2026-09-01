---
title: "将 Claude Mythos 5 的网络防御能力带给更多防御方 | Claude by Anthropic"
description: "---"
pubDate: "2026-08-21"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/bringing-claude-mythos-5-to-more-defenders"
---

# 将 Claude Mythos 5 的网络防御能力带给更多防御方 | Claude by Anthropic

---

*我们在此分享有关帮助更多团队利用前沿能力进行网络防御的最新进展。* [*Claude Mythos 5*](https://www.anthropic.com/news/claude-fable-5-mythos-5) *现已在* [*Claude Security*](https://claude.com/product/claude-security)* 中上线，并将很快集成到合作伙伴的网络防御工具中。我们同时启动了一项 3500 万美元的基金，用于保障开源软件的安全，并公布了扩大* [*网络验证计划*](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet)* 的计划。*

四月，我们启动了 [玻璃翼计划](https://www.anthropic.com/glasswing)，将我们最强大的前沿模型 Claude Mythos 预览版（及其继任者 Claude Mythos 5）交由一小批组织使用，这些组织致力于保护全球最关键的软件。这为防御方赢得了一段窗口期，使其能够在具备类似能力的模型广泛可用或落入恶意行为者之手之前，发现并修复漏洞。

我们的目标始终是尽可能安全地将 Mythos 级别的防御能力扩展给更多防御方。为此，我们一直在研发[安全分类器](https://www.anthropic.com/research/next-generation-constitutional-classifiers)和防护机制，使我们能够在不将攻击性网络能力置于错误之人的手中的前提下，扩大对 Mythos 级模型的访问权限。[Claude Fable 5](https://www.anthropic.com/news/claude-fable-5-mythos-5) 迈出了第一步：它在广泛开放模型的同时，屏蔽了双重用途的网络工作。

今天，我们正在迈出下一步。风险最高的场景出现在用户直接访问模型时，恶意行为者可能试图将模型引导至有害用途。但如果用户只能获取特定输出，例如某个漏洞的补丁或安全警报，那么风险会大幅降低。我们此次宣布的变更让用户能够更广泛地获取防御性成果，同时在模型直接访问方面保持适当的护栏：

* **将 Claude Mythos 5 集成到防御方依赖的工具中。** 我们正在与网络安全技术和服务合作伙伴合作，将 Claude Mythos 5 集成到防御方已用于保护其软件的产品和服务中。
* **Claude Security 扫描现已支持 Claude Mythos 5。** 使用 Claude 企业版计划的客户现在可以在 Claude Security 中运行我们最强大的模型，用它来扫描代码库中的安全漏洞并建议补丁。
* **3500 万美元的开源安全积分。** 我们新设立的防御者优势基金（0xDAF）将为致力于修补开源项目漏洞、自动化开源软件扫描和修补流程的部分环节、以及尝试新安全方法的组织提供 3500 万美元的积分支持。
* **扩大网络验证计划。** 该计划已为经过审核的防御方在 Opus 和 Sonnet 模型上提供降低的防护措施。在未来几周，我们将扩大该计划的范围，在 Opus 和 Sonnet 上开放更广泛的双重用途能力，随后将开放 Mythos 级访问权限。

我们的目标始终是帮助各组织适应网络安全领域日益加速的节奏和需求，因为 AI 模型的能力在不断增强。我们将继续开发防护机制、访问计划及社区支持，使我们最强大的模型能够安全地为各类人群和组织所用。

## 将 Mythos 集成到现有网络防御工具中

为医院、公用事业、金融系统和软件供应链提供防护的团队，已依赖一系列产品和服务来开展安全运营、事件响应、威胁情报和检测工程工作。让前沿能力最快惠及这些防御方的方式，就是将 Mythos 级模型集成到他们已经在使用的工具中。

我们的许多合作伙伴已经[基于 Claude Opus 构建了网络安全产品](https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity)，帮助安全团队更快地分类处理警报、识别威胁和修复漏洞。我们现在正与这些合作伙伴以及更多伙伴合作，将 Claude Mythos 5 构建到他们的产品和服务中，使他们能够向客户交付 Mythos 级别的防御成果。

当终端用户使用这些产品之一时，他们并非直接与 Mythos 交互。相反，他们通过一个专用界面进行操作，该界面在后台运行 Mythos 以执行特定任务，用户仅接收该产品旨在提供的特定输出结果。例如，一个漏洞修复工具可能以一组建议补丁作为其输出。这些输出由 Mythos 生成，但用户无法通过提示词让模型去开发某个漏洞的利用程序。我们和合作伙伴也设有滥用预防措施，以确保模型始终在其预期范围内运行。

我们在这项工作上还处于早期阶段，预计未来会逐步扩展。如果您开发安全产品或服务，并希望将 Claude Mythos 5 带给您的客户，可以[在此注册意向](https://claude.com/form/mythos-cyber-partner)。

## 为企业客户推出基于 Claude Mythos 5 的 Claude Security

从今天起，[Claude Security](https://claude.com/product/claude-security) 扫描已全面运行在 Claude Mythos 5 上。Claude Security 会扫描代码库中的漏洞并为人工审核建议补丁；目前面向 Claude 企业版客户处于公开测试阶段，使用 Mythos 5 的扫描按现有计划中的标准令牌使用量计费，无需额外付费。

企业管理员可以在[管理控制台](http://claude.ai/admin-settings/claude-code)中启用 Claude Security。通过 [claude.ai/security](http://claude.ai/security)，用户可以选择一个代码仓库，使用 Claude Mythos 5 进行扫描。Claude 随后会扫描代码库中的漏洞，并返回每项发现，附带 [CWE](https://cwe.mitre.org/)（通用弱点枚举）分类、置信度和严重性评级，以及建议的修复方案。

用户随后可以打开网页版 Claude Code 来实施修复。交互式修补使用组织在 Claude Code 中可访问的模型。Mythos 扫描本身不会将 Mythos 的访问权限扩展到其他场景。每个补丁在实施前都必须经过人工审核和批准。

Claude Security 使用 Mythos 5 扫描用户拥有的代码，返回详细的发现结果而非原始输出，同时不暴露模型本身。这意味着防御方可以访问 Claude Mythos 5 的能力，而模型不会暴露给可能滥用它的人。

有关 Claude Security 的更多信息，请参阅我们的[入门指南](https://claude.com/resources/tutorials/getting-started-with-claude-security)。

## 启动防御者优势基金以保障开源软件安全

世界上一些使用最广泛的程序运行在开源软件之上。然而，这些项目通常由志愿者或非营利基金会维护，他们可能缺乏全面防御攻击所需的资源或人力。通过玻璃翼计划，我们向开源安全组织提供了 400 万美元的直接捐赠，为计划中的开源安全基金会提供积分支持，帮助扫描和修补广泛使用的项目，并支持协调式漏洞修复工作，如 [Akrites](https://akrites.org/) 和 [Gold Eagle](https://www.whitehouse.gov/releases/2026/07/white-house-launches-gold-eagle-initiative-for-unprecedented-cybersecurity-vulnerability-coordination/)。

我们新设立的防御者优势基金（0xDAF）在此基础上进一步发力，为帮助开源维护者保障软件安全的组织提供 3500 万美元的 Claude 积分。资助将聚焦三个方向：修补广泛使用项目中的活跃漏洞、以其他项目可复制的方式自动化扫描和修补流程、以及帮助项目探索更具雄心的安全方案，使其能够抵御整类攻击。

我们将从少量大额试点资助开始，以探索哪些做法最有效、最具可扩展性。我们将在未来几周公布首批受助方的详细信息。

## 扩大网络验证计划

迄今为止，我们的网络验证计划为使用 Claude Opus 和 Sonnet 模型的组织提供了双重用途能力的访问权限。计划中的组织享有降低的防护措施，最大限度减少已获批准的团队在受授权保护的系统中开展合法网络安全工作时受到的中断。

在未来几周，我们将推动该计划演进，扩大对 Claude Mythos 的受控访问。作为其中的一部分，漏洞分类处理、验证等防御性能力的访问将扩展到 Mythos 级模型，网络防御方在 Claude Opus 和 Sonnet 级模型上将体验到更少的拦截。此外，我们将继续与美国政府合作伙伴合作，通过玻璃翼计划扩大对 Claude Mythos 的访问，重点面向符合严格安全管控要求的关键基础设施保护者。

我们将在未来几周分享有关网络验证计划扩展的更多详情。在此期间，我们鼓励所有开展合法网络安全工作的安全团队申请加入该计划，以在 Claude Opus 和 Sonnet 模型上获得降低的防护措施。如果您已注册并获批准，则无需采取任何操作；我们将主动联系您并提供更新。

## 下一步

这些举措是我们持续努力的延续，旨在将前沿模型的防御能力带给更多人和组织，并支持开源社区加固其项目以抵御攻击。我们将继续与政府合作伙伴、各类组织、开源维护者以及更广泛的行业合作，构建当今高能力 AI 模型所需的韧性网络基础设施。

* 申请加入[网络验证计划](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude-opus-and-sonnet)。
* [注册意向](https://claude.com/form/mythos-cyber-partner)，参与基于 Mythos 的网络安全产品和解决方案开发。
* Claude Security 已面向企业客户开放公开测试。管理员可在[管理控制台](http://claude.ai/admin-settings/claude-code)中启用 Claude Security。完整操作指南请参阅我们的[入门指南](https://claude.com/resources/tutorials/getting-started-with-claude-security)。
