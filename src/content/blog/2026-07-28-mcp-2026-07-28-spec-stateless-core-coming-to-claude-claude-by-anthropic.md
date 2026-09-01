---
title: "MCP 2026-07-28 规范：无状态核心，即将登陆 Claude | Anthropic 出品 Claude"
description: "---"
pubDate: "2026-07-28"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/bringing-mcp-2026-07-28-to-claude"
---

# MCP 2026-07-28 规范：无状态核心，即将登陆 Claude | Anthropic 出品 Claude

---

模型上下文协议的第五个规范版本，[**MCP 2026-07-28**](https://modelcontextprotocol.io/specification/2026-07-28)**，** 现已正式发布。最新规范将 MCP 转向无状态核心架构，同时强化授权机制并正式推出官方扩展。Claude 各产品正在陆续上线相关支持。

## **MCP 新特性** 

MCP 的月度 SDK 下载量近期已突破 4 亿次，今年实现了 4 倍增长，已成为连接 AI 智能体与应用的事实行业标准。MCP 2026-07-28 是迄今为止最重要的规范版本之一：**无状态核心。** MCP 从双向有状态协议转向请求/响应模型。服务器现可部署于无服务器和边缘基础设施上。这简化了为 Claude 构建 MCP 服务器的体验，并使其随采用规模增长而轻松扩展。

**标准化扩展。** [MCP 应用](https://modelcontextprotocol.io/extensions/apps/overview) 和 [任务](https://modelcontextprotocol.io/extensions/tasks/overview) 现已纳入版本化扩展框架，为开发者提供了一条正式途径，可在不修改核心协议的前提下添加交互式界面和长时间运行任务等能力。  
  
**授权加固。** 授权机制现已与生产环境的 OAuth 2.0 和 OIDC 部署对齐，MCP 服务器可直接连接 Entra 或 Okta 等企业身份系统，无需任何变通方案。

自 beta 版发布以来，生态系统中的各家公司已与 MCP 社区一同基于新规范开展开发：
