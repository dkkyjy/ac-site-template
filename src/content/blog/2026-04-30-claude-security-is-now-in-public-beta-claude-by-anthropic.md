---
title: "Claude Security 现已进入公开测试阶段 | Claude by Anthropic"
description: "---"
pubDate: "2026-04-30"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-security-public-beta"
---

# Claude Security 现已进入公开测试阶段 | Claude by Anthropic

---

Claude Security 现已面向 Claude Enterprise 客户提供公开测试版。

AI 网络安全能力正在快速发展。当今的模型在发现软件代码中的缺陷方面已经非常高效；下一代模型将更具能力，并且特别擅长自主*利用*这些缺陷。现在是各组织采取行动改善安全性的时机，为一个工作软件漏洞更容易被发现的世界做好准备。

最近，我们向部分合作伙伴提供了 Claude Mythos Preview——该模型在发现和利用软件漏洞方面能够达到甚至超越精英人类专家——作为 [Project Glasswing](https://www.anthropic.com/glasswing) 的一部分。

但我们的网络安全工作不止于 Glasswing：通过 Claude Security，更广泛的组织可以将我们最强大的通用模型 Claude Opus 4.7 应用于其代码库。Opus 4.7 是目前在发现和修补软件漏洞以及发现可能被错过的复杂上下文相关问题方面最强的模型之一。

Claude Security（此前称为 Claude Code Security）已在有限的研究预览阶段接受数百家各种规模组织的测试，帮助团队扫描其代码库中的漏洞并生成针对性补丁。他们的反馈塑造了今天的发布，使 Claude Security 对所有 Enterprise 客户可用。它提供定时和针对性扫描、更易于与审计系统集成，以及改进的已分类发现项跟踪。无需 API 集成或自定义代理构建：如果您所在组织使用 Claude，今天即可开始扫描。

Opus 4.7 的能力也通过 Claude 与许多企业已在使用的软件工具的集成，被带给网络防御者。我们的技术合作伙伴，包括 [CrowdStrike](https://www.crowdstrike.com/en-us/press-releases/crowdstrike-puts-claude-opus-4-7-to-work-across-falcon-platform-project-quiltworks/)、Microsoft Security、[Palo Alto Networks](https://www.paloaltonetworks.com/blog/2026/04/ai-driven-defense-anthropics-claude-opus/)、[SentinelOne](https://www.sentinelone.com/press/sentinelone-unveils-wayfinder-ai/)、[TrendAI](https://newsroom.trendmicro.com/2026-04-30-TrendAI-TM-and-Anthropic-Advance-AI-Powered-Vulnerability-Detection-and-Risk-Mitigation-with-Claude-Opus-4-7) 和 [Wiz](https://www.wiz.io/blog/red-agent-claude-opus)，正在将 Opus 4.7 嵌入其工具中；此外，Accenture、BCG、Deloitte、Infosys 和 PwC 等服务合作伙伴正在帮助组织部署集成了 Claude 的安全解决方案。

我们正进入网络安全的关键时期。AI 正在压缩漏洞发现与利用之间的时间线。我们认为正确的应对方式是确保防御者能够以最易获取的方式——通过 Claude 直接使用以及通过我们的合作伙伴——获得前沿能力。

## **Claude Security 的工作原理**

[Claude Security](https://youtu.be/0SgCiUfoYo8) 可直接从 Claude.ai 侧边栏访问，或访问 [claude.ai/security](http://claude.ai/security)。开始时，选择一个仓库（或限定到特定目录或分支），然后启动扫描。

扫描过程中，Claude 像安全研究员一样推理代码。它并非通过搜索已知模式来发现漏洞，而是试图理解组件如何跨文件和模块交互，追踪数据流，并阅读源代码。

完成后，Claude 会提供每个发现项的详细说明，包括其对漏洞真实性的置信度、严重程度、潜在影响以及复现方式。它还会生成针对性补丁的说明，用户可以在 Claude Code on the Web 中打开这些说明，在上下文中完成修复。

## **自首次预览以来我们学到的**

在过去的两个月里，我们根据 Claude Security 在数百家企业生产环境中的使用经验对其进行了优化。具体来说，我们看到了：

**检测质量至关重要。** 团队告诉我们，高置信度的发现才能真正加速安全工作。Claude Security 的多阶段验证管道在发现项到达分析师之前独立检查每个发现项，从而降低误报率，并且 Claude 会为每个结果附上置信度评级。这意味着最终到达团队的信号是值得采取行动的。

**从扫描到修复的时间是重要的指标。** 早期用户一致指出这一点，一些团队在一次会议中就从扫描进入了应用补丁阶段，而不是在安全和工程团队之间来回数天。

**团队希望持续覆盖，而非一次性审计。** 我们增加了安排扫描的选项，使团队能够设定定期节奏来审查和采取行动。

在此次发布中，我们还增加了以下功能：将扫描限定到仓库中的特定目录、附带文档原因关闭发现项（以便未来的审查者信任先前的分类决策）、将发现项导出为 CSV 或 Markdown 以用于现有跟踪和审计系统，以及通过 webhooks 将扫描结果发送到 Slack、Jira 或其他工具。

以下是一些使用过 Claude Security 的组织分享的体验：
