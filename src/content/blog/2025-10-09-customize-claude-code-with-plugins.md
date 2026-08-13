---
title: "使用插件自定义 Claude Code"
description: "斜杠命令、代理、MCP 服务器和钩子都是你可以用来自定义 Claude Code 体验的扩展点。在我们推出这些功能的过程中，我们看到用户构建了越来越强大的配置，并希望与团队成员和更广泛的社区分享。我们构建了插件来简化这一过程。"
pubDate: "2025-10-09"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-code-plugins"
---
斜杠命令、代理、MCP 服务器和钩子都是你可以用来自定义 Claude Code 体验的扩展点。在我们推出这些功能的过程中，我们看到用户构建了越来越强大的配置，并希望与团队成员和更广泛的社区分享。我们构建了插件来简化这一过程。

插件是一种轻量级的方式，用于打包和分享以下任意组合：

* **斜杠命令**：为常用操作创建自定义快捷方式
* **子代理**：安装针对特定开发任务的专业代理
* **MCP 服务器**：通过模型上下文协议连接工具和数据源
* **钩子**：在 Claude Code 工作流的关键节点自定义其行为

你可以直接在 Claude Code 中使用 `/plugin` 命令安装插件，该功能现已进入公开测试阶段。它们设计为可按需开启和关闭。当你需要特定功能时启用它们，不需要时禁用它们，以减少系统提示的上下文和复杂性。

未来，插件将是我们打包和分享 Claude Code 自定义功能的标准方式，并且随着我们添加更多扩展点，我们将继续改进这一格式。

### 使用场景

插件帮助你围绕一套共享的最佳实践标准化 Claude Code 环境。常见的插件使用场景包括：

* **强制执行标准**：工程负责人可以通过使用插件确保团队在代码审查或测试工作流中运行特定的钩子，从而保持一致性
* **支持用户**：例如，开源维护者可以提供斜杠命令，帮助开发者正确使用他们的软件包
* **分享工作流**：构建了提升生产力工作流的开发者——如调试设置、部署管道或测试框架——可以轻松与他人分享
* **连接工具**：需要通过 MCP 服务器连接内部工具和数据源的团队，可以使用具有相同安全和配置协议的插件来加速流程
* **打包自定义功能**：框架作者或技术负责人可以打包多个协同工作的自定义功能，用于特定用例

### 插件市场

为了更轻松地分享这些自定义功能，任何人都可以构建和托管插件，并创建插件市场——即经过策划的集合，其他开发者可以在其中发现和安装插件。

你可以使用插件市场与社区分享插件，在你的组织内分发已批准的插件，并基于现有的解决方案构建常见开发挑战的解决方案。

要托管一个市场，你只需要一个 git 仓库、GitHub 仓库或包含格式正确的 `.claude-plugin/marketplace.json` 文件的 URL。详情请参阅我们的文档。

要使用市场中的插件，请运行 `/plugin marketplace add user-or-org/repo-name`，然后通过 `/plugin` 菜单浏览和安装插件。

### 发现新的市场

插件市场放大了我们社区已经开发的最佳实践，社区成员正在引领这一方向。例如，工程师 Dan Ávila 的[插件市场](https://www.aitmpl.com/plugins)提供了用于 DevOps 自动化、文档生成、项目管理和测试套件的插件，而工程师 Seth Hobson 在他的 [GitHub 仓库](https://github.com/wshobson/agents)中策划了超过 80 个专业子代理，让开发者可以通过插件即时访问。

你还可以查看我们为 PR 审查、安全指导、[Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) 开发以及甚至用于创建新插件的元插件而开发的几个[示例插件](https://github.com/anthropics/claude-code)。

### 开始使用

插件现已对所有 Claude Code 用户进入公开测试阶段。使用 `/plugin` 命令安装它们，它们将在你的终端和 VS Code 中工作。

查看我们的文档以[开始使用](https://docs.claude.com/en/docs/claude-code/plugins-reference)、[构建你自己的插件](https://docs.claude.com/en/docs/claude-code/plugins)或[发布一个市场](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)。要查看插件的实际效果，请尝试我们用于开发 Claude Code 的这个多代理工作流：

`/plugin marketplace add anthropics/claude-code`

```
/plugin marketplace add anthropics/claude-code
```

```
/plugin install feature-dev
```
