---
title: "桌面扩展：为 Claude Desktop 实现一键式 MCP 服务器安装"
description: "* 文件扩展名更新   2025 年 9 月 11 日   Claude Desktop Extensions 现使用 .mcpb（MCP Bundle）文件扩展名，取代原来的 .dxt。现有的 .dxt 扩展将继续正常工作，但我们建议开发者在未来为新扩展使用 .mcpb。所有功能保持不变——这纯粹是命名约定的更新。 — 去年我们发布 Model Context Protocol（MCP）时，看到"
pubDate: "2025-06-26"
heroImage: "/post_img.png"
tags: ["anthropic-engineering"]
originalLink: "https://www.anthropic.com/engineering/desktop-extensions"
---

* 文件扩展名更新

  2025 年 9 月 11 日

  Claude Desktop Extensions 现使用 .mcpb（MCP Bundle）文件扩展名，取代原来的 .dxt。现有的 .dxt 扩展将继续正常工作，但我们建议开发者在未来为新扩展使用 .mcpb。所有功能保持不变——这纯粹是命名约定的更新。

—

去年我们发布 Model Context Protocol（MCP）时，看到开发者构建了出色的本地服务器，让 Claude 能够访问从文件系统到数据库的各种资源。但我们不断听到同样的反馈：安装过程过于复杂。用户需要开发者工具、手动编辑配置文件，并且常常因依赖问题而卡住。

今天，我们推出 Desktop Extensions——一种新的打包格式，使得安装 MCP 服务器像点击按钮一样简单。

### 解决 MCP 安装问题

本地 MCP 服务器为 Claude Desktop 用户解锁了强大的能力。它们可以与本地应用程序交互、访问私有数据、与开发工具集成——同时所有数据都保留在用户机器上。然而，当前的安装过程带来了显著障碍：

* **需要开发者工具**：用户需要安装 Node.js、Python 或其他运行时
* **手动配置**：每个服务器都需要编辑 JSON 配置文件
* **依赖管理**：用户必须解决包冲突和版本不匹配问题
* **缺乏发现机制**：查找有用的 MCP 服务器需要搜索 GitHub
* **更新复杂**：保持服务器最新意味着手动重新安装

这些摩擦点意味着，尽管 MCP 服务器功能强大，但对非技术用户来说基本不可及。

### 介绍 Desktop Extensions

Desktop Extensions（`.mcpb` 文件）通过将整个 MCP 服务器（包括所有依赖）打包成一个可安装的包来解决这些问题。以下是用户角度的变化：

**之前：**

```
# Install Node.js first 
npm install -g @example/mcp-server 
# Edit ~/.claude/claude_desktop_config.json manually 
# Restart Claude Desktop 
# Hope it works
```

复制

**之后：**

1. 下载一个 `.mcpb` 文件
2. 双击用 Claude Desktop 打开
3. 点击“安装”

就是这样。没有终端，没有配置文件，没有依赖冲突。

## 架构概览

Desktop Extension 是一个 zip 存档，包含本地 MCP 服务器以及一个 `manifest.json`，它描述了 Claude Desktop 和其他支持桌面扩展的应用所需的一切信息。

```
extension.mcpb (ZIP archive)
├── manifest.json         # Extension metadata and configuration
├── server/               # MCP server implementation
│   └── [server files]    
├── dependencies/         # All required packages/libraries
└── icon.png             # Optional: Extension icon

# Example: Node.js Extension
extension.mcpb
├── manifest.json         # Required: Extension metadata and configuration
├── server/               # Server files
│   └── index.js          # Main entry point
├── node_modules/         # Bundled dependencies
├── package.json          # Optional: NPM package definition
└── icon.png              # Optional: Extension icon

# Example: Python Extension
extension.mcpb (ZIP file)
├── manifest.json         # Required: Extension metadata and configuration
├── server/               # Server files
│   ├── main.py           # Main entry point
│   └── utils.py          # Additional modules
├── lib/                  # Bundled Python packages
├── requirements.txt      # Optional: Python dependencies list
└── icon.png              # Optional: Extension icon
```

复制

Desktop Extension 中唯一必需的文件是 manifest.json。Claude Desktop 处理所有复杂工作：

* **内置运行时**：我们在 Claude Desktop 中内置了 Node.js，消除了外部依赖
* **自动更新**：扩展在新版本可用时自动更新
* **安全密钥**：API 密钥等敏感配置存储在操作系统的密钥链中

清单包含人类可读的信息（如名称、描述或作者）、功能声明（工具、提示）、用户配置和运行时要求。大多数字段是可选的，因此最小版本非常简短，但实际上，我们预计所有三种支持的扩展类型（Node.js、Python 和经典二进制/可执行文件）都会包含文件：

```
{
  "mcpb_version": "0.1",                    // MCPB spec version this manifest conforms to
  "name": "my-extension",                   // Machine-readable name (used for CLI, APIs)
  "version": "1.0.0",                       // Semantic version of your extension
  "description": "A simple MCP extension",  // Brief description of what the extension does
  "author": {                               // Author information (required)
    "name": "Extension Author"              // Author's name (required field)
  },
  "server": {                               // Server configuration (required)
    "type": "node",                         // Server type: "node", "python", or "binary"
    "entry_point": "server/index.js",       // Path to the main server file
    "mcp_config": {                         // MCP server configuration
      "command": "node",                    // Command to run the server
      "args": [                             // Arguments passed to the command
        "${__dirname}/server/index.js"      // ${__dirname} is replaced with the extension's directory
      ]                              
    }
  }
}
```

复制

清单规范中提供了许多便利选项，旨在使本地 MCP 服务器的安装和配置更加容易。服务器配置对象可以定义得既包含模板字面量形式的用户自定义配置，也包含平台特定的覆盖。扩展开发者可以详细定义他们希望从用户那里收集哪种配置。

让我们看一个具体的例子，了解清单如何辅助配置。在下面的清单中，开发者声明用户需要提供一个 `api_key`。Claude 在用户提供该值之前不会启用扩展，会将其自动保存在操作系统的安全保管库中，并在启动服务器时透明地将 `${user_config.api_key}` 替换为用户提供的值。类似地，`${__dirname}` 将被替换为扩展解压目录的完整路径。

```
{
  "mcpb_version": "0.1",
  "name": "my-extension",
  "version": "1.0.0",
  "description": "A simple MCP extension",
  "author": {
    "name": "Extension Author"
  },
  "server": {
    "type": "node",
    "entry_point": "server/index.js",
    "mcp_config": {
      "command": "node",
      "args": ["${__dirname}/server/index.js"],
      "env": {
        "API_KEY": "${user_config.api_key}"
      }
    }
  },
  "user_config": {
    "api_key": {
      "type": "string",
      "title": "API Key",
      "description": "Your API key for authentication",
      "sensitive": true,
      "required": true
    }
  }
}
```

复制

包含大部分可选字段的完整 `manifest.json` 可能如下所示：

```
{
  "mcpb_version": "0.1",
  "name": "My MCP Extension",
  "display_name": "My Awesome MCP Extension",
  "version": "1.0.0",
  "description": "A brief description of what this extension does",
  "long_description": "A detailed description that can include multiple paragraphs explaining the extension's functionality, use cases, and features. It supports basic markdown.",
  "author": {
    "name": "Your Name",
    "email": "yourname@example.com",
    "url": "https://your-website.com"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/your-username/my-mcp-extension"
  },
  "homepage": "https://example.com/my-extension",
  "documentation": "https://docs.example.com/my-extension",
  "support": "https://github.com/your-username/my-extension/issues",
  "icon": "icon.png",
  "screenshots": [
    "assets/screenshots/screenshot1.png",
    "assets/screenshots/screenshot2.png"
  ],
  "server": {
    "type": "node",
    "entry_point": "server/index.js",
    "mcp_config": {
      "command": "node",
      "args": ["${__dirname}/server/index.js"],
      "env": {
        "ALLOWED_DIRECTORIES": "${user_config.allowed_directories}"
      }
    }
  },
  "tools": [
    {
      "name": "search_files",
      "description": "Search for files in a directory"
    }
  ],
  "prompts": [
    {
      "name": "poetry",
      "description": "Have the LLM write poetry",
      "arguments": ["topic"],
      "text": "Write a creative poem about the following topic: ${arguments.topic}"
    }
  ],
  "tools_generated": true,
  "keywords": ["api", "automation", "productivity"],
  "license": "MIT",
  "compatibility": {
    "claude_desktop": ">=1.0.0",
    "platforms": ["darwin", "win32", "linux"],
    "runtimes": {
      "node": ">=16.0.0"
    }
  },
  "user_config": {
    "allowed_directories": {
      "type": "directory",
      "title": "Allowed Directories",
      "description": "Directories the server can access",
      "multiple": true,
      "required": true,
      "default": ["${HOME}/Desktop"]
    },
    "api_key": {
      "type": "string",
      "title": "API Key",
      "description": "Your API key for authentication",
      "sensitive": true,
      "required": false
    },
    "max_file_size": {
      "type": "number",
      "title": "Maximum File Size (MB)",
      "description": "Maximum file size to process",
      "default": 10,
      "min": 1,
      "max": 100
    }
  }
}
```

复制

要查看扩展和清单，请参考 [MCPB 仓库中的示例](https://github.com/anthropics/dxt/tree/main/examples)。

`manifest.json` 中所有必需和可选字段的完整规范可以在我们的[开源工具链](https://github.com/anthropics/dxt/blob/main/MANIFEST.md)中找到。

### 构建你的第一个扩展

让我们逐步将现有的 MCP 服务器打包成 Desktop Extension。我们以简单的文件系统服务器为例。

#### 第一步：创建清单

首先，为你的服务器初始化清单：

```
npx @anthropic-ai/mcpb init
```

复制

这个交互式工具会询问关于你的服务器的信息，并生成一个完整的 manifest.json。如果你想快速生成最基本的 manifest.json，可以使用 `--yes` 参数运行该命令。

#### 第二步：处理用户配置

如果你的服务器需要用户输入（如 API 密钥或允许的目录），请在清单中声明：

```
"user_config": {
  "allowed_directories": {
    "type": "directory",
    "title": "Allowed Directories",
    "description": "Directories the server can access",
    "multiple": true,
    "required": true,
    "default": ["${HOME}/Documents"]
  }
}
```

复制

Claude Desktop 将：

* 显示用户友好的配置界面
* 在启用扩展前验证输入
* 安全地存储敏感值
* 将配置作为参数或环境变量传递给服务器（取决于开发者配置）

在下面的例子中，我们将用户配置作为环境变量传递，但也可以作为参数。

```
"server": {
   "type": "node",
   "entry_point": "server/index.js",
   "mcp_config": {
   "command": "node",
   "args": ["${__dirname}/server/index.js"],
   "env": {
      "ALLOWED_DIRECTORIES": "${user_config.allowed_directories}"
   }
   }
}
```

复制

#### 第三步：打包扩展

将所有内容打包成 `.mcpb` 文件：

```
npx @anthropic-ai/mcpb pack
```

复制

此命令：

1. 验证你的清单
2. 生成 `.mcpb` 存档

#### 第四步：本地测试

将你的 `.mcpb` 文件拖入 Claude Desktop 的设置窗口。你将看到：

* 关于扩展的人类可读信息
* 所需的权限和配置
* 一个简单的“安装”按钮

### 高级功能

#### 跨平台支持

扩展可以适应不同的操作系统：

```
"server": {
  "type": "node",
  "entry_point": "server/index.js",
  "mcp_config": {
    "command": "node",
    "args": ["${__dirname}/server/index.js"],
    "platforms": {
      "win32": {
        "command": "node.exe",
        "env": {
          "TEMP_DIR": "${TEMP}"
        }
      },
      "darwin": {
        "env": {
          "TEMP_DIR": "${TMPDIR}"
        }
      }
    }
  }
}
```

复制

#### 动态配置

使用模板字面量获取运行时值：

* `${__dirname}`：扩展的安装目录
* `${user_config.key}`：用户提供的配置
* `${HOME}, ${TEMP}`：系统环境变量

#### 功能声明

帮助用户提前了解能力：

```
"tools": [
  {
    "name": "read_file",
    "description": "Read contents of a file"
  }
],
"prompts": [
  {
    "name": "code_review",
    "description": "Review code for best practices",
    "arguments": ["file_path"]
  }
]
```

复制

### 扩展目录

我们随 Claude Desktop 内置了一个经过筛选的扩展目录。用户可以浏览、搜索并通过一次点击安装——无需搜索 GitHub 或审查代码。

虽然我们预计 Desktop Extension 规范以及 macOS 和 Windows 版 Claude 中的实现会随着时间的推移而演变，但我们期待看到扩展以各种创造性的方式扩展 Claude 的能力。

要提交你的扩展：

1. 确保它符合提交表格中的指南
2. 在 Windows 和 macOS 上进行测试
3. [提交你的扩展](https://docs.google.com/forms/d/14_Dmcig4z8NeRMB_e7TOyrKzuZ88-BLYdLvS6LPhiZU/edit)
4. 我们的团队会审查质量和安全性

### 构建开放的生态系统

我们致力于围绕 MCP 服务器构建开放的生态系统，并相信其被多个应用程序和服务普遍采用的能力已使社区受益。为了履行这一承诺，我们将开源 Desktop Extension 规范、工具链以及用于 macOS 和 Windows 版 Claude 实现其自身 Desktop Extension 支持的架构和关键函数。我们希望 MCPB 格式不仅能让本地 MCP 服务器在 Claude 上更易于移植，也能在其他 AI 桌面应用程序中实现。

我们将开源：

* 完整的 MCPB 规范
* 打包和验证工具
* 参考实现代码
* TypeScript 类型和架构

这意味着：

* **对于 MCP 服务器开发者**：打包一次，在任何支持 MCPB 的地方运行
* **对于应用程序开发者**：无需从头构建即可添加扩展支持
* **对于用户**：在所有支持 MCP 的应用程序中获得一致的体验

规范和工具链特意被版本化为 0.1，因为我们期待与更大的社区合作，共同发展和改进该格式。我们期待听到你的反馈。

### 安全与企业考量

我们理解扩展引入了新的安全考量，尤其是对于企业用户。我们在 Desktop Extensions 的预览版中内置了多项安全措施：

#### 对于用户

* 敏感数据保留在操作系统密钥链中
* 自动更新
* 能够审计已安装的扩展

#### 对于企业

* 支持组策略（Windows）和 MDM（macOS）
* 能够预安装批准的扩展
* 将特定扩展或发布者列入黑名单
* 完全禁用扩展目录
* 部署私有扩展目录

有关如何在组织内管理扩展的更多信息，请参阅我们的[文档](https://support.anthropic.com/en/articles/10949351-getting-started-with-model-context-protocol-mcp-on-claude-for-desktop)。

### 入门指南

准备好构建自己的扩展了吗？以下是开始的方法：

**对于 MCP 服务器开发者**：查阅我们的[开发者文档](https://github.com/anthropics/dxt) – 或者直接在本地 MCP 服务器目录中运行以下命令：

```
npm install -g @anthropic-ai/mcpb
mcpb init
mcpb pack
```

复制

**对于 Claude Desktop 用户**：更新到最新版本，然后在设置中查找扩展部分

**对于企业**：查看我们的企业文档了解部署选项

### 使用 Claude Code 进行构建

在 Anthropic 内部，我们发现 Claude 能够以极少的干预出色地构建扩展。如果你也想使用 Claude Code，我们建议你简要说明你希望扩展做什么，然后在提示中添加以下上下文：

```
I want to build this as a Desktop Extension, abbreviated as "MCPB". Please follow these steps:

1. **Read the specifications thoroughly:**
   - https://github.com/anthropics/mcpb/blob/main/README.md - MCPB architecture overview, capabilities, and integration patterns
   - https://github.com/anthropics/mcpb/blob/main/MANIFEST.md - Complete extension manifest structure and field definitions
   - https://github.com/anthropics/mcpb/tree/main/examples - Reference implementations including a "Hello World" example

2. **Create a proper extension structure:**
   - Generate a valid manifest.json following the MANIFEST.md spec
   - Implement an MCP server using @modelcontextprotocol/sdk with proper tool definitions
   - Include proper error handling and timeout management

3. **Follow best development practices:**
   - Implement proper MCP protocol communication via stdio transport
   - Structure tools with clear schemas, validation, and consistent JSON responses
   - Make use of the fact that this extension will be running locally
   - Add appropriate logging and debugging capabilities
   - Include proper documentation and setup instructions

4. **Test considerations:**
   - Validate that all tool calls return properly structured responses
   - Verify manifest loads correctly and host integration works

Generate complete, production-ready code that can be immediately tested. Focus on defensive programming, clear error messages, and following the exact
MCPB specifications to ensure compatibility with the ecosystem.
```

复制

### 结论

Desktop Extensions 代表了用户与本地 AI 工具交互方式的根本性转变。通过消除安装摩擦，我们让强大的 MCP 服务器对所有人——不仅仅是开发者——变得触手可及。

在内部，我们使用 desktop extensions 来共享高度实验性的 MCP 服务器——有些有趣，有些实用。一个团队尝试看看当我们的模型直接连接到 GameBoy 时能走多远，类似于我们的[“Claude 玩 Pokémon”研究](https://www.anthropic.com/news/visible-extended-thinking)。我们使用 Desktop Extensions 打包了一个单一的扩展，它打开了流行的 [PyBoy](https://github.com/Baekalfen/PyBoy) GameBoy 模拟器，并让 Claude 控制它。我们相信，将模型的能力与用户本地机器上已有的工具、数据和应用程序连接起来的机会是无限的。

我们迫不及待地想看到你的创作。带来数千个 MCP 服务器的创造力现在只需一次点击就能触达数百万用户。准备好分享你的 MCP 服务器了吗？[提交你的扩展进行审查](https://forms.gle/tyiAZvch1kDADKoP9)。

[### 想要了解更多？

探索课程](https://anthropic.skilljar.com/)
