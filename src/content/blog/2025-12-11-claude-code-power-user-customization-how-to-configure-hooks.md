---
title: "Claude Code 高级用户自定义：如何配置钩子"
description: "即便是流畅的 [Claude Code](https://www.claude.com/product/claude-code) 工作流，随着时间的推移也会积累摩擦点。每次 Claude 写入文件时，都需要手动运行 [Prettier](https://prettier.io/)。每次运行 npm test 时，都会出现相同的权限提示。每个会话开始时，都需要将相同的样板项目上下文粘贴到第一条消息中"
pubDate: "2025-12-11"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/how-to-configure-hooks"
---
            即便是流畅的 [Claude Code](https://www.claude.com/product/claude-code) 工作流，随着时间的推移也会积累摩擦点。每次 Claude 写入文件时，都需要手动运行 [Prettier](https://prettier.io/)。每次运行 npm test 时，都会出现相同的权限提示。每个会话开始时，都需要将相同的样板项目上下文粘贴到第一条消息中。

好消息是？[钩子](https://code.claude.com/docs/en/hooks-guide) 可以消除这些摩擦点。它们充当触发器，您可以配置它们在特定操作之前或之后触发，从而允许您将自定义逻辑、脚本和命令直接注入到 Claude 的操作中。

本文面向已经熟悉 Claude Code 基础的开发者，介绍高级配置。通过本文，您将了解八种钩子类型、每种钩子的使用时机、如何配置它们，以及当出现问题时如何进行调试。

让我们开始吧。

## **什么是钩子？**

钩子是您创建的自定义 shell 命令，当 Claude Code 会话中发生目标事件时（例如 Claude 即将写入文件或您提交提示时），它会自动执行。您可以将钩子用于各种场景：在操作执行前拦截它们、注入代理上下文、自动批准操作，或在操作发生前阻止它们。

钩子使用 JSON 结构在设置文件中进行配置，包含事件名称、匹配器（用于过滤触发钩子的工具）以及要运行的命令。它们在您的本地环境中以您的用户权限执行，通过 stdin 接收有关触发事件的信息，并通过退出代码和 stdout 进行通信。这使您无需修改工具本身，即可精确控制 Claude Code 的行为。

## **为什么在 Claude Code 中使用钩子？**

钩子解决三类问题。

首先，**它们消除了重复的手动步骤**。无需在每次文件更改后运行格式化程序，PostToolUse 钩子会自动处理。无需第一百次批准 npm test，PermissionRequest 钩子会自动批准。

其次，**钩子自动强制执行项目特定规则**。您可以在危险命令执行前阻止它们，在写入前验证文件路径，或确保遵循命名约定。这些防护措施每次都会运行，而不仅仅是当您记得检查时。

第三，**钩子无需手动操作即可注入动态上下文**。SessionStart 钩子可以向 Claude 提供您当前的 git 状态和 TODO 列表。UserPromptSubmit 钩子可以将您的冲刺优先级附加到每个请求中。Claude 无需您重复说明即可保持信息灵通。

## **Claude Code 钩子类型及其使用时机**

Claude Code 提供八个钩子事件，涵盖会话的完整生命周期，从启动到工具执行再到完成。每个事件在特定时刻触发，让您精确控制自动化运行的时间。选择合适的钩子取决于您想要实现的目标。

**钩子一览**

| 钩子 | 触发时机 | 常见用途 |
| --- | --- | --- |
| PreToolUse | 在工具执行之前 | 阻止危险命令，验证文件路径，自动批准安全操作 |
| PermissionRequest | 在权限对话框出现之前 | 自动批准测试命令，阻止访问敏感文件 |
| PostToolUse | 在工具完成后 | 运行格式化程序，触发代码检查工具，记录文件更改 |
| PreCompact | 在上下文压缩之前 | 备份对话记录，保留重要决策 |
| SessionStart | 当会话开始或恢复时 | 注入 git 状态，加载 TODO 列表，设置环境上下文 |
| Stop | 当 Claude 完成响应时 | 验证任务完成，运行测试，生成摘要 |
| SubagentStop | 当子代理完成时 | 验证子代理输出，触发后续操作 |
| UserPromptSubmit | 当您提交提示时 | 注入冲刺上下文，验证请求，添加动态上下文 |

如果不需要标题，可以删除此行

### **PreToolUse**

这是最常用的钩子，在 Claude 选择要使用的工具但工具实际执行之前触发。您的脚本可以检查计划的操作并批准、阻止、请求用户确认或修改参数，使用匹配器过滤触发此钩子的工具。

此 PreToolUse 钩子示例在文件写入执行前进行评估。Claude 根据指定标准审查计划操作，并可以根据提示逻辑批准、阻止或标记问题。

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/validate-file-path.sh"
          }
        ]
      }
    ]
  }
}
```

何时使用 PreToolUse：

* 阻止危险的 Bash 命令，如 rm -rf 或强制推送
* 自动批准安全、重复的操作以减少提示疲劳
* 在写入前验证文件路径以防止意外覆盖
* 修改工具输入以注入项目特定默认值

### **PermissionRequest**

此钩子在 Claude 通常会显示权限对话框时触发。此钩子拦截在您看到确认提示之前的时刻，让您的脚本决定是允许、拒绝还是仍然询问用户。

```
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": "Bash(npm test*)",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/validate-test-command.sh"
          }
        ]
      }
    ]
  }
}
```

此示例自动批准任何以 npm test 开头的 Bash 命令。匹配器模式可以包含参数以实现更精细的控制。

何时使用 PermissionRequest：

* 自动批准每个会话中运行数十次的测试命令
* 阻止对生产配置文件的写入访问
* 允许对特定目录进行读取操作而无需提示
* 拒绝任何匹配危险模式的命令

### **PostToolUse**

在工具成功完成后立即触发。您的脚本接收有关所发生事件的信息，包括工具输出，使用匹配器过滤触发它的工具。

此 PostToolUse 示例对 Claude 写入或编辑的任何文件运行 Prettier。匹配器中的管道语法表示它同时为 Write 和 Edit 工具触发。

```
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write \"$CLAUDE_TOOL_INPUT_FILE_PATH\""
          }
        ]
      }
    ]
  }
}
```

何时使用 PostToolUse：

* 在每次文件写入后运行 Prettier、Black 或 gofmt 以强制执行格式化
* 将所有文件修改记录到审计跟踪中
* 触发代码检查工具并在代码更改后显示警告
* 在特定操作完成时发送通知

### **PreCompact**

在 Claude 压缩对话上下文以释放空间之前触发。压缩会总结对话的较旧部分，这意味着一些细节会丢失。此钩子让您有机会在压缩发生之前保留信息。

此 PreCompact 示例在自动压缩之前备份对话记录。匹配器可以是 "auto" 或 "manual"，以便您可以区分自动压缩和用户触发的压缩事件。

```
{
  "hooks": {
    "PreCompact": [
      {
        "matcher": "auto",
        "hooks": [
          {
            "type": "command",
            "command": "/path/to/backup-transcript.sh"
          }
        ]
      }
    ]
  }
}
```

何时使用 PreCompact：

* 在总结之前将完整对话记录备份到文件
* 提取并保存重要决策或代码片段
* 记录会话里程碑以供日后审查

### **SessionStart**

在 Claude Code 启动新会话或恢复现有会话时触发。您的脚本输出的任何内容都会添加到对话上下文中，因此 Claude 在启动时就已经加载了这些信息。

```
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "git status --short && echo '---' && cat TODO.md"
          }
        ]
      }
    ]
  }
}
```

每个会话开始时，Claude 都知道您当前的 git 状态和 TODO 列表。stdout 自动成为上下文。

何时使用 SessionStart：

* 向 Claude 提供您当前的 git 分支和最近的提交
* 加载您的 TODO 列表或冲刺积压的内容
* 注入特定于环境的配置详细信息

### **Stop**

在 Claude 完成响应并通常会等待您的下一个输入时触发。您的脚本可以检查 Claude 生成的内容，并决定任务是否真正完成。

脚本可以返回包含 "continue": true 的 JSON，使 Claude 继续工作，这对于多步骤工作流非常有用：

```
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Review whether the task is complete. If all requirements are met, respond with 'complete'. If work remains, respond with 'continue' and specify what still needs to be done."
          }
        ]
      }
    ]
  }
}
```

何时使用 Stop：

* 强制 Claude 继续，直到清单中的所有项目都完成
* 在认为任务完成之前验证测试是否通过
* 在会话结束时触发摘要生成
* 在停止之前检查生成的代码是否编译通过

### **SubagentStop**

此钩子在通过 Task 工具创建的子代理完成时触发。工作方式与 Stop 相同，但专门在子代理完成其操作时触发（而不是主代理）。SubagentStop 的配置与 Stop 钩子结构相同：

```
{
  "hooks": {
    "SubagentStop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Evaluate the subagent's output. Verify the task was completed correctly and the results meet quality standards. If the output is satisfactory, respond with 'accept'. If issues exist, respond with 'reject' and explain what needs to be fixed."
          }
        ]
      }
          ]
  }
}
```

何时使用 SubagentStop：

* 验证子代理输出是否满足质量标准
* 根据子代理结果触发后续操作
* 记录子代理活动以进行调试或审计

### **UserPromptSubmit**

在您提交提示时触发，在 Claude 处理之前。您的脚本通过 stdout 输出的任何内容都会与您的提示一起添加到 Claude 的上下文中，这使得 UserPromptSubmit 对于动态注入 Claude 应考虑的信息非常有用。

在此示例中，每次您提交提示时，Claude 都会收到您的冲刺上下文文件的内容。这使 Claude 了解当前优先级，而无需您重复说明。

```
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "cat ./current-sprint-context.md"
          }
        ]
      }
    ]
  }
}
```

何时使用 UserPromptSubmit：

* 在每个提示中注入当前冲刺上下文或项目优先级
* 在提示到达 Claude 之前进行验证
* 根据内容阻止某些类型的请求
* 添加动态上下文，如最近的错误日志或测试结果

## **配置和文件位置**

钩子位于三个级别的 JSON 设置文件中。项目级钩子放在仓库中的 .claude/settings.json 中，使其可与您的团队共享。用户级钩子放在 ~/.claude/settings.json 中，适用于您的所有项目。本地项目钩子放在 .claude/settings.local.json 中，用于您不想提交的个人配置。

项目级设置优先于用户级设置。还有企业管理的策略设置可用于组织控制。有关完整详细信息，请参阅 Claude Code 设置信息。

**专业提示：** 这是同一个文件，您可以在其中为 Claude 操作设置细粒度权限，在项目、用户或本地级别。例如，您可以明确允许 Claude 读取目录中的所有文件，这样您就不必每次都批准，或者阻止任何对敏感文件的修改。

## **匹配器语法**

匹配器用于过滤哪些工具可以触发您的钩子。它们仅适用于 PreToolUse、PostToolUse 和 PermissionRequest 钩子。

简单的字符串匹配完全按预期工作："Write" 仅匹配 Write 工具。

例如：

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "your-command-here"
          }
        ]
      }
    ]
  }
}
```

管道语法允许您匹配多个工具："Write|Edit" 为两者触发，而通配符匹配所有内容："\*" 或空字符串匹配所有工具。

**注意：** 匹配器区分大小写，因此 "bash" 不会匹配到 Bash 工具。

对于更精细的控制，参数模式如 "Bash(npm test\*)" 可以匹配特定的命令参数。MCP 工具模式遵循格式 "mcp\_\_memory\_\_.\*" 用于模型上下文协议工具。

## **输入、输出和结构化响应**

### **钩子接收什么**

所有钩子通过 stdin 接收包含会话信息和事件特定数据的 JSON。常见字段包括：session_id、transcript_path、cwd、permission_mode 和 hook_event_name。

此外，与工具相关的钩子还会接收 tool_name 和 tool_input。这些数据使您的脚本能够做出明智的响应决策。

### **钩子如何响应**

退出代码确定基本结果。退出代码 0 表示成功，stdout 要么被处理为 JSON，要么被添加到上下文中。退出代码 2 表示阻塞错误：stderr 成为错误消息，操作被阻止。

其他退出代码表示非阻塞错误，stderr 在详细模式下显示。

除了退出代码之外，钩子可以返回结构化 JSON 以实现更多控制。字段包括：decision（批准、阻止、允许或拒绝）、reason（向 Claude 显示的解释）、continue（用于 Stop 钩子以强制继续）和 updatedInput（用于在执行前修改工具参数）。

## **环境和执行**

钩子可以访问环境变量，包括：CLAUDE_PROJECT_DIR 用于项目根路径，CLAUDE_CODE_REMOTE 对于 Web 环境为 true，以及 CLAUDE_ENV_FILE 用于 SessionStart 钩子以持久化变量。来自 shell 的标准环境变量也可访问。

另外需要注意的是：钩子有 60 秒的默认超时时间，每个钩子可配置。当多个钩子匹配一个事件时，它们并行运行。相同的命令会自动去重。

## **安全考虑**

钩子以您的用户权限执行任意 shell 命令。Claude Code 包含一项安全措施：对钩子配置文件的直接编辑需要在 /hooks 菜单中审查后才能生效。这可以防止恶意代码悄悄地将钩子添加到您的配置中。

但是，如果您配置并批准了钩子，它们将以您的权限级别执行。

**专业提示：** 在环境中运行任何命令之前，请考虑风险。如果您要使用钩子运行命令，请考虑以下良好实践：验证和清理来自 stdin 的输入，引用 shell 变量以防止注入，使用脚本的绝对路径，以及避免处理敏感文件，如 .env 或凭据。

## **调试和测试**

Claude Code 将所有内容记录到对话记录文件中，这提供了对工具调用和响应的可见性，无需任何设置。每个钩子都会收到一个 transcript_path 字段，指向包含完整会话历史的 JSONL 文件。您可以使用 SessionStart 钩子来记录每个对话记录的位置：

```
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '\"Session: \" + .transcript_path' >> ~/.claude/sessions.log"
          }
        ]
      }
    ]
  }
}
```

然后使用 tail 跟踪该对话记录，实时查看 Claude 的工作：`tail -f /path/to/transcript.jsonl | jq`。

### **钩子特定调试**

对于钩子特定的调试，向您的钩子脚本添加日志记录。对话记录文件将显示 Claude 做了什么，但不会显示您的钩子为什么批准或阻止了某个操作。

稍加额外努力，您可以添加一个小型 bash 脚本，它将包装您的工具并记录附加信息。例如，log-wrapper.sh：

```
#!/bin/bash
LOG=~/.claude/hooks.log
INPUT=$(cat)

TOOL=$(echo "$INPUT" |
 jq -r '.tool_name // "n/a"')
EVENT=$(echo "$INPUT" | jq -r '.hook_event_name // "n/a"')

echo "=== $(date) | $EVENT | $TOOL ===" >> "$LOG"

echo "$INPUT" | "$1"
CODE=$?

echo "Exit: $CODE" >> "$LOG"
exit $CODE
```

这个小型包装脚本将 stdin 捕获到一个变量中，记录时间戳和工具名称，然后将输入通过管道传递给您的实际工具。

编写好 log-wrapper.sh 后，您需要将其前置到钩子中的工具调用：

```
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "log-wrapper.sh your-tool-command.py"
          }
        ]
      }
    ]
  }
}
```

**专业提示：** 有关更多调试技巧，请查看 Claude Code 调试文档。

## **构建您自己的钩子**

从一个简单的钩子开始，解决工作流中实际存在的摩擦点。PostToolUse 格式化程序钩子是一个很好的首选，因为反馈是即时且可见的。一旦它正常工作，根据您学到的内容进行扩展。

有关完整参考文档，包括所有可用字段和高级模式，请参阅官方钩子文档。

钩子让您塑造 Claude Code 以匹配您的工作流，而不是让您的工作流适应工具。当您投入精力配置钩子时，每个会话都会受益。

*立即开始使用钩子来自定义您的* [*Claude Code*](https://www.claude.com/product/claude-code) *工作流。*
