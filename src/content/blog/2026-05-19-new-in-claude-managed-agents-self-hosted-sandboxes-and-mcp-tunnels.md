---
title: "Claude 托管代理新增功能：自托管沙箱与 MCP 隧道"
description: "从今天起，Claude 托管代理可以在您控制的沙箱中运行，并连接到您的私有模型上下文协议（MCP）服务器。代理执行工具的沙箱及其访问的服务，均在您企业的既定边界内运行，受您的安全和运行时控制。"
pubDate: "2026-05-19"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents-updates"
---
            从今天起，Claude 托管代理可以在您控制的沙箱中运行，并连接到您的私有模型上下文协议（MCP）服务器。代理执行工具的沙箱及其访问的服务，均在您企业的既定边界内运行，受您的安全和运行时控制。

沙箱可在您自己的基础设施上运行，也可借助 [Cloudflare](https://developers.cloudflare.com/sandbox/claude-managed-agents/)、[Daytona](https://www.daytona.io/docs/en/guides/claude/claude-managed-agents)、[Modal](https://github.com/modal-labs/claude-managed-agents-modal-sandbox/tree/main) 或 [Vercel](https://vercel.com/kb/guide/run-claude-managed-agent-tools-with-vercel-sandbox) 等托管提供商为您处理计算和隔离。

在 Claude 平台上，[自托管沙箱](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes)已进入公开测试阶段，MCP 隧道处于研究预览阶段（[申请访问权限](https://claude.com/form/claude-managed-agents)）。

## **将代理执行保持在您的边界内**

借助自托管沙箱，您可以将敏感文件、软件包和服务保留在自己的基础设施中，或交由托管沙箱提供商处理。负责编排、上下文管理和错误恢复的[代理循环](https://www.anthropic.com/engineering/managed-agents)保留在 Anthropic 的基础设施上，而工具执行则转移到您自己配置的环境中。

在您的边界内，网络策略、审计日志和安全工具均已就位，文件和仓库不会外泄。您还可以控制计算资源：资源大小和运行时镜像由您方设定，因此执行长时间构建或图像生成等计算密集型任务的代理，能够获得任务所需的 CPU、内存和容量。

## **选择您的沙箱客户端**

您可以使用任何沙箱客户端，或从我们支持的提供商之一开始：

* [**Cloudflare**](https://developers.cloudflare.com/sandbox/claude-managed-agents/) 使用微虚拟机（microVM）和更轻量级的隔离机制大规模运行沙箱。出站网络请求由您控制，支持零信任密钥注入、可定制的代理以审计、重路由或修改出口流量，并能通过 Cloudflare 网络连接到内部服务。[**Amplitude**](https://amplitude.com/blog/design-agent) 正在基于托管代理和 Cloudflare 构建 Design Agent，这是一个用于品牌化生产 UI 和营销设计的内部工具，旨在实现更严格的观测性和控制。
* [**Daytona**](https://www.daytona.io/docs/en/guides/claude/claude-managed-agents) 沙箱是完全可组合的计算机，具有长期运行和有状态特性。同一原语既可支持快速突发任务，也可支持运行数小时的代理。会话运行期间，沙箱可通过 SSH 或经过身份验证的预览 URL 保持可访问，也可暂停并恢复，完整保留状态。[**Clay's**](http://clay.com/) GTM 工程代理 Sculptor 在托管代理和 Daytona 上自主构建、测试和监控工作流。
* [**Modal**](https://modal.com/blog/introducing-claude-managed-agents-with-modal-sandboxes) 是一个专为 AI 工作负载构建的云平台，其沙箱与 Modal 的函数、存储和网络原语共享相同基础，为您提供构建生产级 AI 系统所需的一切。Modal 的自定义容器运行时可在任何镜像上实现亚秒级启动，可扩展到数十万个并发沙箱，并按需提供 CPU 和 GPU 资源。
* [**Vercel**](https://vercel.com/kb/guide/run-claude-managed-agent-tools-with-vercel-sandbox) 沙箱结合了虚拟机安全性、VPC 对等连接和自带云功能，启动时间仅为毫秒级。托管代理负责处理模型、工具和会话状态，而 Vercel 沙箱防火墙在网络边界注入凭据，使其永远不会进入沙箱。[**Rogo**](https://rogo.ai/) 是一个面向机构金融的 AI 平台，正在基于托管代理和 Vercel 沙箱构建分析师代理，以安全处理其专有数据。

## **连接到私有网络内的服务**

借助 [MCP 隧道](http://platform.claude.com/docs/en/agents-and-tools/mcp-tunnels/overview)，您的代理可以访问私有网络内的 MCP 服务器，而无需将其暴露在公共互联网上。内部数据库、私有 API、知识库和工单系统都成为代理可以调用的工具。您部署的轻量级网关仅建立单个出站连接，无需入站防火墙规则，无需公共端点，且流量全程加密。

托管代理和消息 API 均支持 MCP 隧道。组织管理员可通过 [Claude 控制台](https://platform.claude.com/) 中的工作区设置管理 MCP 隧道。

## **开始使用**

自托管沙箱和 MCP 隧道均基于托管代理支持的相同核心原语运行。自托管沙箱已进入公开测试阶段，MCP 隧道处于研究预览阶段。要开始使用 MCP 隧道，请[申请访问权限](https://claude.com/form/claude-managed-agents)。

探索我们的[文档](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes)以了解更多信息，按照我们的[操作指南](https://github.com/anthropics/claude-cookbooks/tree/main/managed_agents/self_hosted_sandboxes)设置沙箱提供商，或在 [Claude 控制台](https://platform.claude.com/) 中部署您的第一个代理。
