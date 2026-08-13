---
title: "Claude Code 高级用户自定义：如何配置钩子 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-12-11"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/how-to-configure-hooks"
---

# Claude Code 高级用户自定义：如何配置钩子 | Anthropic 的 Claude

---

即使流畅的 [Claude Code](https://www.claude.com/product/claude-code) 工作流也会随着时间积累摩擦点。每次 Claude 写入文件，都需要手动运行 [Prettier](https://prettier.io/)。每次运行 npm test，都会出现相同的权限提示。每次会话开始时，都需要将相同的样板项目上下文粘贴到第一条消息中。

好消息是？[钩子](https://code.claude.com/docs/en/hooks-guide) 消除了这些摩擦点。它们充当触发器，你可以配置它们在特定操作之前或之后触发，从而允许你在 Claude 的操作中注入自定义逻辑、脚本和命令。

本文面向已经熟悉 Claude Code 基础的开发者，介绍高级配置。阅读本文后，你将了解八种钩子类型、每种钩子的使用时机、如何配置它们，以及当出现问题时如何调试。

让我们开始吧。

## **什么是钩子？**

钩子是你创建的自定义 shell 命令，用于在 Claude Code 会话中发生目标事件时自动执行，例如当 Claude 即将写入文件或当你提交提示时。你可以将钩子用于各种用途：在操作执行前拦截它们、注入代理上下文、自动化审批，或者在操作发生前阻止它们。

钩子在你的设置文件中使用 JSON 结构进行配置，包含事件名称、匹配器（用于过滤哪些工具触发钩子）以及要运行的命令。它们在你本地的环境中以你的用户权限执行，通过 stdin 接收触发事件的信息，并通过退出代码和 stdout 进行通信。这让你能够精确控制 Claude Code 的行为，而无需修改工具本身。

## **为什么在 Claude Code 中使用钩子？**

钩子解决三类问题。

首先，**它们消除了重复的手动步骤**。无需在每次文件更改后运行格式化器，PostToolUse 钩子会自动处理。无需第 100 次批准 npm test，PermissionRequest 钩子会自动批准。

其次，**钩子自动强制执行项目特定规则**。你可以在危险命令执行前阻止它们，在写入前验证文件路径，或确保遵循命名约定。这些护栏每次都会运行，而不仅仅是你记得检查的时候。

第三，**钩子无需手动操作即可注入动态上下文**。SessionStart 钩子可以让 Claude 知道你当前的 git 状态和待办事项列表。UserPromptSubmit 钩子可以将你的冲刺优先级附加到每个请求中。Claude 始终了解情况，而你无需重复。

## **Claude Code 钩子类型及其使用时机**

Claude Code 提供了八种钩子事件，覆盖会话的完整生命周期，从启动到工具执行再到完成。每个事件在特定时刻触发，让你精确控制自动化运行的时间。选择合适的钩子取决于你想要实现的目标。

**钩子一览**

| 钩子 | 何时触发 | 常见用途 |
| --- | --- | --- |
| PreToolUse | 在工具执行之前 | 阻止危险命令、验证文件路径、自动批准安全操作 |
| PermissionRequest | 在权限对话框出现之前 | 自动批准测试命令、阻止访问敏感文件 |
| PostToolUse | 在工具完成之后 | 运行格式化器、触发 linter、记录文件更改 |
| PreCompact | 在上下文压缩之前 | 备份对话记录、保留重要决策 |
| SessionStart | 当会话开始或恢复时 | 注入 git 状态、加载待办事项列表、设置环境上下文 |
| Stop | 当 Claude 完成响应时 | 验证任务完成、运行测试、生成摘要 |
| SubagentStop | 当子代理完成时 | 验证子代理输出、触发后续操作 |
| UserPromptSubmit | 当你提交提示时 | 注入冲刺上下文、验证请求、添加动态上下文 |

如果不需要说明文字，可以删除此行

### **PreToolUse**

这是最常用的钩子，在 Claude 选择要使用的工具之后但在工具实际执行之前触发。你的脚本可以检查计划的操作并批准它、阻止它、请求用户确认或修改参数，使用匹配器来过滤哪些工具触发此钩子。

这个 PreToolUse 钩子示例在文件写入执行之前评估这些写入操作。Claude 会根据指定的标准审查计划的操作，并可以根据提示逻辑批准、阻止或标记问题。

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
* 修改工具输入以注入项目特定的默认值

### **PermissionRequest**

此钩子在 Claude 通常会显示权限对话框时触发。此钩子拦截在你看到确认提示之前的那一刻，让你的脚本决定是允许、拒绝还是仍然询问用户。

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

* 自动批准你在一个会话中运行数十次的测试命令
* 阻止对生产配置文件的写入访问
* 允许对特定目录进行读取操作而无需提示
* 拒绝任何匹配危险模式的命令

### **PostToolUse**

在工具成功完成后立即触发。你的脚本会收到所发生事件的信息，包括工具输出，使用匹配器来过滤哪些工具触发它。

此 PostToolUse 示例对 Claude 写入或编辑的任何文件运行 Prettier。匹配器中的管道语法表示它对 Write 和 Edit 工具都触发。

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
* 触发 linter 并在代码更改后显示警告
* 在特定操作完成时发送通知

### **PreCompact**

在 Claude 压缩对话上下文以释放空间之前触发。压缩会总结对话中较旧的部分，这意味着某些细节会丢失。此钩子让你有机会在压缩发生之前保留信息。

此 PreCompact 示例在自动压缩之前备份对话记录。匹配器可以是 "auto" 或 "manual"，以便你可以区分自动压缩和用户触发的压缩事件。

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

* 在总结之前将完整对话记录备份到文件中
* 提取并保存重要决策或代码片段
* 记录会话里程碑以供后续审查

### **SessionStart**

在 Claude Code 启动新会话或恢复现有会话时触发。你的脚本输出的任何内容都会添加到对话上下文中，因此 Claude 启动时已加载了这些信息。

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

每个会话开始时，Claude 都知道你当前的 git 状态和待办事项列表。stdout 自动成为上下文。

何时使用 SessionStart：

* 将你当前的 git 分支和最近的提交信息提供给 Claude
* 加载你的待办事项列表或冲刺待办事项的内容
* 注入特定于环境的配置细节

### **Stop**

在 Claude 完成响应并且通常会等待你的下一个输入时触发。你的脚本可以检查 Claude 产生的内容并决定任务是否真正完成。

脚本可以返回包含 "continue": true 的 JSON，以让 Claude 继续工作，这对于多步骤工作流很有用：

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

* 强制 Claude 继续，直到检查列表中的所有项目都完成
* 在认为任务完成之前验证测试是否通过
* 在会话结束时触发摘要生成
* 在停止之前检查生成的代码是否能编译

### **SubagentStop**

此钩子在通过 Task 工具创建的子代理完成时触发。与 Stop 的工作方式相同，但专门在子代理完成其操作时触发（而不是主代理）。SubagentStop 的配置与 Stop 钩子结构相同：

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

* 验证子代理输出是否符合质量标准
* 根据子代理结果触发后续操作
* 记录子代理活动以用于调试或审计

### **UserPromptSubmit**

在你提交提示时触发，在 Claude 处理之前。无论你的脚本通过 stdout 输出什么内容，都会与你的提示一起添加到 Claude 的上下文中，这使得 UserPromptSubmit 对于动态注入 Claude 应考虑的信息非常有用。

在此示例中，每次你提交提示时，Claude 都会收到你的冲刺上下文文件的内容。这使 Claude 始终了解当前优先级，而无需你重复陈述。

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

钩子存在于三个级别的 JSON 设置文件中。项目级钩子放在仓库中的 .claude/settings.json 中，可以与团队共享。用户级钩子放在 ~/.claude/settings.json 中，适用于你的所有项目。本地项目钩子放在 .claude/settings.local.json 中，用于你不想提交的个人配置。

项目级设置优先于用户级设置。还有企业管理的策略设置可用于组织控制。有关完整详细信息，请参阅 Claude Code 设置信息。

**专业提示：** 这也是同一个文件，你可以在项目、用户或本地级别设置 Claude 操作的精细权限。例如，你可以明确允许 Claude 读取目录中的所有文件，这样你就不必每次都批准，或者阻止任何对敏感文件的修改。

## **匹配器语法**

匹配器用于过滤哪些工具可以触发你的钩子。它们仅适用于 PreToolUse、PostToolUse 和 PermissionRequest 钩子。

简单的字符串匹配完全符合你的预期："Write" 只匹配 Write 工具。

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

管道语法允许你匹配多个工具："Write|Edit" 触发两者中的任何一个；通配符匹配所有内容："\*" 或空字符串匹配所有工具。

**注意：** 匹配器区分大小写，因此 "bash" 不会匹配 Bash 工具。

为了更精细的控制，参数模式如 "Bash(npm test\*)" 可以匹配特定的命令参数。MCP 工具模式遵循格式 "mcp\_\_memory\_\_.\*" 用于模型上下文协议工具。

## **输入、输出和结构化响应**

### **钩子接收什么**

所有钩子都通过 stdin 接收 JSON，包含会话信息和特定于事件的数据。常见字段包括：session\_id、transcript\_path、cwd、permission\_mode 和 hook\_event\_name。

此外，与工具相关的钩子还接收 tool\_name 和 tool\_input。这些数据让你的脚本能够做出明智的响应决策。

### **钩子如何响应**

退出代码确定基本结果。退出代码 0 表示成功，stdout 要么被处理为 JSON，要么被添加到上下文中。退出代码 2 表示阻塞错误：stderr 成为错误消息，操作被阻止。

其他退出代码表示非阻塞错误，在详细模式下会显示 stderr。

除了退出代码，钩子还可以返回结构化 JSON 以实现更多控制。字段包括：decision（approve、block、allow 或 deny）、reason（向 Claude 显示的解释）、continue（用于 Stop 钩子以强制继续）以及 updatedInput（用于在执行前修改工具参数）。

## **环境和执行**

钩子可以访问环境变量，包括：CLAUDE\_PROJECT\_DIR（项目根路径）、CLAUDE\_CODE\_REMOTE（对于 Web 环境为 true）以及 CLAUDE\_ENV\_FILE（用于 SessionStart 钩子以持久化变量）。你的 shell 中的标准环境变量也可访问。

另外需要注意的是：钩子有 60 秒的默认超时时间，可为每个钩子单独配置。当多个钩子匹配一个事件时，它们并行运行。相同的命令会自动去重。

## **安全注意事项**

钩子以你的用户权限执行任意 shell 命令。Claude Code 包含一项安全措施：对钩子配置文件的直接编辑需要在 /hooks 菜单中审查后才能生效。这可以防止恶意代码静默地将钩子添加到你的配置中。

但是，如果你配置并批准了钩子，它们将以你的权限级别执行。

**专业提示：** 在执行任何命令之前，请考虑风险。如果你打算使用钩子运行命令，请考虑以下良好实践：验证和清理来自 stdin 的输入、对 shell 变量加引号以防止注入、对脚本使用绝对路径、避免处理敏感文件（如 .env 或凭据）。

## **调试和测试**

Claude Code 将所有内容记录到对话记录文件中，这提供了对工具调用和响应的可见性，无需任何设置。每个钩子都会收到一个 transcript\_path 字段，指向包含完整会话历史的 JSONL 文件。你可以使用 SessionStart 钩子记录每个对话记录的位置：

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

然后使用 tail 实时查看该对话记录以观察 Claude 的工作：`tail -f /path/to/transcript.jsonl | jq`。

### **特定钩子的调试**

对于特定钩子的调试，请在钩子脚本中添加日志记录。对话记录文件会显示 Claude 做了什么，但不会显示你的钩子为什么批准或阻止了某个操作。

多花一点精力，你可以添加一个小的 bash 脚本来包装你的工具并记录额外信息。例如，log-wrapper.sh：

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

这个小包装脚本将 stdin 捕获到一个变量中，记录时间戳和工具名称，然后将输入传递给实际工具。

在编写好 log-wrapper.sh 之后，你需要在钩子中的工具调用之前加上它：

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

## **构建你自己的钩子**

从一个简单的钩子开始，解决你工作流中一个实际的摩擦点。PostToolUse 格式化器钩子是一个很好的首选，因为反馈是即时且可见的。一旦它工作正常，根据你学到的内容进行扩展。

有关完整参考文档，包括所有可用字段和高级模式，请参阅官方钩子文档。

钩子让你能够塑造 Claude Code 以匹配你的工作流，而不是让你的工作流适应工具。当你投资配置钩子时，每个会话都会受益。

*立即开始使用钩子来自定义你的* [*Claude Code*](https://www.claude.com/product/claude-code) *工作流。*
