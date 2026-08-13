---
title: "Claude 托管代理新增功能：自托管沙盒与 MCP 隧道 | Claude by Anthropic"
description: "---"
pubDate: "2026-05-19"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-managed-agents-updates"
---

# Claude 托管代理新增功能：自托管沙盒与 MCP 隧道 | Claude by Anthropic

---

从今天开始，[Claude 托管代理](https://claude.com/blog/claude-managed-agents) 可以在您控制的沙盒中运行，并连接到您的私有模型上下文协议（MCP）服务器。代理执行工具所在的沙盒以及它所访问的服务，都在您企业既定的边界内运行，受您的安全与运行时控制。

沙盒可以运行在您自己的基础设施上，也可以使用 [Cloudflare](https://developers.cloudflare.com/sandbox/claude-managed-agents/)、[Daytona](https://www.daytona.io/docs/en/guides/claude/claude-managed-agents)、[Modal](https://github.com/modal-labs/claude-managed-agents-modal-sandbox/tree/main) 或 [Vercel](https://vercel.com/kb/guide/run-claude-managed-agent-tools-with-vercel-sandbox) 等托管提供商来为您处理计算和隔离。

在 Claude 平台上，[自托管沙盒](https://platform.claude.com/docs/en/managed-agents/self-hosted-sandboxes) 现已公开测试，MCP 隧道处于研究预览阶段（[申请访问权限](https://claude.com/form/claude-managed-agents)）。

## **`自托管沙盒：将代理执行保留在您的边界内`**

自托管沙盒让 Claude 托管代理能够在您控制的基础设施上或通过托管沙盒提供商执行工具。代码执行、敏感文件、包、服务和数据都保留在您企业边界内，受您的安全与运行时控制。

使用自托管沙盒，您可以将敏感文件、包和服务保留在您自己的基础设施或托管沙盒提供商处。负责编排、上下文管理和错误恢复的[代理循环](https://www.anthropic.com/engineering/managed-agents) 保留在 Anthropic 的基础设施上，而工具执行则迁移到您自己配置的环境中。

在您的边界内部，网络策略、审计日志和安全工具已经就位，文件和存储库不会外泄。您还可以控制计算资源：资源大小和运行时镜像在您这边设置，因此运行计算密集型工作（如长时间构建或图像生成）的代理可以获得任务所需的 CPU、内存和容量。

## **选择您的沙盒客户端**

您可以使用任何想要的沙盒客户端，或者从我们支持的提供商之一开始：

* [**Cloudflare**](https://developers.cloudflare.com/sandbox/claude-managed-agents/) 使用微虚拟机和更轻量的隔离单元大规模运行沙盒。出站网络请求由您控制，支持零信任密钥注入、可自定义代理来审计、重路由或修改出站流量，并能通过 Cloudflare 网络连接到内部服务。[**Amplitude**](https://amplitude.com/blog/design-agent) 正在基于托管代理和 Cloudflare 构建 Design Agent，这是一个用于品牌化生产 UI 和营销设计的内部工具，以实现更紧密的可观测性和控制。
* [**Daytona**](https://www.daytona.io/docs/en/guides/claude/claude-managed-agents) 沙盒是完整的可组合计算机，可长期运行且有状态。同一个原语既可以执行快速突发任务，也可以运行一个持续工作数小时的代理。在会话运行期间，沙盒可通过 SSH 或经过身份验证的预览 URL 保持可访问，也可以暂停并保留完整状态后恢复。[**Clay's**](http://clay.com/) GTM 工程代理 Sculptor 在托管代理和 Daytona 上自主构建、测试和监控工作流。
* [**Modal**](https://modal.com/blog/introducing-claude-managed-agents-with-modal-sandboxes) 是一个为 AI 工作负载构建的云平台，其沙盒与 Modal 的函数、存储和网络原语共享相同的基础，为您提供构建生产级 AI 系统所需的一切。Modal 的自定义容器运行时可在任何镜像上实现亚秒级启动，可扩展至数十万个并发沙盒，并按需提供 CPU 和 GPU 资源。
* [**Vercel**](https://vercel.com/kb/guide/run-claude-managed-agent-tools-with-vercel-sandbox) 沙盒结合了虚拟机安全性、VPC 对等连接和自带云功能，启动时间仅为毫秒级。托管代理处理模型、工具和会话状态，而 Vercel 沙盒防火墙在网络边界注入凭据，使其永远不会进入沙盒。[**Rogo**](https://rogo.ai/) 是一个面向机构金融的 AI 平台，正在基于托管代理和 Vercel 沙盒构建分析师代理，以安全地处理其专有数据。

## **MCP 隧道：连接到您私有网络内的服务**

[MCP 隧道](http://platform.claude.com/docs/en/agents-and-tools/mcp-tunnels/overview) 将 Claude 托管代理连接到您私有网络内的模型上下文协议（MCP）服务器，而无需将其暴露到公共互联网。内部数据库、私有 API、知识库和工单系统都成为代理可以调用的工具。您部署的一个轻量级网关建立单个出站连接，不需要入站防火墙规则，没有公共端点，并且流量端到端加密。

MCP 隧道在托管代理和 Messages API 中受支持。由组织管理员在 [Claude 控制台](https://platform.claude.com/) 的工作区设置中管理。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保您的整个最终响应（包括在执行指令过程中生成的任何章节标题或标题）仅使用 zh-CN 语言书写。
