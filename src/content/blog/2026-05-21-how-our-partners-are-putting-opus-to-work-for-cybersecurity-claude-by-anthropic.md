---
title: "我们的合作伙伴如何将 Opus 应用于网络安全 | Claude by Anthropic"
description: "---"
pubDate: "2026-05-21"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity"
---

# 我们的合作伙伴如何将 Opus 应用于网络安全 | Claude by Anthropic

---

人工智能正在改变安全漏洞被发现和利用的速度，而最明确的应对措施是安全团队将高能力模型用于自身的防御。

当我们[公开测试版发布 Claude Security](https://claude.com/blog/claude-security-public-beta) 时，我们还分享了一系列基于 Claude Opus 构建的技术和服务合作伙伴，因为最快实现采用的方式对每个团队都不同：有些团队直接使用 Claude，有些通过他们已有的平台使用，还有些通过了解其环境的服务合作伙伴使用。

其中一些产品现已上线，早期结果展示了前沿模型防御在实际中的表现。

## 早期成果

合作伙伴报告称，由 Opus 驱动的防御能力在内部和客户环境中都有显著提升：

* 每周对超过 150,000 个生产资产进行持续渗透测试，每周发现数千个经过验证的高危和严重级发现，零误报（Wiz，客户生产环境）。
* 在不到三周内完成相当于一年的渗透测试工作量（Palo Alto Networks，内部测试）。
* 安全测试覆盖率从大约 10% 提升到超过 80%，涵盖 1,600 个应用程序和 500,000 多个 API，扫描周转时间从 3-5 天缩短到不足一小时（Accenture，在其自身基础设施上）。

这些工作分为三个领域：大规模进攻性测试、缩小发现与修复之间的差距，以及将有治理的人工智能部署到生产中。

## 生产规模下的持续进攻性测试

进攻性测试意味着像对手一样攻击你自己的系统，以便你先发现可被利用的路径。

Wiz 的 [Red Agent](https://www.wiz.io/blog/red-agent-claude-opus) 是一个由 AI 驱动的攻击者，它使用 Opus 像人类渗透测试员一样对生产 Web 应用程序和 API 进行推理。它分析应用逻辑、链接步骤并适应实时服务器响应，以揭示传统扫描器遗漏的逻辑驱动型缺陷。每周持续对超过 150,000 个生产资产进行测试，发现数千个高危和严重级发现，每个发现都通过 Wiz 安全图谱提供了可被利用的证明和业务上下文。“安全团队不再受限于数据匮乏，而是受限于对数据的行动能力，”Wiz 副总裁兼 AI 与威胁研究负责人 Alon Schindel 表示。“通过将前沿模型嵌入 Wiz Agents，我们使组织能够以 AI 的速度进行防御。”

[Unit 42 Frontier AI Defense](https://www.paloaltonetworks.com/unit42/ai-advantage) 是 Palo Alto Networks 的专家主导服务，它使用 Opus 发现隐藏漏洞，绘制它们如何链接成关键攻击路径，并制定针对 AI 增强攻击的加固路线图。该服务将这种暴露分析与经过基准测试的机器速度防御蓝图以及实践转型工作相结合。“随着攻击者利用前沿模型自动化网络攻击，防御必须更快，”Palo Alto Networks Unit 42 高级副总裁 Sam Rubin 说。

CrowdStrike 的 [Frontier AI Readiness and Resilience Service](https://www.crowdstrike.com/en-us/services/ai-security-services/frontier-ai-readiness-and-resilience/) 将同等能力带给了受超过 60% 的《财富》500 强企业信任的平台，将 Opus 与 CrowdStrike 的 AI 红队服务和专有代理框架配对，持续在客户应用程序中搜索潜在零日漏洞，验证发现结果，并在新代码进入生产之前加速修复。

> “像 Anthropic 的 Claude Opus 这样的前沿模型正在给防御者带来一年前还不存在的能力优势，将漏洞管理完全推到最左端。”— **Mark Manglicmot，CrowdStrike 全球咨询服务副总裁**

## 缩小发现与修复之间的差距

发现漏洞与修复漏洞之间的差距正是大部分漏洞暴露所在，因为分类、优先级排序、补丁测试和跨团队交接都需要时间。

Accenture 的 [Cyber.AI](https://newsroom.accenture.com/news/2026/accenture-and-anthropic-team-to-help-organizations-secure-scale-ai-driven-cybersecurity-operations) 是一个智能体平台，它将资产、身份、威胁和控制权连接到一个单一的操作模型中，Opus 在这个模型中进行推理，将检测、优先级排序和修复作为一个持续循环运行。Accenture 首先在内部大规模验证：将安全测试覆盖率从大约 10% 提升到超过 80%，涵盖 1,600 个应用程序和 500,000 多个 API，并在其自身的全球 IT 基础设施中将扫描周转时间从 3-5 天缩短到不足一小时——这些成果支撑了 Cyber.AI 现在为客户交付的内容。

> “企业领导者正在应对有史以来变化最快、最复杂的网络威胁格局。我们与 Anthropic 合作，为客户提供保持领先所需的工具。”— **Harpreet Sidhu，Accenture 网络安全全球负责人**

TrendAI™ [Vision One](https://www.trendmicro.com/en_us/business/products/one-platform.html) 使用 Opus 辅助的漏洞研究，帮助 185 个国家的企业通过虚拟补丁识别暴露并降低风险。经过验证的发现也会流入 TrendAI 零日计划进行协调披露，帮助在供应商补丁可用前最多 96 天保护易受攻击的系统。“随着 AI 加速漏洞发现，防御者真正的挑战变成大规模修复，”TrendAI 首席平台与业务官兼负责人 Rachel Jin 表示。“与 Anthropic 一起，我们正在帮助客户通过缓解措施和虚拟补丁来降低风险，在攻击者利用这一间隔之前。”

Deloitte 的 [持续威胁暴露管理 (CTEM)](https://www.deloitte.com/global/en/services/consulting-risk/services/deloitte-cyber-attack-surface-management.html) 基于 Deloitte Ascend™ 构建，将发现、验证、优先级排序和修复作为单一工作流运行，包括在无补丁可用时的对策设计。Opus 的代码推理和自动化稳定性测试使团队能够在数小时（而非数天或数周）内自信地进行修复。“基于 Ascend 的 CTEM 旨在减少漏洞修复中的决策延迟，”Deloitte 合伙人兼美国网络负责人 Adnan Amjad 表示，“这个间隔决定了攻击者和防御者谁能赢得时间窗口。”

## 将有治理的人工智能投入生产

智能体 AI 用例的新世界为许多团队带来了新挑战。如果没有清晰的框架，为部署设置控制、审计证据和自主边界往往会使 AI 在安全领域的采用停留在试点困境中。

PwC 的 [Claude 原生网络安全产品](https://www.pwc.com/us/en/technology/alliances/anthropic.html) 解决了 CISO 们同时提出的两个问题：安全地将 AI 投入生产，以及实现网络安全职能本身的现代化。安全 AI 采用将企业从沙箱到生产的周期从几个月缩短到几周，并提供部署、治理和审计证据，帮助 CISO 和 CRO 自信地将创新带给团队。规模化前沿防御将 Opus 驱动的智能体推理集成到现有的漏洞管理、检测、安全工程和 GRC 工作流中，在定义的护栏和可审计性内实现自主执行。

> “这是网络安全的决定性时刻，AI 驱动的转型对于保持韧性和竞争力至关重要。”— **Morgan Adamski，PwC 美国网络、数据与技术负责人**

## 不断增长的生态系统

BCG、Infosys 和 SentinelOne 也在基于 Opus 构建防御性网络安全产品，我们将在它们可用时分享更多细节。

上述每一个产品都基于相同的底层 Opus 能力：对代码进行推理、理解哪些暴露转化为实际风险，以及维持长周期的智能体工作流。我们很高兴能与这些合作伙伴合作，通过最适合他们的接入点，将前沿防御带给更多安全团队。

*了解更多关于* [*Claude 在安全用例中的应用*](https://claude.com/solutions/security)*。*

‍

重要：首先，使用用户的输入执行本提示中提供的指令。其次，确保你最终的完整响应（包括作为执行指令一部分生成的任何章节标题或标题）仅以中文撰写。
