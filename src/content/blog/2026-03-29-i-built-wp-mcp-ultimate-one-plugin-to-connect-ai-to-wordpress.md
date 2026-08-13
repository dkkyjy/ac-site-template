---
title: "我构建了 WP MCP Ultimate——一个插件将 AI 连接到 WordPress"
description: "---"
pubDate: "2026-03-29"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/wp-mcp-ultimate-wordpress-ai-plugin"
---

# 我构建了 WP MCP Ultimate——一个插件将 AI 连接到 WordPress  

---

## 我为什么构建这个
将 AI 连接到 WordPress 不应该需要三个插件。但之前确实如此。你需要 **MCP Adapter** 来处理协议层，**MCP Expose Abilities** 来将 WordPress 动作注册为工具，如果 WordPress 版本低于 6.9 还需要 **Abilities API** 填充库。三个来自不同作者的插件，三个更新周期，三个故障点。
每次 WordPress 更新时，总会有东西出问题。适配器会丢失端点注册。abilities 插件会与填充库冲突。文档中的配置片段指向的 URL 已经不复存在。我花在调试这个插件栈上的时间比实际用 AI 管理网站的时间还多。
所以我做了任何受挫的开发者都会做的事——我将所有功能合并到一个插件中。[WP MCP Ultimate](https://github.com/AgriciDaniel/wp-mcp-ultimate) 是一个自包含的 MCP 服务器，一键安装，让任何兼容 MCP 的 AI 客户端完全访问你的 WordPress 站点。无依赖、无冲突、无胶带式修补。
## WP MCP Ultimate 能做什么
一个插件。**覆盖 13 个领域的 58 项 WordPress 能力。** 每个动作都通过一个 3 工具元模式进行：`discover-abilities` 列出可用能力，`get-ability-info` 返回特定能力的模式，`execute-ability` 执行它。AI 客户端无需猜测哪些是可能的——它直接询问插件。
以下是完整分类：
| 领域 | 能力数 | AI 能做什么 |
| --- | --- | --- |
| 文章 | 6 | 列出、获取、创建、更新、删除、部分修改内容 |
| 页面 | 6 | 完整的增删改查 + 内容部分修改 |
| 分类法 | 4 | 分类目录和标签——列出和创建 |
| 搜索 | 1 | 全文内容搜索 |
| 修订 | 2 | 列出和查看文章修订 |
| 媒体 | 5 | 上传、列出、获取、更新、删除文件 |
| 用户 | 6 | 完整的用户管理及角色分配 |
| 插件 | 6 | 上传、安装、启用、停用、删除 |
| 菜单 | 7 | 创建菜单、添加项目、分配位置 |
| 小工具 | 3 | 列出侧边栏和可用小工具 |
| 评论 | 6 | 审核、回复、创建、删除 |
| 设置 | 3 | 获取、更新、列出站点设置 |
| 系统 | 3 | 瞬态、调试日志、切换调试模式 |
| 总计 | 58 |  |
该插件兼容 **MCP 协议版本 2025-06-18** 并使用 **可流式 HTTP 传输**——而非较旧的 SSE（服务器发送事件）模式。这一点很重要，因为可流式 HTTP 是双向的，不需要长连接，这意味着它能在通常 30 秒后杀死 SSE 连接的 CDN 和负载均衡器后可靠工作。
它还包含冲突检测。如果你的站点仍安装有旧的 MCP Adapter、MCP Expose Abilities 或 Abilities API 插件，仪表板会发出警告并说明为什么应该停用它们。对于运行低于 6.9 版本的 WordPress 站点，**WP MCP Ultimate 捆绑了自有的 Abilities API 填充库**——无需额外插件。
## 2 分钟设置
三步。无需手动编辑配置文件，无需终端命令，无需寻找 API 密钥。
1. **安装插件** – 从 [v1.1.0 发布页面](https://github.com/AgriciDaniel/wp-mcp-ultimate/releases/tag/v1.1.0) 下载 zip，然后在 WordPress 后台通过 插件 > 安装插件 > 上传插件 上传并激活。
2. **生成 API 密钥** – 前往 工具 > MCP Ultimate，点击“生成”。该插件会自动创建一个 WordPress 应用密码。
3. **复制配置片段** – 仪表板会显示可直接粘贴到 Claude Code、Claude Desktop 和 Cursor 的配置。复制并粘贴到 AI 客户端的设置中。
对于 Claude Code，配置放在 `~/.claude/settings.json`：
```
{
  "mcpServers": {
    "wordpress": {
      "type": "streamable-http",
      "url": "https://你的网站.com/wp-json/mcp/wp-mcp-ultimate",
      "headers": {
        "Authorization": "Basic BASE64_凭据"
      }
    }
  }
}
```
仪表板会自动生成 Base64 凭据。无需手动编码。连接后，你的 AI 客户端可以通过元工具模式自动发现所有 58 项能力。
## v1.1.0 修复了什么
初始版本有三个 bug，导致首次使用体验不佳。这些都在 v1.1.0 中修复了：
* **媒体上传崩溃** – 通过 MCP 上传图片失败，因为 `media_handle_sideload()` 未定义。WordPress 仅在后管理上下文中加载该函数，而 MCP 请求是通过 REST API 进行的。修复程序在任何媒体操作之前加载所需的 admin 包含文件。
* **错误的端点 URL** – 所有配置片段（仪表板、README 和设置指南中）都指向 `/sse` 并使用 SSE 传输类型。正确的端点是 `/wp-json/mcp/wp-mcp-ultimate` 使用可流式 HTTP 传输。每个片段都已更正。
* **错误的配置路径** – Claude Code 的配置片段显示的是 `~/.claude.json` 而不是正确的 `~/.claude/settings.json`。一个小的拼写错误，但给遵循文档的人带来了很大麻烦。
能力数量也从 57 更正为 58——一个小工具能力从注册表中遗漏了。详情请见完整的 [v1.1.0 发布说明](https://github.com/AgriciDaniel/wp-mcp-ultimate/releases/tag/v1.1.0)。
## 生态系统——工具如何连接
WP MCP Ultimate 并非孤立存在。它曾是我在 AI 工具与真实 WordPress 站点之间缺失的桥梁。以下是各组件如何协同工作：
AI 内容流水线
Claude Blog → 编写内容 → Claude SEO → 优化内容 → WP MCP Ultimate → 推送至站点 → WordPress → 真实站点 → Rankenstein → 监控表现
内容从左向右流动。数据通过 Rankenstein 返回。
WP MCP Ultimate 是 AI 工具与真实站点之间的桥梁。
[Claude Blog](https://claude-blog.md)（[GitHub](https://github.com/AgriciDaniel/claude-blog)）使用其 [17 命令、100 点评分系统](/blog/claude-code-blog-writer) 编写 SEO 优化的内容。[Claude SEO](https://claude-seo.md)（[GitHub](https://github.com/AgriciDaniel/claude-seo)）使用 [9 个并行代理进行完整技术审计](/blog/claude-code-seo-stack)。但在此之前，最后一步是手动的——我必须将优化后的内容手动复制粘贴到 WordPress 中。
WP MCP Ultimate 弥合了这一鸿沟。Claude Code 现在可以在一个会话中撰写博客文章、为搜索优化并发布到 WordPress——全程无需离开终端。[Rankenstein](https://rankenstein.pro) 随后监控已发布内容在搜索中的表现，将数据反馈到下一个优化循环。
这是我一直在构建的 [AI 营销自动化栈](/blog/ai-marketing-automation-stack)。WP MCP Ultimate 是使其端到端完整的那个组件。
## 安全性与下一步计划
我将直接说明：**我进行了完整的安全审计，发现了 7 个需要在 v2.0 中解决的问题。** 当前版本依赖 WordPress 内置的身份验证（通过 HTTPS 使用基本认证的应用密码），这对 REST API 插件来说是标准的。但我想做一些改进：
* **所有状态更改操作都需要 nonce 验证**（目前仅使用应用密码认证）
* **基于能力的权限检查** 已实现，但需要对边缘情况进行更细致的审查
* **会话管理** 需要速率限制和连接跟踪
* **输入清理** 遵循 WordPress 编码标准，但需要对媒体上传和插件安装等复杂输入进行额外验证
这些都不是关键漏洞——该插件需要管理员凭据和 HTTPS。但我希望 v2.0 能够为生产环境的大规模运行进行加固。如果你发现安全问题，请通过 [GitHub 安全公告](https://github.com/AgriciDaniel/wp-mcp-ultimate/security/advisories/new) 负责任地报告。
v2.0 的路线图还包括用于实时通知的 webhook 支持、主题开发者的自定义能力注册以及多站点网络支持。
## 观看完整演示
这个 10 分钟的讲解涵盖了完整设置——从安装插件到通过 Claude Code 管理文章、上传媒体和审核评论：
（此处应有视频嵌入）
## 开始使用
WP MCP Ultimate 是开源的，采用 GPL-2.0 许可，且完全免费。无高级版、无功能限制。全部 58 项能力对所有人开放。
* **安装它** – [从 GitHub 下载 v1.1.0](https://github.com/AgriciDaniel/wp-mcp-ultimate/releases/tag/v1.1.0)
* **给项目加星** – [github.com/AgriciDaniel/wp-mcp-ultimate](https://github.com/AgriciDaniel/wp-mcp-ultimate)
* **报告 Bug** – 在 GitHub 上提 issue。欢迎 PR。
* **加入社区** – [AI 营销中心](https://www.skool.com/ai-marketing-hub)（免费，4500+ 成员）或 [AI 营销中心 Pro](https://www.skool.com/ai-marketing-hub-pro)（$88/月）获取工作流模板和直接支持。
如果你正在管理 WordPress 站点并使用 Claude Code，这个插件能将你的终端变成一个完整的 WordPress 管理面板。先在测试站点上试用，熟悉各项能力后再部署到生产环境。设置只需 2 分钟。它为你节省的时间是永久的。
## 常见问题
### 问：什么是 MCP（模型上下文协议）？
MCP 是 Anthropic 创建的一个开放协议，用于标准化 AI 应用程序连接外部工具和数据源的方式。可以把它想象成 AI 的 USB——一个通用接口，让任何兼容 MCP 的客户端（Claude Code、Claude Desktop、Cursor）都能使用共享语言与任何 MCP 服务器（WordPress、数据库、API）通信。WP MCP Ultimate 将你的 WordPress 站点变成了一个 MCP 服务器。
### 问：WP MCP Ultimate 能用于 ClassicPress 吗？
目前不能。该插件依赖 WordPress 的 REST API 基础设施和应用密码系统，而 ClassicPress 已经与之分叉。ClassicPress 支持不在路线图上，但代码库是 GPL-2.0 许可的——任何人都可以 fork 并适配。
### 问：在生产站点上使用安全吗？
该插件要求通过 HTTPS 提供管理员凭据，并尊重 WordPress 的能力检查。对于大多数单站点 WordPress 安装，这等同于登录 wp-admin。话虽如此，我建议先在测试站点上了解每个能力的作用，然后再连接生产站点。v2.0 版本将增加额外的安全加固。
### 问：价格是多少？
完全免费。永久免费。GPL-2.0 许可。无高级版本、无功能限制、无使用限制。该插件在 [GitHub](https://github.com/AgriciDaniel/wp-mcp-ultimate) 上完全开源。
### 问：这与直接使用 WordPress REST API 有何不同？
WordPress REST API 是一个通用 HTTP API——你的 AI 客户端需要知道每个操作的确切端点、认证头、请求格式和响应结构。WP MCP Ultimate 将所有这些封装在 MCP 协议之后，AI 客户端本身已经原生理解该协议。AI 自动发现可用能力、获取每个能力的类型模式，并通过单个标准化接口执行它们。这就像是给某人一张地图与给某人 GPS 的区别。
## 相关文章
* [Claude Code 刚刚取代了你的整个 SEO 栈](/blog/claude-code-seo-stack) – claude-seo 如何使用 9 个并行代理进行完整站点审计
* [Claude Code 刚刚取代了你的博客写手](/blog/claude-code-blog-writer) – claude-blog 的 100 点评分系统用于双重优化内容
* [AI 营销自动化：我每天使用的开源栈](/blog/ai-marketing-automation-stack) – WP MCP Ultimate 现在补齐的完整栈
* [2026 年最佳的 Claude Code 技能](/blog/best-claude-code-skills-2026) – WP MCP Ultimate 在更广泛生态系统中的位置
加入 4500+ AI 营销建设者
获取工作流模板、自动化蓝图，并与 SEO、代理商所有者和创作者建立联系。
[免费加入 →](https://www.skool.com/ai-marketing-hub)
