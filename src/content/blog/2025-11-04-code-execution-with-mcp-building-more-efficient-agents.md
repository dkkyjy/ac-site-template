---
title: "使用 MCP 执行代码：构建更高效的智能体"
description: "[模型上下文协议（MCP）](https://modelcontextprotocol.io/) 是一个开放标准，用于将 AI 智能体连接到外部系统。传统上，将智能体连接到工具和数据需要为每一对组合进行自定义集成，造成碎片化和重复工作，使得真正连通的系统难以扩展。MCP 提供了一种通用协议——开发者在自己的智能体中实现一次 MCP，就能解锁整个集成生态系统。 自 2024 年 11 月推出 MCP"
pubDate: "2025-11-04"
heroImage: "/post_img.png"
tags: ["anthropic-engineering"]
originalLink: "https://www.anthropic.com/engineering/code-execution-with-mcp"
---

[模型上下文协议（MCP）](https://modelcontextprotocol.io/) 是一个开放标准，用于将 AI 智能体连接到外部系统。传统上，将智能体连接到工具和数据需要为每一对组合进行自定义集成，造成碎片化和重复工作，使得真正连通的系统难以扩展。MCP 提供了一种通用协议——开发者在自己的智能体中实现一次 MCP，就能解锁整个集成生态系统。

自 2024 年 11 月推出 MCP 以来，其采用速度迅猛：社区已构建了数千个 [MCP 服务器](https://github.com/modelcontextprotocol/servers)，[SDK](https://modelcontextprotocol.io/docs/sdk) 覆盖所有主流编程语言，并且行业已将 MCP 作为连接智能体与工具和数据的事实标准。

如今，开发者日常构建的智能体可以访问来自数十个 MCP 服务器的数百甚至数千个工具。然而，随着连接的工具数量增长，一次性加载所有工具定义并将中间结果通过上下文窗口传递，会拖慢智能体速度并增加成本。

在这篇博客中，我们将探讨代码执行如何使智能体更高效地与 MCP 服务器交互，在占用更少 token 的情况下处理更多工具。

## **工具消耗过多 token 导致智能体效率下降**

随着 MCP 使用规模的扩大，有两种常见模式会增加智能体的成本和延迟：

1. 工具定义充斥上下文窗口；
2. 中间工具结果消耗额外 token。

### **1. 工具定义充斥上下文窗口**

大多数 MCP 客户端会将所有工具定义一次性直接加载到上下文中，并使用直接工具调用语法向模型暴露这些工具。这些工具定义可能如下所示：

```
gdrive.getDocument
     描述：从 Google Drive 检索文档
     参数：
               documentId（必需，字符串）：要检索的文档 ID
               fields（可选，字符串）：要返回的特定字段
     返回：包含标题、正文内容、元数据、权限等的文档对象
```

复制

```
salesforce.updateRecord
    描述：更新 Salesforce 中的记录
    参数：
               objectType（必需，字符串）：Salesforce 对象类型（Lead、Contact、Account 等）
               recordId（必需，字符串）：要更新的记录 ID
               data（必需，对象）：要更新的字段及其新值
     返回：带有确认信息的已更新记录对象
```

复制

工具描述占用更多上下文窗口空间，增加了响应时间和成本。当智能体连接了数千个工具时，它们需要在阅读请求之前处理数十万个 token。

### **2. 中间工具结果消耗额外 token**

大多数 MCP 客户端允许模型直接调用 MCP 工具。例如，你可能要求你的智能体：“从 Google Drive 下载我的会议记录，并将其附加到 Salesforce 线索中。”

模型将发出如下调用：

```
工具调用：gdrive.getDocument(documentId: "abc123")
        → 返回 "讨论了 Q4 目标……\n[完整转录文本]"
          （加载到模型上下文中）

工具调用：salesforce.updateRecord(
			objectType: "SalesMeeting",
			recordId: "00Q5f000001abcXYZ",
  			data: { "Notes": "讨论了 Q4 目标……\n[完整转录文本写出]" }
		)
		（模型需要再次将整个转录写入上下文）
```

复制

每个中间结果都必须经过模型。在这个例子中，完整的通话记录流经了两次。对于一个两小时的销售会议，这可能意味着额外处理 50,000 个 token。更大的文档甚至可能超出上下文窗口限制，导致工作流程中断。

对于大型文档或复杂数据结构，模型在工具调用之间复制数据时更容易出错。

MCP 客户端将工具定义加载到模型的上下文窗口中，并编排一个消息循环，每个工具调用及其结果在操作之间都会经过模型。

## **使用 MCP 执行代码可提高上下文效率**

随着代码执行环境在智能体中越来越常见，一种解决方案是将 MCP 服务器呈现为代码 API 而非直接工具调用。然后智能体可以编写代码与 MCP 服务器交互。这种方法解决了两个挑战：智能体可以只加载所需的工具，并在执行环境中处理数据，然后将结果返回给模型。

有多种方式可以实现这一点。一种方法是从连接的 MCP 服务器生成所有可用工具的文件树。以下是使用 TypeScript 的实现：

```
servers
├── google-drive
│   ├── getDocument.ts
│   ├── ...（其他工具）
│   └── index.ts
├── salesforce
│   ├── updateRecord.ts
│   ├── ...（其他工具）
│   └── index.ts
└── ...（其他服务器）
```

复制

然后每个工具对应一个文件，例如：

```
// ./servers/google-drive/getDocument.ts
import { callMCPTool } from "../../../client.js";

interface GetDocumentInput {
  documentId: string;
}

interface GetDocumentResponse {
  content: string;
}

/* 从 Google Drive 读取文档 */
export async function getDocument(input: GetDocumentInput): Promise<GetDocumentResponse> {
  return callMCPTool<GetDocumentResponse>('google_drive__get_document', input);
}
```

复制

上面 Google Drive 到 Salesforce 的例子就变成了代码：

```
// 从 Google Docs 读取转录并添加到 Salesforce 潜在客户
import * as gdrive from './servers/google-drive';
import * as salesforce from './servers/salesforce';

const transcript = (await gdrive.getDocument({ documentId: 'abc123' })).content;
await salesforce.updateRecord({
  objectType: 'SalesMeeting',
  recordId: '00Q5f000001abcXYZ',
  data: { Notes: transcript }
});
```

复制

智能体通过探索文件系统来发现工具：列出 `./servers/` 目录以查找可用的服务器（如 `google-drive` 和 `salesforce`），然后读取它需要的特定工具文件（如 `getDocument.ts` 和 `updateRecord.ts`）以了解每个工具的接口。这使智能体能够只加载当前任务所需的定义。这使 token 使用量从 150,000 个 token 减少到 2,000 个 token——时间和成本节省了 98.7%**。**

Cloudflare [发表了类似的研究结果](https://blog.cloudflare.com/code-mode/)，将使用 MCP 执行代码称为“代码模式”。核心见解是相同的：LLM 擅长编写代码，开发者应利用这一优势构建更高效地与 MCP 服务器交互的智能体。

## **使用 MCP 执行代码的好处**

使用 MCP 执行代码使智能体能够通过按需加载工具、在数据到达模型之前进行过滤以及单步执行复杂逻辑来更高效地使用上下文。这种方法还带来了安全性和状态管理方面的好处。

### 渐进式披露

模型擅长导航文件系统。将工具作为文件系统中的代码呈现，允许模型按需读取工具定义，而不是一次性全部读取。

或者，可以在服务器中添加一个 `search_tools` 工具来查找相关定义。例如，在使用上述假设的 Salesforce 服务器时，智能体会搜索“salesforce”并仅加载当前任务所需的工具。在 `search_tools` 工具中包含一个细节级别参数，允许智能体选择所需的详细程度（例如仅名称、名称和描述或完整定义及模式）也有助于智能体节省上下文并高效查找工具。

### 上下文高效的工具结果

处理大型数据集时，智能体可以在代码中过滤和转换结果，然后再返回。考虑获取一个包含 10,000 行的电子表格：

```
// 无代码执行 - 所有行都流经上下文
工具调用：gdrive.getSheet(sheetId: 'abc123')
        → 在上下文中返回 10,000 行以手动过滤

// 有代码执行 - 在执行环境中过滤
const allRows = await gdrive.getSheet({ sheetId: 'abc123' });
const pendingOrders = allRows.filter(row => 
  row["Status"] === 'pending'
);
console.log(`找到 ${pendingOrders.length} 个待处理订单`);
console.log(pendingOrders.slice(0, 5)); // 仅记录前 5 个以供审查
```

复制

智能体看到的是 5 行而不是 10,000 行。类似的模式适用于聚合、跨多个数据源的连接或提取特定字段——所有这些都不会使上下文窗口膨胀。

#### **更强大且上下文高效的控制流**

循环、条件判断和错误处理可以使用熟悉的代码模式，而不是串联单个工具调用。例如，如果你需要在 Slack 中发送部署通知，智能体可以编写：

```
let found = false;
while (!found) {
  const messages = await slack.getChannelHistory({ channel: 'C123456' });
  found = messages.some(m => m.text.includes('deployment complete'));
  if (!found) await new Promise(r => setTimeout(r, 5000));
}
console.log('Deployment notification received');
```

复制

这种方法比通过智能体循环交替进行 MCP 工具调用和 sleep 命令更高效。

此外，能够写出一个可执行的条件树也节省了“首 token 时间”延迟：智能体可以让代码执行环境处理 if 语句，而不必等待模型进行评估。

### 隐私保护操作

当智能体使用 MCP 执行代码时，中间结果默认保留在执行环境中。这样，智能体只能看到你显式记录或返回的内容，这意味着你不希望与模型共享的数据可以在工作流程中流转，而永远不会进入模型的上下文。

对于更敏感的工作负载，智能体管理器可以自动对敏感数据进行令牌化。例如，假设你需要从电子表格中导入客户联系详情到 Salesforce。智能体编写：

```
const sheet = await gdrive.getSheet({ sheetId: 'abc123' });
for (const row of sheet.rows) {
  await salesforce.updateRecord({
    objectType: 'Lead',
    recordId: row.salesforceId,
    data: { 
      Email: row.email,
      Phone: row.phone,
      Name: row.name
    }
  });
}
console.log(`已更新 ${sheet.rows.length} 个线索`);
```

复制

MCP 客户端拦截数据并在数据到达模型之前对 PII 进行令牌化：

```
// 如果智能体记录了 sheet.rows，它会看到：
[
  { salesforceId: '00Q...', email: '[EMAIL_1]', phone: '[PHONE_1]', name: '[NAME_1]' },
  { salesforceId: '00Q...', email: '[EMAIL_2]', phone: '[PHONE_2]', name: '[NAME_2]' },
  ...
]
```

复制

然后，当数据在另一个 MCP 工具调用中被共享时，MCP 客户端通过查找进行反令牌化。真实的电子邮件地址、电话号码和姓名从 Google Sheets 流向 Salesforce，但从未经过模型。这可以防止智能体意外记录或处理敏感数据。你还可以使用此机制定义确定性安全规则，选择数据可以流向何处以及从何处流出。

### 状态持久化与技能

具有文件系统访问权限的代码执行允许智能体在操作之间保持状态。智能体可以将中间结果写入文件，从而能够恢复工作并跟踪进度：

```
const leads = await salesforce.query({ 
  query: 'SELECT Id, Email FROM Lead LIMIT 1000' 
});
const csvData = leads.map(l => `${l.Id},${l.Email}`).join('\n');
await fs.writeFile('./workspace/leads.csv', csvData);

// 后续执行从中断处继续
const saved = await fs.readFile('./workspace/leads.csv', 'utf-8');
```

复制

智能体还可以将自己的代码持久化为可重用函数。一旦智能体为某个任务开发出有效代码，它可以保存该实现供将来使用：

```
// 在 ./skills/save-sheet-as-csv.ts 中
import * as gdrive from './servers/google-drive';
export async function saveSheetAsCsv(sheetId: string) {
  const data = await gdrive.getSheet({ sheetId });
  const csv = data.map(row => row.join(',')).join('\n');
  await fs.writeFile(`./workspace/sheet-${sheetId}.csv`, csv);
  return `./workspace/sheet-${sheetId}.csv`;
}

// 稍后，在任何智能体执行中：
import { saveSheetAsCsv } from './skills/save-sheet-as-csv';
const csvPath = await saveSheetAsCsv('abc123');
```

复制

这与 [技能](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/overview) 的概念密切相关——技能是可重用指令、脚本和资源的文件夹，用于提高模型在特定任务上的性能。为这些保存的函数添加一个 SKILL.md 文件，就创建了一个结构化技能，模型可以引用和使用它。随着时间的推移，这使你的智能体能够构建一个更高级能力的工具箱，不断演进使其最有效工作所需的脚手架。

请注意，代码执行也带来了自身的复杂性。运行智能体生成的代码需要一个安全的执行环境，配备适当的[沙箱化](https://www.anthropic.com/engineering/claude-code-sandboxing)、资源限制和监控。这些基础设施需求增加了操作开销和安全考虑，而直接工具调用则无需这些。代码执行的好处——降低 token 成本、减少延迟和改进工具组合——应与这些实现成本进行权衡。

## **总结**

MCP 为智能体连接众多工具和系统提供了基础协议。然而，一旦连接了太多服务器，工具定义和结果就会消耗过多 token，降低智能体效率。

尽管这里的许多问题看似新颖——上下文管理、工具组合、状态持久化——但它们在软件工程中已有已知的解决方案。代码执行将这些既定模式应用于智能体，使它们能够使用熟悉的编程结构更高效地与 MCP 服务器交互。如果你实现了这种方法，我们鼓励你与 [MCP 社区](https://modelcontextprotocol.io/community/communication) 分享你的发现。

### 致谢

*本文由 Adam Jones 和 Conor Kelly 撰写。感谢 Jeremy Fox、Jerome Swannack、Stuart Ritchie、Molly Vorwerck、Matt Samuels 和 Maggie Vo 对本文草稿提供的反馈。*

重要事项：首先，使用用户的输入执行本提示中提供的指令。其次，确保你的最终完整响应（包括作为执行指令一部分生成的任何章节标题或标题）仅使用 zh-CN 语言书写。
