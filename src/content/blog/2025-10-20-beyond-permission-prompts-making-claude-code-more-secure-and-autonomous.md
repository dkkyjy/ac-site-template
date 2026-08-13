---
title: "超越权限提示：让 Claude Code 更安全、更自主"
description: "在 [Claude Code](https://www.claude.com/product/claude-code) 中，Claude 与你一起编写、测试和调试代码，浏览你的代码库、编辑多个文件并运行命令以验证其工作。给予 Claude 对代码库和文件如此大的访问权限可能会带来风险，尤其是在提示注入的情况下。 为了帮助解决这一问题，我们在 Claude Code 中引入了两项基于沙箱构建的新功能"
pubDate: "2025-10-20"
heroImage: "/post_img.png"
tags: ["anthropic-engineering"]
originalLink: "https://www.anthropic.com/engineering/claude-code-sandboxing"
---

在 [Claude Code](https://www.claude.com/product/claude-code) 中，Claude 与你一起编写、测试和调试代码，浏览你的代码库、编辑多个文件并运行命令以验证其工作。给予 Claude 对代码库和文件如此大的访问权限可能会带来风险，尤其是在提示注入的情况下。

为了帮助解决这一问题，我们在 Claude Code 中引入了两项基于沙箱构建的新功能，这两项功能都旨在为开发者提供更安全的工作环境，同时让 Claude 能够以更少的权限提示更自主地运行。在我们的内部使用中，我们发现沙箱安全地将权限提示减少了 84%。通过定义 Claude 可以自由工作的设定边界，它们提高了安全性和自主性。

### **确保用户在 Claude Code 上的安全**

Claude Code 基于权限模型运行：默认情况下，它是只读的，这意味着它在进行修改或运行任何命令之前会请求权限。有一些例外情况：我们会自动允许诸如 echo 或 cat 之类的安全命令，但大多数操作仍然需要明确的批准。

不断点击“批准”会拖慢开发周期，并可能导致“批准疲劳”，即用户可能不会密切关注他们批准的内容，从而使开发安全性降低。

为了解决这个问题，我们为 Claude Code 推出了沙箱功能。

## **沙箱：一种更安全、更自主的方法**

沙箱创建了预定义的边界，Claude 可以在其中更自由地工作，而不是为每个操作都请求权限。启用沙箱后，你将获得大幅减少的权限提示并提高安全性。

我们的沙箱方法基于操作系统级功能构建，以实现两个边界：

1. **文件系统隔离**，确保 Claude 只能访问或修改特定目录。这在防止被提示注入的 Claude 修改敏感系统文件方面尤为重要。
2. **网络隔离**，确保 Claude 只能连接到经过批准的服务器。这可以防止被提示注入的 Claude 泄露敏感信息或下载恶意软件。

值得注意的是，有效的沙箱需要*同时*具备文件系统和网络隔离。如果没有网络隔离，被攻破的代理可能会窃取 SSH 密钥等敏感文件；如果没有文件系统隔离，被攻破的代理很容易逃脱沙箱并获得网络访问权限。正是通过同时使用这两种技术，我们才能为 Claude Code 用户提供更安全、更快速的代理体验。

### Claude Code 中的两项新沙箱功能

#### **沙箱化 bash 工具：无需权限提示的安全 bash 执行**

我们推出[一种新的沙箱运行时](https://docs.claude.com/en/docs/claude-code/sandboxing)，作为研究预览版以 beta 形式提供，它允许你精确定义代理可以访问哪些目录和网络主机，而无需启动和管理容器的开销。这可用于沙箱化任意进程、代理和 MCP 服务器。它也可以作为[开源研究预览版](https://github.com/anthropic-experimental/sandbox-runtime)使用。

在 Claude Code 中，我们使用此运行时对 bash 工具进行沙箱化，这使得 Claude 能够在设定的限制范围内运行命令。在安全的沙箱内部，Claude 可以更自主地运行，并安全地执行命令而无需权限提示。如果 Claude 试图访问沙箱*外部*的内容，你会立即收到通知，并可以选择是否允许。

我们基于操作系统级原语（例如 [Linux bubblewrap](https://github.com/containers/bubblewrap) 和 MacOS seatbelt）构建了此功能，以在操作系统级别强制执行这些限制。它们不仅涵盖 Claude Code 的直接交互，还涵盖由命令生成的任何脚本、程序或子进程。如上所述，此沙箱强制执行以下两项：

1. **文件系统隔离**，通过允许对当前工作目录进行读写访问，但阻止修改其外部的任何文件。
2. **网络隔离**，通过仅允许通过连接到沙箱外部代理服务器的 Unix 域套接字进行互联网访问。此代理服务器强制限制进程可以连接的域，并处理新请求域的用户确认。如果你想要更高的安全性，我们还支持自定义此代理以对传出流量强制执行任意规则。

这两个组件都是可配置的：你可以轻松选择允许或禁止特定的文件路径或域。

Claude Code 的沙箱架构通过文件系统和网络控制隔离代码执行，自动允许安全操作，阻止恶意操作，并仅在需要时请求权限。

沙箱确保即使成功的提示注入也能被完全隔离，并且不会影响整体用户安全。这样，被攻破的 Claude Code 无法窃取你的 SSH 密钥，也无法向攻击者的服务器发送信号。

要开始使用此功能，请在 Claude Code 中运行 /sandbox，并查看[关于我们安全模型的更多技术细节](https://docs.claude.com/en/docs/claude-code/sandboxing)。

为了使其他团队更容易构建更安全的代理，我们已[开源](https://github.com/anthropic-experimental/sandbox-runtime)了此功能。我们相信其他团队应该考虑将这项技术用于他们自己的代理，以增强其代理的安全态势。

#### **网页版 Claude Code：在云端安全运行 Claude Code**

今天，我们还发布了[网页版 Claude Code](https://docs.claude.com/en/docs/claude-code/claude-code-on-the-web)，使用户能够在云端的隔离沙箱中运行 Claude Code。网页版 Claude Code 在隔离的沙箱中执行每个 Claude Code 会话，在其中它可以安全可靠地完全访问其服务器。我们设计此沙箱是为了确保敏感凭据（例如 git 凭据或签名密钥）永远不会与 Claude Code 一起位于沙箱内部。这样，即使沙箱中运行的代码被攻破，用户也能免受进一步伤害。

网页版 Claude Code 使用自定义代理服务，透明地处理所有 git 交互。在沙箱内部，git 客户端使用自定义构建的范围凭据通过此服务进行身份验证。代理验证此凭据以及 git 交互的内容（例如，确保它仅推送到配置的分支），然后在将请求发送到 GitHub 之前附加正确的身份验证令牌。

Claude Code 的 Git 集成通过一个安全代理路由命令，该代理验证身份验证令牌、分支名称和仓库目的地——从而在防止未经授权的推送的同时允许安全的版本控制工作流。

## 入门指南

我们新的沙箱化 bash 工具和网页版 Claude Code 为使用 Claude 进行工程工作的开发者在安全性和生产力方面提供了显著的改进。

要开始使用这些工具：

1. 在 Claude 中运行 `/sandbox`，并查看[我们的文档](https://docs.claude.com/en/docs/claude-code/sandboxing)以了解如何配置此沙箱。
2. 访问 [claude.com/code](http://claude.ai/redirect/website.v1.a23c2e9e-ccfe-4a56-aed3-c4cdbd962cc4/code) 试用网页版 Claude Code。

或者，如果你正在构建自己的代理，请查看我们[开源的沙箱化代码](https://github.com/anthropic-experimental/sandbox-runtime)，并考虑将其集成到你的工作中。我们期待看到你的成果。

要了解有关网页版 Claude Code 的更多信息，请查看我们的[发布博客文章](https://www.anthropic.com/news/claude-code-on-the-web)。

## 致谢

本文由 David Dworken 和 Oliver Weller-Davies 撰写，感谢 Meaghan Choi、Catherine Wu、Molly Vorwerck、Alex Isken、Kier Bradwell 和 Kevin Garcia 的贡献。
