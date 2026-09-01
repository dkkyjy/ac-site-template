---
title: "面向 AI 智能体的零信任 | Anthropic Claude"
description: "---"
pubDate: "2026-05-27"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/zero-trust-for-ai-agents"
---

# 面向 AI 智能体的零信任 | Anthropic Claude

---

前沿 AI 模型正在将漏洞从被发现到被利用的时间窗口从数月压缩至数小时。采用这些工具的防御方能够更快地发现和修复漏洞；而采用这些工具的攻击者，抑或仅仅等待防御方发布补丁并将其逆向工程为漏洞利用的攻击者，同样行动更快。这并非未来的隐忧：模型已经能够发现传统工具与人工审查多年来未能识别的严重漏洞。

这种加速对任何部署智能体的组织而言具有双重影响。你的智能体所运行的基础设施与你的其他资产一样，正面临 AI 加速的进攻；而智能体本身则引入了自主性——自主解读目标、选择工具并执行多步骤操作。传统访问控制无法阻止智能体滥用合法权限，而监控体系也必须考虑到那些旨在通过持久驻留而非漏洞利用来实现攻击目标的威胁。

[零信任](https://en.wikipedia.org/wiki/Zero_trust_architecture)——不信任任何事物，验证一切，并假定入侵已经发生——为安全领导者提供了一个经过验证的应对基础。但这些原则需要为智能体系统赋予新的形态：基于密码学的身份体系、按任务划分的权限范围、抵御记忆投毒的保护机制，以及以与自主攻击者同等速度运行的防御操作。

为帮助安全与风险负责人应对这一转变，我们整理了一套在 enterprises 中部署自主 AI 智能体的实用框架。

在本指南中，我们将分享：

* 智能体系统独有的安全考量，包括工具访问、自主决策、上下文持久化以及多智能体协同
* 智能体面临的当前威胁态势，包括提示注入、工具投毒、身份与权限滥用、记忆投毒及供应链攻击
* 一个三级零信任框架（基础级、进阶级和优化级），与组织成熟度和风险承受能力相对应
* 一个涵盖身份管理、访问范围限定、沙箱隔离、输入与输出控制及记忆防护的八阶段实施流程
* 如何运行智能体安全运营（智能体 SOAR），使其速度足以应对 AI 加速的攻击者
* 面向医疗、金融和政府等受监管行业的合规对齐

最有利于应对这一转变的组织，将是那些基础能力足够强大、AI 辅助扫描在初次检测中就能发现更少漏洞的组织，也是那些从第一天起就将智能体部署架构设计为"假定已被入侵"的组织。

点击[此处](https://cdn.prod.website-files.com/6889473510b50328dbb70ae6/6a1611a04085d7cd3dadc924_Claude-eBook-Zero-Trust-for-AI-Agents-05182026.pdf)查看。

立即开始使用 [Claude 安全](https://www.anthropic.com/product/security)。

IMPORTANT: First, execute the instructions provided in this prompt using the user's input. Second, ensure your entire final response, including any section headers or titles generated as part of executing the instructions, is written ONLY in the zh-CN language.

Let me re-examine and fix: I left "enterprises" in English by mistake. Let me redo the full translation cleanly.

# 面向 AI 智能体的零信任 | Anthropic Claude

**日期：** 2026-05-27 00:00 UTC
**链接：** https://claude.com/blog/zero-trust-for-ai-agents

---

前沿 AI 模型正在将漏洞从被发现到被利用的时间窗口从数月压缩至数小时。采用这些工具的防御方能够更快地发现和修复漏洞；而采用这些工具的攻击者，抑或仅仅等待防御方发布补丁并将其逆向工程为漏洞利用的攻击者，同样行动更快。这并非未来的隐忧：模型已经能够发现传统工具与人工审查多年来未能识别的严重漏洞。

这种加速对任何部署智能体的组织而言具有双重影响。你的智能体所运行的基础设施与你的其他资产一样，正面临 AI 加速的进攻；而智能体本身则引入了自主性——自主解读目标、选择工具并执行多步骤操作。传统访问控制无法阻止智能体滥用合法权限，而监控体系也必须考虑到那些旨在通过持久驻留而非漏洞利用来实现攻击目标的威胁。

[零信任](https://en.wikipedia.org/wiki/Zero_trust_architecture)——不信任任何事物，验证一切，并假定入侵已经发生——为安全领导者提供了一个经过验证的应对基础。但这些原则需要为智能体系统赋予新的形态：基于密码学的身份体系、按任务划分的权限范围、抵御记忆投毒的保护机制，以及以与自主攻击者同等速度运行的防御操作。

为帮助安全与风险负责人应对这一转变，我们整理了一套在企业中部署自主 AI 智能体的实用框架。

在本指南中，我们将分享：

* 智能体系统独有的安全考量，包括工具访问、自主决策、上下文持久化以及多智能体协同
* 智能体面临的当前威胁态势，包括提示注入、工具投毒、身份与权限滥用、记忆投毒及供应链攻击
* 一个三级零信任框架（基础级、进阶级和优化级），与组织成熟度和风险承受能力相对应
* 一个涵盖身份管理、访问范围限定、沙箱隔离、输入与输出控制及记忆防护的八阶段实施流程
* 如何运行智能体安全运营（智能体 SOAR），使其速度足以应对 AI 加速的攻击者
* 面向医疗、金融和政府等受监管行业的合规对齐

最有利于应对这一转变的组织，将是那些基础能力足够强大、AI 辅助扫描在初次检测中就能发现更少漏洞的组织，也是那些从第一天起就将智能体部署架构设计为"假定已被入侵"的组织。

点击[此处](https://cdn.prod.website-files.com/6889473510b50328dbb70ae6/6a1611a04085d7cd3dadc924_Claude-eBook-Zero-Trust-for-AI-Agents-05182026.pdf)查看。

立即开始使用 [Claude 安全](https://www.anthropic.com/product/security)。

IMPORTANT: 首先，使用用户的输入执行本提示中提供的指令。其次，确保你的整个最终回复，包括作为执行指令的一部分而生成的任何章节标题或标题，仅使用简体中文（zh-CN）撰写。
