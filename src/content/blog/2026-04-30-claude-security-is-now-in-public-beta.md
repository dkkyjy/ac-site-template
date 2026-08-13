---
title: "Claude Security 现已进入公开测试阶段"
description: "Claude Security 现已向 Claude Enterprise 客户提供公开测试版。"
pubDate: "2026-04-30"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-security-public-beta"
---
            Claude Security 现已向 Claude Enterprise 客户提供公开测试版。

AI 网络安全能力正在快速发展。当今的模型在发现软件代码缺陷方面已经非常高效；下一代模型将具备更强的能力，尤其是在自主*利用*这些缺陷方面将特别有效。现在是组织采取行动改善其安全性的时机，为一个更容易发现可用软件漏洞的世界做好准备。

最近，我们推出了 Claude Mythos Preview——该版本在发现和利用软件漏洞方面能够达到甚至超越顶尖人类专家——并作为 [Project Glasswing](https://www.anthropic.com/glasswing) 的一部分提供给部分合作伙伴。

但我们的网络安全工作不止于 Glasswing：借助 Claude Security，更广泛的组织可以将我们最强大的通用模型 Claude Opus 4.7 应用于其代码库。Opus 4.7 是目前用于发现和修补软件漏洞，以及发现可能被遗漏的复杂、上下文相关问题的最强模型之一。

Claude Security（此前称为 Claude Code Security）已在有限的研究预览阶段接受了数百家不同规模组织的测试，帮助团队扫描其代码库以发现漏洞并生成针对性补丁。他们的反馈塑造了今天的发布版本，使 Claude Security 可供所有 Enterprise 客户使用。它配备了定时和定向扫描、更易于集成审计系统，以及改进的已分类发现结果追踪功能。无需 API 集成或自定义代理构建：如果您的组织使用 Claude，即可立即开始扫描。

Opus 4.7 的能力也通过 Claude 与许多企业已使用的软件工具集成，被带给网络防御者。我们的技术合作伙伴，包括 [CrowdStrike](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-puts-claude-opus-4-7-to-work-across-falcon-platform-project-quiltworks/)、Microsoft Security、[Palo Alto Networks](https://www.paloaltonetworks.com/blog/2026/04/ai-driven-defense-anthropics-claude-opus/)、[SentinelOne](https://www.sentinelone.com/press/sentinelone-unveils-wayfinder-ai/)、[TrendAI](https://newsroom.trendmicro.com/2026-04-30-TrendAI-TM-and-Anthropic-Advance-AI-Powered-Vulnerability-Detection-and-Risk-Mitigation-with-Claude-Opus-4-7) 和 [Wiz](https://www.wiz.io/blog/red-agent-claude-opus) 正在将 Opus 4.7 嵌入其工具中；此外，Accenture、BCG、Deloitte、Infosys 和 PwC 等服务合作伙伴正在帮助组织部署集成 Claude 的安全解决方案。

我们正进入网络安全的关键时期。AI 正在缩短漏洞发现与利用之间的时间线。我们相信，正确的应对方式是确保防御者能够通过最便捷的途径——直接通过 Claude 以及通过我们的合作伙伴——获得前沿能力。

## **Claude Security 的工作原理**

[Claude Security](https://youtu.be/0SgCiUfoYo8) 可直接从 Claude.ai 侧边栏访问，或通过 [claude.ai/security](http://claude.ai/security) 访问。要开始使用，请选择您的其中一个代码仓库（或限定到特定目录或分支），然后开始扫描。

在扫描过程中，Claude 会像安全研究员一样推理代码。Claude 并非通过搜索已知模式来发现漏洞，而是试图理解组件如何在文件和模块之间交互、追踪数据流并阅读源代码。

完成后，Claude 会提供每个发现的详细说明，包括其对漏洞真实性的置信度、严重程度、可能的影响以及复现方法。它还会生成针对性补丁的说明，用户可以在 Claude Code on the Web 中打开这些说明，在上下文中处理修复。

## **自初始预览以来我们学到的经验**

在过去两个月中，我们根据从数百家企业生产环境中使用 Claude Security 所获得的经验对其进行了改进。具体而言，我们观察到：

**检测质量至关重要。** 团队告诉我们，高置信度的发现才能真正加速安全工作。Claude Security 的多阶段验证管道在发现结果到达分析师之前会独立检查每个发现，从而降低误报率，并且 Claude 会为每个结果附加置信度评级。这意味着到达团队的信号是值得采取行动的。

**从扫描到修复的时间是关键指标。** 早期用户一致指出这一点，多个团队在一次工作中就完成了从扫描到应用补丁的过程，而不是在安全和工程团队之间来回数天。

**团队需要持续覆盖，而非一次性审计。** 我们增加了安排扫描的选项，以便团队可以设定定期节奏来审查和处理发现结果。

在此次发布中，我们还增加了以下功能：将扫描限定到仓库中的特定目录、附带记录原因驳回发现结果（以便未来的审查者可以信任先前的分类决策）、将发现结果导出为 CSV 或 Markdown 格式用于现有追踪和审计系统，以及通过 Webhook 将扫描结果发送到 Slack、Jira 或其他工具。

以下是使用过 Claude Security 的组织分享的体验：

我们仍处于 AI 驱动安全的早期阶段。随着我们的模型不断改进以及我们从在生产环境中使用 Claude Security 的团队那里学习，我们将继续扩展这些工具的功能。

## **在防御者的工作环境中满足其需求**

Claude Security 是我们将前沿能力交到防御者手中的更广泛努力的一部分。其覆盖范围很大程度上来自于团队今天依赖的平台和服务合作伙伴。

[CrowdStrike](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-puts-claude-opus-4-7-to-work-across-falcon-platform-project-quiltworks/)、Microsoft Security、[Palo Alto Networks](https://www.paloaltonetworks.com/blog/2026/04/ai-driven-defense-anthropics-claude-opus/)、[SentinelOne](https://www.sentinelone.com/press/sentinelone-unveils-wayfinder-ai/)、[TrendAI](https://newsroom.trendmicro.com/2026-04-30-TrendAI-TM-and-Anthropic-Advance-AI-Powered-Vulnerability-Detection-and-Risk-Mitigation-with-Claude-Opus-4-7) 和 [Wiz](https://www.wiz.io/blog/red-agent-claude-opus) 正在将 Opus 4.7 的能力集成到企业今天已经运行的安全平台中。

Accenture、BCG、Deloitte、Infosys 和 PwC 正在与企业安全组织合作，部署集成 Claude 的安全解决方案，用于漏洞管理、安全代码审查和事件响应项目。

总之，这意味着组织可以通过适合其现有运营方式的任何途径采用这些能力：直接在 Claude Security 中使用、嵌入到其信任的平台中，或由服务团队指导部署。

## **开始使用**

Claude Security 自今天起向 Claude Enterprise 客户提供公开测试版。Claude Team 和 Max 客户的访问权限即将推出。

管理员可以在[管理控制台](http://claude.ai/admin-settings/claude-code)中启用 Claude Security。有关完整指南，请参阅我们的[入门指南](https://claude.com/resources/tutorials/getting-started-with-claude-security)。

¹Claude Opus 4.7 使用新的网络安全防护措施，可自动检测并阻止暗示被禁止或高风险网络安全用途的请求。但是，进行可能触发这些防护措施的工作的组织可以加入我们的[网络验证计划](https://support.claude.com/en/articles/14604842-real-time-cyber-safeguards-on-claude)，这是我们努力将前沿能力提供给防御者同时防止其落入不法之手的一部分。
