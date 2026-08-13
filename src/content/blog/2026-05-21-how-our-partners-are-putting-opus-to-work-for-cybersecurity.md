---
title: "我们的合作伙伴如何将Opus应用于网络安全"
description: "人工智能正在改变安全漏洞被发现和利用的速度，而最明确的应对措施是让安全团队将高性能模型应用于自身的防御工作。"
pubDate: "2026-05-21"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/how-our-partners-are-putting-opus-to-work-for-cybersecurity"
---
            人工智能正在改变安全漏洞被发现和利用的速度，而最明确的应对措施是让安全团队将高性能模型应用于自身的防御工作。

当我们推出[Claude Security公开测试版](https://claude.com/blog/claude-security-public-beta)时，我们还分享了一批基于Claude Opus构建的技术和服务合作伙伴，因为实现快速采用的最佳路径因团队而异：有些团队可能直接使用Claude，有些通过他们已有的平台使用，还有些通过熟悉其环境的服务合作伙伴使用。

其中一些服务现已上线，初步结果展示了前沿模型防御在实际中的应用。

## 初步成果

合作伙伴报告了由Opus驱动的防御能力在内部和客户环境中的显著提升：

* 每周对超过150,000个生产资产进行持续渗透测试，每周发现数千个经过验证的高危和严重级别发现，且零误报（Wiz，在客户生产环境中）。
* 在不到三周的时间内完成了相当于一年的渗透测试工作量（Palo Alto Networks，内部测试）。
* 安全测试覆盖率从约10%提升至超过80%，覆盖1,600个应用程序和500,000多个API，扫描周期从3-5天缩短至不到一小时（Accenture，在其自身基础设施上）。

这些工作分为三个领域：大规模进攻性测试、缩小发现与修复漏洞之间的差距，以及将受治理的AI部署到生产环境中。

## 在生产规模下进行持续进攻性测试

进攻性测试意味着像对手那样攻击自己的系统，以便你首先发现可利用的路径。

Wiz [Red Agent](https://www.wiz.io/blog/red-agent-claude-opus) 是一个AI驱动的攻击者，它使用Opus像人类渗透测试人员一样对生产Web应用程序和API进行推理。它分析应用程序逻辑，串联步骤，并适应实时服务器响应，以发现传统扫描器遗漏的逻辑驱动型缺陷。该工具每周持续运行，覆盖超过150,000个生产资产，发现数千个高危和严重级别的发现，每个发现都通过Wiz Security Graph提供了可利用性证明和业务背景验证。"安全团队不再受限于数据不足，而是受限于采取行动的能力，"Wiz AI与威胁研究副总裁Alon Schindel表示。"通过将前沿模型嵌入Wiz Agents，我们使组织能够以AI的速度进行防御。"

[Unit 42 Frontier AI Defense](https://www.paloaltonetworks.com/unit42/ai-advantage) 是Palo Alto Networks的专家主导服务，使用Opus发现隐藏漏洞，绘制它们如何串联成关键攻击路径，并制定针对AI攻击的加固路线图。该服务将这种暴露分析与机器速度防御的基准蓝图以及实践转型工作相结合。"随着攻击者利用前沿模型自动化网络攻击，防御必须更快行动，"Palo Alto Networks Unit 42高级副总裁Sam Rubin表示。

CrowdStrike的[Frontier AI Readiness and Resilience Service](https://www.crowdstrike.com/en-us/services/ai-security-services/frontier-ai-readiness-and-resilience/) 将同类能力引入一个受超过60%财富500强企业信任的平台，将Opus与CrowdStrike的AI红队服务和专有代理框架相结合，持续在客户应用程序中搜寻潜在的零日漏洞，验证发现，并在新代码进入生产环境之前加速修复。

> "像Anthropic的Claude Opus这样的前沿模型正在为防御者提供一年前还不存在的能力优势，将漏洞管理完全向左推移。" - **Mark Manglicmot，CrowdStrike全球咨询服务副总裁**

## 缩小发现与修复之间的差距

发现漏洞与修复漏洞之间的差距是漏洞暴露的主要来源，因为分类、优先级排序、补丁测试和跨团队交接都需要时间。

Accenture的[Cyber.AI](https://newsroom.accenture.com/news/2026/accenture-and-anthropic-team-to-help-organizations-secure-scale-ai-driven-cybersecurity-operations) 是一个代理平台，将资产、身份、威胁和控制连接成一个单一运营模型，Opus在其中进行推理，将检测、优先级排序和修复作为连续循环运行。Accenture首先在内部进行了大规模验证：将安全测试覆盖率从约10%提升至超过80%，覆盖1,600个应用程序和500,000多个API，并将自身全球IT基础设施中的扫描周期从3-5天缩短至不到一小时——这些成果支撑了Cyber.AI现在为客户提供的服务。

> "企业领导者正在应对历史上变化最快、最复杂的网络威胁环境。我们正在与Anthropic合作，提供客户所需的工具以保持领先。" - **Harpreet Sidhu，Accenture网络安全全球负责人**

TrendAI™ [Vision One](https://www.trendmicro.com/en_us/business/products/one-platform.html) 使用Opus辅助的漏洞研究，帮助覆盖185个国家的企业通过虚拟补丁识别暴露并降低风险。经过验证的发现还会流入TrendAI零日计划进行协调披露，帮助在供应商补丁可用之前长达96天内保护高风险系统。"随着AI加速漏洞发现，防御者面临的实际挑战成为大规模修复，"TrendAI首席平台与业务官兼负责人Rachel Jin表示。"与Anthropic合作，我们正在帮助客户在攻击者利用漏洞窗口之前，通过缓解措施和虚拟补丁降低风险。"

Deloitte基于Deloitte Ascend™构建的[持续威胁暴露管理(CTEM)](https://www.deloitte.com/global/en/services/consulting-risk/services/deloitte-cyber-attack-surface-management.html) 将发现、验证、优先级排序和修复作为一个工作流运行，包括在没有补丁时设计应对措施。Opus的代码推理和自动化稳定性测试使团队有信心在数小时内而非数天或数周内完成修复。"基于Ascend构建的CTEM旨在帮助减少漏洞修复中的决策延迟，"Deloitte合伙人兼美国网络负责人Adnan Amjad表示，"这个差距决定了攻击者还是防御者能赢得时间窗口。"

## 将AI受治理地投入生产

代理型AI用例的新世界给许多团队带来了新的挑战。如果没有清晰的框架，为部署设置控制措施、审计证据和自主边界往往会使AI在安全领域的采用陷入试点困境。

PwC的[Claude原生网络安全服务](https://www.pwc.com/us/en/technology/alliances/anthropic.html) 解决了CISO们同时提出的两个问题：安全地将AI投入生产，以及实现网络安全功能本身的现代化。安全AI采用使企业能够在数周而非数月内从沙盒过渡到生产环境，并提供部署、治理和审计证据，帮助CISO和CRO自信地为团队带来创新。规模化前沿防御将Opus驱动的代理推理集成到现有的漏洞管理、检测、安全工程和GRC工作流中，在定义的护栏和可审计性范围内实现自主执行。

> "这是网络安全的一个决定性时刻，AI驱动的转型对于保持韧性和竞争力至关重要，" - **Morgan Adamski，PwC美国网络、数据与技术负责人**

## 不断发展的生态系统

BCG、Infosys和SentinelOne也正在基于Opus构建防御性网络安全服务，我们将在这些服务可用时分享更多细节。

上述每项服务都基于相同的底层Opus能力：对代码进行推理，理解哪些暴露会转化为实际风险，以及维持长期的代理工作流。我们很高兴能与这些合作伙伴合作，通过最适合安全团队的接入点，将前沿防御带给更多安全团队。

*了解更多关于*[*Claude在安全用例中的应用*](https://claude.com/solutions/security)*。*
