---
title: "Gemini CLI 技巧与窍门"
description: "本指南涵盖约30个专业技巧，助你高效使用Gemini CLI进行Gemini 2.5、3.0及更高版本的智能编码"
pubDate: "2026-01-01"
heroImage: "/post_img.png"
tags: ["addy-osmani"]
originalLink: "https://addyosmani.com/blog/gemini-cli/"
---
**[Gemini CLI](https://github.com/google-gemini/gemini-cli)** 是一款开源AI助手，它将Google Gemini模型的能力直接带入你的[终端](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=The%20Gemini%20CLI%20is%20an,via%20a%20Gemini%20API%20key)。它作为一个对话式、“智能体”命令行工具运行——意味着它可以推理你的请求，选择工具（如运行shell命令或编辑文件），并执行多步骤计划来协助你的开发[工作流程](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=The%20Gemini%20CLI%20%20is,understanding%20of%20the%20developer%20workflow)。
实际上，Gemini CLI就像一个超强的结对编程伙伴和命令行助手。它擅长编码任务、调试、内容生成，甚至系统自动化，所有这些都通过自然语言提示完成。在深入探讨专业技巧之前，让我们快速回顾一下如何设置Gemini CLI并让它运行起来。
* [入门指南](#入门指南)
* [技巧1：使用 `GEMINI.md` 实现持久上下文](#技巧1使用-geminimd-实现持久上下文)
* [技巧2：创建自定义斜杠命令](#技巧2创建自定义斜杠命令)
* [技巧3：使用你自己的 `MCP` 服务器扩展Gemini](#技巧3使用你自己的-mcp-服务器扩展gemini)
* [技巧4：利用记忆添加与回忆](#技巧4利用记忆添加与回忆)
* [技巧5：使用检查点和 `/restore` 作为撤销按钮](#技巧5使用检查点和-restore-作为撤销按钮)
* [技巧6：读取Google文档、表格等更多内容](#技巧6读取google文档表格等更多内容)
* [技巧7：使用 `@` 引用文件和图像以提供显式上下文](#技巧7使用--引用文件和图像以提供显式上下文)
* [技巧8：即时创建工具（让Gemini构建辅助工具）](#技巧8即时创建工具让gemini构建辅助工具)
* [技巧9：使用Gemini CLI进行系统故障排除与配置](#技巧9使用gemini-cli进行系统故障排除与配置)
* [技巧10：YOLO模式——自动批准工具操作（谨慎使用）](#技巧10yolo模式自动批准工具操作谨慎使用)
* [技巧11：无头与脚本模式（在后台运行Gemini CLI）](#技巧11无头与脚本模式在后台运行gemini-cli)
* [技巧12：保存和恢复聊天会话](#技巧12保存和恢复聊天会话)
* [技巧13：多目录工作区——一个Gemini，多个文件夹](#技巧13多目录工作区一个gemini多个文件夹)
* [技巧14：借助AI整理和清理文件](#技巧14借助ai整理和清理文件)
* [技巧15：压缩长对话以保持在上下文窗口内](#技巧15压缩长对话以保持在上下文窗口内)
* [技巧16：使用 `!` 传递Shell命令（与终端对话）](#技巧16使用--传递shell命令与终端对话)
* [技巧17：将每个CLI工具视为潜在的Gemini工具](#技巧17将每个cli工具视为潜在的gemini工具)
* [技巧18：利用多模态AI——让Gemini看到图像等更多内容](#技巧18利用多模态ai让gemini看到图像等更多内容)
* [技巧19：自定义 `$PATH`（和工具可用性）以提高稳定性](#技巧19自定义-path和工具可用性以提高稳定性)
* [技巧20：通过令牌缓存和统计跟踪并减少令牌消耗](#技巧20通过令牌缓存和统计跟踪并减少令牌消耗)
* [技巧21：使用 `/copy` 快速复制到剪贴板](#技巧21使用-copy-快速复制到剪贴板)
* [技巧22：掌握 `Ctrl+C` 用于Shell模式和退出](#技巧22掌握-ctrlc-用于shell模式和退出)
* [技巧23：使用 `settings.json` 自定义Gemini CLI](#技巧23使用-settingsjson-自定义gemini-cli)
* [技巧24：利用IDE集成（VS Code）获取上下文和差异对比](#技巧24利用ide集成vs-code获取上下文和差异对比)
* [技巧25：使用 `Gemini CLI GitHub Action` 自动化仓库任务](#技巧25使用-gemini-cli-github-action-自动化仓库任务)
* [技巧26：启用遥测以获得洞察和可观测性](#技巧26启用遥测以获得洞察和可观测性)
* [技巧27：关注路线图（后台智能体等更多内容）](#技巧27关注路线图后台智能体等更多内容)
* [技巧28：使用 `Extensions` 扩展Gemini CLI](#技巧28使用-extensions-扩展gemini-cli)
* [技巧29：柯基模式彩蛋 🐕](#技巧29柯基模式彩蛋)
**安装：** 你可以通过npm安装Gemini CLI。要进行全局安装，请使用：
```
npm install -g @google/gemini-cli
```
或者使用 `npx` 无需安装即可运行：
```
npx @google/gemini-cli
```
Gemini CLI可在所有主流平台上使用（它使用Node.js/TypeScript构建）。安装完成后，只需在终端中运行 `gemini` 命令即可启动交互式[CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Interactive%20Mode%20,conversational%20session)。
**身份验证：** 首次使用时，你需要通过Gemini服务进行身份验证。你有两个选项：(1) **Google账户登录（免费层）**——这允许你免费使用Gemini 2.5 Pro，具有慷慨的使用限制（大约每分钟60个请求，每天1,000个请求）。启动时，Gemini CLI会提示你使用Google账户登录（无需[计费](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=%2A%20Google,Google%20AI%20Studio%2C%20then%20run)。(2) **API密钥（付费或更高层级访问）**——你可以从Google AI [Studio](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=1,key%20from%20Google%20AI%20Studio)获取API密钥，并设置环境变量 `GEMINI_API_KEY` 来使用[它](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Method%201%3A%20Shell%20Environment%20Variable,zshrc)。
使用API密钥可以提供更高的配额和企业级数据使用保护；付费/计费用途的提示不会用于训练，但日志可能会保留用于[安全](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=responses%20may%20be%20logged%20for,Google%20AI%20Studio%2C%20then%20run)。
例如，添加到你的shell配置文件：
```
export GEMINI_API_KEY="YOUR_KEY_HERE"
```
**基本用法：** 要启动交互式会话，只需运行 `gemini` 不带任何参数。你将看到一个 `gemini>` 提示符，可以在其中输入请求或命令。例如：
```
$ gemini
gemini> 创建一个使用SQLite的React食谱管理应用
```
然后你可以观察Gemini CLI创建文件、安装依赖、运行测试等，以完成你的请求。如果你更喜欢一次性调用（非交互式），请使用 `-p` 标志加上提示，例如：
```
gemini -p "总结附件文件的主要要点。@./report.txt"
```
这将输出单个响应并[退出](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=gemini)。你也可以将输入通过管道传递给Gemini CLI：例如，`echo "Count to 10" | gemini` 将通过[stdin](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=gemini%20,txt)提供提示。
**CLI界面：** Gemini CLI提供了一个丰富的类似REPL的界面。它支持**斜杠命令**（以 `/` 为前缀的特殊命令，用于控制会话、工具和设置）和**感叹号命令**（以 `!` 为前缀，用于直接执行shell命令）。我们将在下面的专业技巧中介绍其中的许多内容。默认情况下，Gemini CLI在安全模式下运行，任何修改系统的操作（写入文件、运行shell命令等）都会要求确认。当提出工具操作时，你将看到差异对比或命令，并提示（`Y/n`）批准或拒绝。这确保了AI不会在你不同意的情况下进行不必要的更改。
了解了基础知识后，让我们探索一系列专业技巧和隐藏功能，帮助你充分利用Gemini CLI。每个技巧首先提供一个简单的示例，然后是更深入的细节和细微差别。这些技巧融合了工具创建者（例如Taylor Mullen）和Google开发者关系团队以及更广泛社区的建议和见解，旨在成为Gemini CLI高级用户的**权威指南**。
**快速用例：** 停止在提示中重复自己。通过创建 `GEMINI.md` 文件提供项目特定的上下文或指令，这样AI始终拥有重要的背景知识，无需每次都[告知](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Context%20Files%20%28)。
在处理项目时，你通常会有一些总体细节——例如编码风格指南、项目架构或重要事实——你希望AI牢记。Gemini CLI允许你将这些编码到一个或多个 `GEMINI.md` 文件中。只需在项目中创建一个 `.gemini` 文件夹（如果尚不存在），并添加一个名为 `GEMINI.md` 的Markdown文件，其中包含你希望AI持久化的任何笔记或指令。例如：
```
# 项目凤凰 - AI助手
- 所有Python代码必须遵循PEP 8风格。
- 使用4个空格进行缩进。
- 用户正在构建一个数据管道；优先使用函数式编程范式。
```
将此文件放在项目根目录中（或子目录中以实现更精细的上下文）。现在，每当你在该项目中运行 `gemini` 时，它将自动将这些指令加载到[上下文](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Context%20Files%20%28)中。这意味着模型将*始终*准备好这些指令，避免了将相同的指导添加到每个提示中的需要。
**工作原理：** Gemini CLI使用分层上下文加载[系统](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Hierarchical%20Loading%3A%20The%20CLI%20combines,The%20loading%20order%20is)。它将结合**全局上下文**（来自 `~/.gemini/GEMINI.md`，你可以用于跨项目默认设置）与你的**项目特定 `GEMINI.md`**，甚至子文件夹中的上下文文件。更具体的文件会覆盖更通用的文件。你可以随时使用以下命令检查加载了哪些上下文：
```
/memory show
```
这将显示AI[看到](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,current%20conversation%20with%20a%20tag)的完整组合上下文。如果你对 `GEMINI.md` 进行了更改，请使用 `/memory refresh` 重新加载上下文，而无需重新启动[会话](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,current%20conversation%20with%20a%20tag)。
**专业提示：** 使用 `/init` 斜杠命令快速生成一个入门 `GEMINI.md`。在新项目中运行 `/init` 会创建一个模板上下文文件，其中包含检测到的技术栈、项目摘要等信息。然后你可以编辑和扩展该文件。对于大型项目，考虑将上下文分解为多个文件，并使用 `@include` 语法将它们**导入**到 `GEMINI.md` 中。例如，你的主 `GEMINI.md` 可以包含像 `@./docs/prompt-guidelines.md` 这样的行来引入额外的上下文[文件](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Modularizing%20Context%20with%20Imports%3A%20You,files)。这使你的指令保持井井有条。
通过精心制作的 `GEMINI.md`，你基本上给了Gemini CLI一个项目需求和约定的“记忆”。这种**持久上下文**会带来更相关的响应和更少来回的提示工程。
**快速用例：** 通过定义自己的斜杠命令来加速重复性任务。例如，你可以创建一个命令 `/test:gen`，根据描述生成单元测试，或 `/db:reset` 删除并重新创建测试数据库。这通过针对你的工作流程量身定制的单行命令扩展了Gemini CLI的功能。
Gemini CLI支持**自定义斜杠命令**，你可以在简单的配置文件中定义。在底层，这些本质上是预定义的提示模板。要创建一个，请在 `~/.gemini/` 下创建一个 `commands/` 目录用于全局命令，或在项目的 `.gemini/` 文件夹中创建用于项目特定的[命令](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Custom%20Commands)。在 `commands/` 内部，为每个新命令创建一个TOML文件。文件名格式决定了命令名称：例如，文件 `test/gen.toml` 定义了一个命令 `/test:gen`。
让我们通过一个示例来了解。假设你想要一个命令来根据需求描述生成单元测试。你可以创建 `~/.gemini/commands/test/gen.toml`，内容如下：
```
# 调用方式：/test:gen "测试描述"
description = "根据需求生成单元测试。"
prompt = """
你是一名专业的测试工程师。根据以下需求，请使用Jest框架编写一个全面的单元测试。
需求：
"""
```
现在，在重新加载或重新启动Gemini CLI后，你可以简单地输入：
```
/test:gen "确保登录按钮在成功后重定向到仪表板"
```
Gemini CLI将识别 `/test:gen`，并将 `` 替换为你提供的参数（在本例中为需求）。然后AI将相应地生成一个Jest单元[测试](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Example%3A%20%60)。`description` 字段是可选的，但在你运行 `/help` 或 `/tools` 列出可用命令时会用到。
这种机制非常强大——实际上，你可以用自然语言编写AI脚本。社区已经创建了许多有用的自定义命令。例如，Google的DevRel团队分享了一组*10个实用的工作流程命令*（通过一个开源仓库），演示了如何编写常见流程的脚本，如创建API文档、清理数据或设置样板[代码](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=,to%20generate%20a%20better%20output)。通过定义自定义命令，你将一个复杂的提示（或一系列提示）打包成一个可重用的快捷方式。
**专业提示：** 自定义命令还可以用于强制格式化或为某些任务对AI应用“角色”。例如，你可能有一个 `/review:security` 命令，它总是以“你是一名安全审计员……”作为提示的前缀，以审查代码漏洞。这种方法确保了AI在响应特定类别的任务时的一致性。
要与团队共享命令，你可以将TOML文件提交到项目仓库中（在 `.gemini/commands` 目录下）。拥有Gemini CLI的团队成员在项目工作时将自动获取这些命令。这是在团队中**标准化AI辅助工作流程**的好方法。
**快速用例：** 假设你希望Gemini与外部系统或非内置的自定义工具交互——例如，查询专有数据库，或与Figma设计集成。你可以通过运行一个自定义的 **[模型上下文协议（MCP）](/agentic-engineering/mcp/)服务器** 并将其接入Gemini [CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Extend%20the%20CLI%20with%20your,add%7Clist%7Cremove%3E%60%20commands)来实现。MCP服务器允许你向Gemini添加新的工具和能力，有效地**扩展智能体**。
Gemini CLI开箱即用带有几个MCP服务器（例如，支持Google搜索、代码执行沙箱等的服务器），你可以添加自己的服务器。MCP服务器本质上是一个外部进程（可以是本地脚本、微服务，甚至是云端点），它使用简单的协议来处理Gemini的任务。这种架构正是Gemini CLI如此[可扩展](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/#:~:text=,interactively%20within%20your%20scripts)的原因。
**MCP服务器示例：** 一些社区和Google提供的MCP集成包括一个**Figma MCP**（用于从Figma获取设计细节）、一个**剪贴板MCP**（用于读取/写入系统剪贴板）等。事实上，在一个内部演示中，Gemini CLI团队展示了一个“Google Docs MCP”服务器，允许直接将内容保存到Google [Docs](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=%2A%20Utilize%20the%20google,summary%20directly%20to%20Google%20Docs)。其理念是，每当Gemini需要执行内置工具无法处理的操作时，它可以委托给你的MCP服务器。
**如何添加一个：** 你可以通过 `settings.json` 或使用CLI配置MCP服务器。对于快速设置，请尝试CLI命令：
```
gemini mcp add myserver --command "python3 my_mcp_server.py" --port 8080
```
这将注册一个名为“myserver”的服务器，Gemini CLI将通过运行给定的命令（这里是一个Python模块）在端口8080上启动它。在 `~/.gemini/settings.json` 中，它将在 `mcpServers` 下添加一个条目。例如：
```
"mcpServers": {
  "myserver": {
    "command": "python3",
    "args": ["-m", "my_mcp_server", "--port", "8080"],
    "cwd": "./mcp_tools/python",
    "timeout": 15000
  }
}
```
此配置（基于官方文档）告诉Gemini如何启动MCP服务器以及[在哪里](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Example%20)。一旦运行，该服务器提供的工具将可用于Gemini CLI。你可以使用斜杠命令列出所有MCP服务器及其工具：
```
/mcp
```
这将显示任何已注册的服务器以及它们[暴露](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Command%20Description%20,List%20active%20extensions)的工具名称。
**MCP的力量：** MCP服务器可以提供**丰富的、多模态的结果**。例如，通过MCP提供的工具可以返回图像或格式化表格作为对Gemini [CLI](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Capabilities%3A)的响应的一部分。它们还支持OAuth 2.0，因此你可以通过MCP工具安全地连接到API（如Google的API、GitHub等），而无需暴露[凭据](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Extend%20the%20CLI%20with%20your,add%7Clist%7Cremove%3E%60%20commands)。本质上，如果你能编写代码，你就可以将其封装为MCP工具——将Gemini CLI变成一个编排许多服务的中心。
**默认与自定义：** 默认情况下，Gemini CLI的内置工具涵盖了很多内容（读取文件、网络搜索、执行shell命令等），但MCP让你更进一步。一些高级用户已经创建了MCP服务器来与内部系统交互或执行专门的数据处理。例如，你可以有一个 `database-mcp`，它提供一个 `/query_db` 工具用于在公司数据库上运行SQL查询，或者一个 `jira-mcp` 用于通过自然语言创建工单。
在创建自己的MCP服务器时，要注意安全性：默认情况下，自定义MCP工具需要确认，除非你将其标记为受信任。你可以使用设置来控制安全性，例如为服务器设置 `trust: true`（自动批准其工具操作），或者将特定的安全工具列入白名单，将危险工具列入黑[名单](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,takes%20precedence)。
简而言之，**MCP服务器解锁了无限的集成可能性**。它们是一个专业功能，让Gemini CLI成为你的AI助手和你需要它与之工作的任何系统之间的粘合剂。如果你有兴趣构建一个，请查看官方[MCP指南](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Transport%20)和社区示例。
**快速用例：** 通过将重要事实添加到其长期记忆中，让重要信息随时可供AI使用。例如，在找出数据库端口或API令牌后，你可以执行：
```
/memory add "我们的staging RabbitMQ在端口5673上"
```
这将存储该事实，以便你（或AI）以后不会[忘记](https://binaryverseai.com/gemini-cli-open-source-ai-tool/#:~:text=Gemini%20CLI%20Ultimate%20Agent%3A%2060,a%20branch%20of%20conversation)。然后你可以随时使用 `/memory show` 回忆记忆中的所有内容。
`/memory` 命令提供了一种简单但强大的*持久记忆*机制。当你使用 `/memory add <text>` 时，给定的文本会被附加到项目的全局上下文中（从技术上讲，它被保存到全局 `~/.gemini/GEMINI.md` 文件或项目的 [`GEMINI.md`](https://genmind.ch/posts/Howto-Supercharge-Your-Terminal-with-Gemini-CLI/#:~:text=,load%20memory%20from%20%60GEMINI.md) 文件中）。这有点像记笔记并将其钉在AI的虚拟公告板上。一旦添加，AI在未来的交互中（跨会话）将始终在提示上下文中看到该笔记。
考虑一个示例：你正在调试一个问题，并发现了一个不明显的见解（“配置标志 `X_ENABLE` 必须设置为 `true`，否则服务无法启动”）。如果你将此添加到记忆中，以后如果你或AI正在讨论相关问题，它不会忽略这个关键细节——它就在上下文中。
**使用 `/memory`：**
* `/memory add "<text>"` - 将事实或笔记添加到记忆（持久上下文）。这会立即用新条目更新 `GEMINI.md`。
* `/memory show` - 显示记忆的完整内容（即当前加载的组合上下文文件）。
* `/memory refresh` - 从磁盘重新加载上下文（如果你在Gemini CLI之外手动编辑了 `GEMINI.md` 文件，或者多人在协作编辑它，则很有用）。
由于记忆存储在Markdown中，你也可以手动编辑 `GEMINI.md` 文件来整理或组织信息。`/memory` 命令是为了在对话期间方便使用，这样你就不必打开编辑器。
**专业提示：** 这个功能非常适合“决策日志”。如果你在聊天中决定了一种方法或规则（例如，要使用的某个库，或商定的代码风格），请将其添加到记忆中。AI将随后回忆起该决定，并避免以后与之矛盾。在可能持续数小时或数天的长会话中，这尤其有用——通过保存关键点，你可以减轻模型在对话变长时忘记早期上下文的倾向。
另一个用途是个人笔记。由于 `~/.gemini/GEMINI.md`（全局记忆）会为所有会话加载，你可以将通用偏好或信息放在那里。例如，“用户的名字是Alice。说话要有礼貌，避免使用俚语。”这就像配置AI的角色或全局知识。只需注意，全局记忆适用于*所有*项目，所以不要用项目特定的信息来塞满它。
总之，**记忆添加与回忆**帮助Gemini CLI保持状态。把它想象成一个随着项目增长的知识库。使用它可以避免重复自己，或提醒AI它本来需要从头重新发现的事实。
**快速用例：** 如果Gemini CLI对你的文件进行了一系列你不满意的更改，你可以*立即回滚*到之前的状态。在启动Gemini时（或在设置中）启用检查点，并使用 `/restore` 命令像轻量级Git[还原](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=,Exit%20the%20Gemini%20CLI)一样撤销更改。`/restore` 将你的工作区回滚到保存的检查点；会话状态可能会受到影响，具体取决于检查点的捕获方式。
Gemini CLI的**检查点**功能充当了一个安全网。启用后，CLI会在每次[修改](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=When%20,snapshot%20before%20tools%20modify%20files)文件的工具执行*之前*拍摄项目文件的快照。如果出现问题，你可以恢复到最后一个已知的良好状态。这本质上是AI操作的版本控制，无需你每次都手动提交到Git。
**如何使用：** 你可以通过使用 `--checkpointing` 标志启动CLI来打开检查点：
```
gemini --checkpointing
```
或者，你可以通过添加到配置（在 [`settings.json`](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=%7B%20,true) 中设置 `"checkpointing": { "enabled": true }`）使其成为默认设置。一旦激活，你会注意到每次Gemini即将写入文件时，它都会说“检查点已保存”。
如果你后来意识到AI所做的编辑有问题，你有两个选择：
* 运行 `/restore list`（或仅 `/restore` 不带参数）以查看带有时间戳和描述的最近检查点列表。
* 运行 `/restore <id>` 以回滚到特定的检查点。如果你省略id并且只有一个待处理的检查点，它将默认[恢复](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Step)该检查点。
例如：
```
/restore
```
Gemini CLI可能会输出：
0: [2025-09-22 10:30:15] 在运行‘apply_patch’之前
1: [2025-09-22 10:45:02] 在运行‘write_file’之前
然后你可以执行 `/restore 0` 将所有文件更改（甚至会话上下文）恢复到该检查点时的状态。通过这种方式，你可以“撤销”错误的代码重构或Gemini[做出](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=1,point%20and%20roll%20back%20instantly)的任何其他更改。
**恢复什么：** 检查点捕获了工作目录的状态（Gemini CLI允许修改的所有文件）和工作区文件（会话状态也可能根据检查点的捕获方式回滚）。当你恢复时，它会用旧版本覆盖文件，并将对话记忆重置到那个快照。这就像将AI智能体时间旅行回它做出错误决定之前。请注意，它不会撤消外部副作用（例如，如果AI运行了数据库迁移，它无法撤消），但文件系统和聊天上下文中的任何内容都是可以恢复的。
**最佳实践：** 对于非平凡的任务，最好保持检查点开启。开销很小，并且提供了安心。如果你发现不需要检查点（一切顺利），你可以随时清除它，或者让下一个检查点覆盖它。开发团队建议在进行多步代码[编辑](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=Tips%20to%20avoid%20messy%20rollbacks)之前特别使用检查点。但是，对于关键任务项目，你仍然应该使用适当的版本控制（`git`）作为你的主要安全[网](https://medium.com/@ferreradaniel/gemini-cli-free-ai-tool-upgrade-5-new-features-you-need-right-now-04cfefac5e93#:~:text=No,VS%20Code%20is%20already%20free)——将检查点视为快速撤销的便利工具，而不是完整的VCS。
本质上，`/restore` 让你可以自信地使用Gemini CLI。你可以让AI尝试大胆的更改，知道在需要时有一个“*哦不按钮*”可以回退。
**快速用例：** 想象你有一个包含一些规格或数据的Google文档或表格，你希望AI使用它。无需复制粘贴内容，你可以提供链接，通过配置的Workspace MCP服务器，Gemini CLI可以获取并读取它。
例如：
```
总结这个设计文档中的需求：https://docs.google.com/document/d/<id>
```
Gemini可以拉取该文档的内容并将其整合到响应中。同样，它可以通过链接读取Google表格或云端硬盘文件。
**工作原理：** 这些功能通常通过**MCP集成**启用。Google的Gemini CLI团队已经构建（或正在构建）Google Workspace的连接器。一种方法是运行一个小型MCP服务器，该服务器使用Google的API（文档API、表格API等）在给定URL或[ID](https://github.com/google-gemini/gemini-cli/issues/7175)时检索文档内容。配置后，你可能会有斜杠命令或工具，如 `/read_google_doc`，或者简单的自动检测，看到Google文档链接后调用适当的工具来获取它。
例如，在Agent Factory播客演示中，团队使用了一个**Google Docs MCP**将摘要直接保存到[文档](https://cloud.google.com/blog/topics/developers-practitioners/agent-factory-recap-deep-dive-into-gemini-cli-with-taylor-mullen#:~:text=%2A%20Utilize%20the%20google,summary%20directly%20to%20Google%20Docs)中——这意味着他们也可以首先读取文档的内容。在实践中，你可以这样做：
```
@https://docs.google.com/document/d/XYZ12345
```
包含带有 `@` 的URL（上下文引用语法）会向Gemini CLI发出信号以获取该资源。有了Google文档集成，该文档的内容将被拉入，就像本地文件一样。然后，AI可以总结它、回答关于它的问题，或者在对话中使用它。
类似地，如果你粘贴一个Google云端硬盘**文件链接**，一个正确配置的云端硬盘工具可以下载或打开该文件（假设权限和API访问已设置）。**Google表格**可以通过运行查询或读取单元格范围的MCP提供，使你能够提出诸如“这个表格[链接]中预算列的总和是多少？”之类的问题，并让AI计算。
**设置：** 在撰写本文时，Google Workspace集成可能需要一些调整（获取API凭据，运行MCP服务器，如[Kanshi Tanaike](https://medium.com/google-cloud/managing-google-docs-sheets-and-slides-by-natural-language-with-gemini-cli-and-mcp-62f4dfbef2d5#:~:text=To%20implement%20this%20approach%2C%20I,methods%20for%20each%20respective%20API)描述的那个等）。请关注官方Gemini CLI仓库和社区论坛，寻找即用型扩展——例如，官方的Google Docs MCP可能作为插件/扩展提供。如果你很急切，你可以按照指南编写一个，了解如何在MCP[服务器](https://github.com/google-gemini/gemini-cli/issues/7175#:~:text=)中使用Google API。这通常涉及处理OAuth（Gemini CLI为MCP服务器支持）并暴露诸如 `read_google_doc` 之类的工具。
**使用提示：** 当你拥有这些工具时，使用它们可以像在提示中提供链接一样简单（AI可能会自动调用工具来获取它），或者使用斜杠命令如 `/doc open <URL>`。检查 `/tools` 以查看可用的命令——Gemini CLI在那里列出了所有工具和自定义[命令](https://dev.to/therealmrmumba/7-insane-gemini-cli-tips-that-will-make-you-a-superhuman-developer-2d7h#:~:text=Gemini%20CLI%20includes%20dozens%20of,can%20supercharge%20your%20dev%20process)。
总之，**Gemini CLI可以超越你的本地文件系统**。无论是Google文档、表格、云端硬盘还是其他外部内容，你都可以通过引用拉取数据。这个专业技巧省去了手动复制粘贴的麻烦，并保持了上下文流的自然性——只需引用你需要的文档或数据集，让AI获取所需内容。它使Gemini CLI成为你所有可访问信息的真正**知识助手**，而不仅仅是磁盘上的文件。
*（注意：访问私有文档当然需要CLI具有适当的权限。始终确保任何集成都尊重安全性和隐私。在企业环境中，设置此类集成可能涉及额外的身份验证步骤。）*
**快速用例：** 无需口头描述文件内容或图像，只需将Gemini CLI直接指向它。使用 `@` 语法，你可以将文件、目录或图像附加到提示中。这保证了AI确切地看到这些文件中的内容作为[上下文](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Reference%20files%20or%20directories%20in,PDFs%2C%20audio%2C%20and%20video%20files)。例如：
```
向我解释这段代码：@./src/main.js
```
这将把 `src/main.js` 的内容包含到提示中（在Gemini的上下文大小限制内），以便AI可以读取并[解释](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Include%20a%20single%20file%3A)它。
这种 `@` *文件引用*是Gemini CLI对开发者最强大的功能之一。它消除了歧义——你不是让模型依赖记忆或对文件的猜测，而是直接将文件交给它读取。你可以将其用于源代码、文本文档、日志等。类似地，你可以引用**整个目录**：
```
重构 @./utils/ 中的代码以使用 async/await。
```
通过附加一个以斜杠结尾的路径，Gemini CLI将递归地包含该[目录](https://www.philschmid.de/gemini-cli-cheatsheet#:~:text=Include%20a%20whole%20directory%20)中的文件（在合理范围内，尊重忽略文件和大小限制）。这对于多文件重构或分析非常有用，因为AI可以一起考虑所有相关的模块。
更令人印象深刻的是，你可以在提示中引用**像图像这样的二进制文件**。Gemini CLI（利用Gemini模型的多模态能力）可以理解图像。例如：
```
描述你在截图中看到的内容：@./design/mockup.png
```
图像将被输入到模型中，AI可能会回应诸如“这是一个带有蓝色登录按钮和标题图像的登录页面”之类的内容。你可以想象其用途：审查UI模型、整理照片（我们将在后面的技巧中看到），或从图像中提取文本（Gemini也可以进行OCR）。
关于有效使用 `@` 引用的几点说明：
* **文件限制：** Gemini 2.5 Pro拥有巨大的上下文窗口（高达100万[令牌](https://blog.google/technology/developers/introducing-gemini-cli-open-source-ai-agent/#:~:text=To%20use%20Gemini%20CLI%20free,per%20day%20at%20no%20charge)），因此你可以包含相当大的文件或许多文件。