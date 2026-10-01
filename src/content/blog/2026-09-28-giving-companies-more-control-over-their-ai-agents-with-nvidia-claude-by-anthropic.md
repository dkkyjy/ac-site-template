---
title: "与英伟达合作，为企业提供更多对 AI 智能体的控制能力 | Anthropic 的 Claude"
description: "英伟达今天宣布推出 [Open Agent Safety Platform](https://nvidianews.nvidia.com/news/open-agent-safety-platform)，这是一个用于加强 AI 安全的开放软件平台和参考系统架构。Anthropic 与英伟达合作，为智能体技术栈添加了更多的安全与控制层。"
pubDate: "2026-09-28"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/giving-companies-more-control-over-their-ai-agents-with-nvidia"
---

# 与英伟达合作，为企业提供更多对 AI 智能体的控制能力 | Anthropic 的 Claude

---

英伟达今天宣布推出 [Open Agent Safety Platform](https://nvidianews.nvidia.com/news/open-agent-safety-platform)，这是一个用于加强 AI 安全的开放软件平台和参考系统架构。Anthropic 与英伟达合作，为智能体技术栈添加了更多的安全与控制层。

Claude Managed Agents 是一套用于大规模构建和部署生产级智能体的可组合 API。它将智能体所需的凭据保存在保险库中，使智能体无法直接看到它们。开源的英伟达 OpenShell 软件旨在控制智能体在工作期间可以执行和访问的内容。使用 Managed Agents 与 OpenShell 的客户可以限制智能体的操作范围，审查智能体的行为，并确认这些限制已生效。

企业正从使用 AI 回答问题，转向部署能够跨业务部门处理复杂工作、使用专有数据并代表用户采取行动的 AI 智能体。随着模型能力的提升，智能体的用途越来越多，获得的访问权限也越来越大。智能体的访问权限越大，企业就越需要对其行为进行控制和检查。

## **分层保护**

保护始于模型内部的安全机制。Managed Agents 和英伟达 Open Shell 在模型外部添加了限制，并应用于智能体的行为。每一层都设计为独立执行其限制，因此保护不依赖于任何单一层级。这些层级是模块化的，企业可以选择适合自己环境的层级。

## **Claude Managed Agents 执行工作并保管凭据**

使用 Managed Agents 时，智能体循环在与沙盒分离的服务器上运行。沙盒是一个隔离环境，工作在其中进行。凭据（包括密码和访问密钥）保存在独立的保险库中，智能体无法看到它们。

Managed Agents 还提供审计追踪，记录每个智能体的行为，并与企业现有的访问控制集成。企业可以自带沙盒环境，并选择其运行方式和位置。

## **英伟达 OpenShell 设定智能体的访问范围**

[OpenShell](https://www.nvidia.com/en-us/ai/openshell/) 是英伟达的开源安全运行时软件。它管理和监控所有 AI 智能体的行为，并对每个操作强制执行策略。OpenShell 默认阻止所有操作，除非规则允许。它会检查智能体尝试使用的每个工具，并对智能体访问的文件、网络连接和数据应用规则。这些规则在智能体外部执行，OpenShell 会记录其允许或阻止的每个决定。

团队可以从窄权限开始，审查日志，并使用 Claude 收紧规则，使智能体仅获得完成任务所需的最小访问权限。OpenShell 的策略证明器随后使用数学证明来确认智能体在团队制定的规则下可以访问的内容。

## **Claude Managed Agents 包含的功能**

* 生产级智能体，为您处理安全的沙盒化、身份验证和工具执行。
* 长时间运行的会话，可自主运行数小时，即使断开连接也能保留进度和输出。
* 多智能体编排，智能体可以启动并指导其他智能体以并行处理复杂工作。
* 可信治理，为智能体提供对真实系统的访问权限，内置范围权限、身份管理和执行追踪。

## **团队如何使用 Managed Agents**

* [Notion](https://claude.com/customers/notion-qa) 让团队可以在工作区内将工作交给 Claude。工程师用它来发布代码，其他员工用它来制作网站和演示文稿。数十个任务可以并行运行，团队可以一起处理结果。
* [Rakuten](https://claude.com/customers/rakuten-qa) 在工程、产品、销售、营销和财务部门部署专业智能体，每个智能体在一周内即可部署。
* [Asana](https://claude.com/customers/asana-qa) 构建了 AI Teammates，即在 Asana 项目中与人协作的智能体，它们可以承担任务并起草交付物。使用 Managed Agents，团队能够比其他方式更快地添加高级功能。

## **可用性**

Managed Agents 现已可用。它可以在您控制的沙盒中运行，无论是在您自己的基础设施上，还是通过托管服务提供商。英伟达 OpenShell 采用 Apache 2.0 许可证开源，可在 [GitHub](https://github.com/NVIDIA/OpenShell) 和英伟达的 [开发者资源页面](https://docs.nvidia.com/openshell/latest/about/overview) 上获取。
