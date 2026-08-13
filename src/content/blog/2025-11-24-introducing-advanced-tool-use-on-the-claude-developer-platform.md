---
title: "在 Claude 开发者平台上引入高级工具使用"
description: "AI 代理的未来是模型能够在数百或数千个工具间无缝工作。一个集成 Git 操作、文件操作、包管理器、测试框架和部署管道的 IDE 助手。一个同时连接 Slack、GitHub、Google Drive、Jira、公司数据库和数十个 MCP 服务器的运营协调器。 要[构建有效的代理](https://www.anthropic.com/research/building-effective-agen"
pubDate: "2025-11-24"
heroImage: "/post_img.png"
tags: ["anthropic-engineering"]
originalLink: "https://www.anthropic.com/engineering/advanced-tool-use"
---

AI 代理的未来是模型能够在数百或数千个工具间无缝工作。一个集成 Git 操作、文件操作、包管理器、测试框架和部署管道的 IDE 助手。一个同时连接 Slack、GitHub、Google Drive、Jira、公司数据库和数十个 MCP 服务器的运营协调器。

要[构建有效的代理](https://www.anthropic.com/research/building-effective-agents)，它们需要能够使用无限的工具库，而无需将每个定义都预先塞入上下文。我们关于[使用 MCP 进行代码执行](https://www.anthropic.com/engineering/code-execution-with-mcp)的博客文章讨论了工具结果和定义在代理读取请求之前有时会消耗超过 50,000 个 token。代理应该按需发现和加载工具，只保留与当前任务相关的内容。

代理还需要能够从代码中调用工具。当使用自然语言调用工具时，每次调用都需要一次完整的推理过程，中间结果无论是否有用都会堆积在上下文中。代码天然适合编排逻辑，比如循环、条件语句和数据转换。代理需要能够根据手头的任务灵活选择代码执行和推理。

代理还需要从示例中学习正确的工具使用方式，而不仅仅是模式定义。JSON 模式定义了什么在结构上是有效的，但无法表达使用模式：何时包含可选参数、哪些组合有意义、或者 API 期望什么约定。

今天，我们发布了三个使这成为可能的功能：

* **工具搜索工具**，允许 Claude 使用搜索工具访问数千个工具，而不消耗其上下文窗口
* **程序化工具调用**，允许 Claude 在代码执行环境中调用工具，减少对模型上下文窗口的影响
* **工具使用示例**，提供了一个通用标准，用于演示如何有效使用给定工具

在内部测试中，我们发现这些功能帮助我们构建了使用传统工具使用模式无法实现的功能。例如，**[Claude for Excel](https://www.claude.com/claude-for-excel)** 使用程序化工具调用读取和修改包含数千行的电子表格，而不会过载模型上下文窗口。

根据我们的经验，我们相信这些功能为用 Claude 构建的可能性打开了新的大门。

## 工具搜索工具

### 挑战

MCP 工具定义提供了重要的上下文，但随着更多服务器连接，这些 token 会累积起来。考虑一个五服务器设置：

* GitHub：35 个工具（约 26K token）
* Slack：11 个工具（约 21K token）
* Sentry：5 个工具（约 3K token）
* Grafana：5 个工具（约 3K token）
* Splunk：2 个工具（约 2K token）

这就是 58 个工具，在对话开始之前就消耗了大约 55K token。如果添加更多服务器，比如 Jira（仅这一个就用了约 17K token），你很快就会接近 100K+ token 的开销。在 Anthropic，我们看到工具定义在优化前消耗了 134K token。

但 token 成本不是唯一的问题。最常见的失败是选择错误的工具和提供不正确的参数，尤其是当工具名称相似时，比如 `notification-send-user` 和 `notification-send-channel`。

### 我们的解决方案

工具搜索工具不是预先加载所有工具定义，而是按需发现工具。Claude 只看到当前任务实际需要的工具。

*工具搜索工具保留了 191,300 个 token 的上下文，而 Claude 的传统方法只有 122,800 个。*

传统方法：

* 所有工具定义预先加载（50+ MCP 工具约 72K token）
* 对话历史和系统提示词竞争剩余空间
* 任何工作开始前的总上下文消耗：约 77K token

使用工具搜索工具：

* 只预先加载工具搜索工具本身（约 500 token）
* 工具按需发现（3-5 个相关工具，约 3K token）
* 总上下文消耗：约 8.7K token，保留了 95% 的上下文窗口

这代表了 token 使用量减少了 85%，同时保持了对完整工具库的访问。内部测试显示，在处理大型工具库时，MCP 评估的准确性显著提升。Opus 4 从 49% 提高到 74%，Opus 4.5 从 79.5% 提高到 88.1%，均启用了工具搜索工具。

### 工具搜索工具如何工作

工具搜索工具让 Claude 能够动态发现工具，而不是预先加载所有定义。你向 API 提供所有工具定义，但将工具标记为 `defer_loading: true`，使其可按需发现。延迟加载的工具最初不会加载到 Claude 的上下文中。Claude 只看到工具搜索工具本身以及任何 `defer_loading: false` 的工具（你最关键的、最常用的工具）。

当 Claude 需要特定能力时，它会搜索相关工具。工具搜索工具返回匹配工具的引用，这些引用会在 Claude 的上下文中扩展为完整的定义。

例如，如果 Claude 需要与 GitHub 交互，它会搜索“github”，只有 `github.createPullRequest` 和 `github.listIssues` 被加载——而不是来自 Slack、Jira 和 Google Drive 的其他 50 多个工具。

这样，Claude 可以访问你的完整工具库，同时只需为实际需要的工具支付 token 成本。

**提示缓存说明：** 工具搜索工具不会破坏提示缓存，因为延迟加载的工具完全从初始提示中排除。它们只在 Claude 搜索后才被添加到上下文中，因此你的系统提示词和核心工具定义保持可缓存。

**实现：**

```
{
  "tools": [
    // 包含一个工具搜索工具（正则表达式、BM25 或自定义）
    {"type": "tool_search_tool_regex_20251119", "name": "tool_search_tool_regex"},

    // 将工具标记为按需发现
    {
      "name": "github.createPullRequest",
      "description": "创建一个拉取请求",
      "input_schema": {...},
      "defer_loading": true
    }
    // ... 更多带有 defer_loading: true 的延迟工具
  ]
}
```

复制

对于 MCP 服务器，你可以延迟加载整个服务器，同时保持特定的高使用率工具已加载：

```
{
  "type": "mcp_toolset",
  "mcp_server_name": "google-drive",
  "default_config": {"defer_loading": true}, # 延迟加载整个服务器
  "configs": {
    "search_files": {
"defer_loading": false
    }  // 保持最常用工具已加载
  }
}
```

复制

Claude 开发者平台提供了基于正则表达式和 BM25 的搜索工具，但你也可以使用嵌入或其他策略实现自定义搜索工具。

### 何时使用工具搜索工具

像任何架构决策一样，启用工具搜索工具涉及权衡。该功能在调用工具之前增加了一个搜索步骤，因此当上下文节省和准确性提升超过额外延迟时，它能提供最佳投资回报。

**在以下情况下使用：**

* 工具定义消耗超过 10K token
* 遇到工具选择准确性问题
* 构建包含多个服务器的 MCP 驱动的系统
* 有 10 个以上的工具可用

**在以下情况下收益较小：**

* 工具库较小（少于 10 个工具）
* 所有工具在每次会话中频繁使用
* 工具定义紧凑

## 程序化工具调用

### 挑战

随着工作流程变得更加复杂，传统工具调用会产生两个根本问题：

* **来自中间结果的上下文污染**：当 Claude 分析一个 10MB 的日志文件以寻找错误模式时，整个文件会进入其上下文窗口，即使 Claude 只需要一个错误频率的摘要。当跨多个表获取客户数据时，每条记录都会累积在上下文中，无论是否相关。这些中间结果消耗了大量的 token 预算，并可能将重要信息完全推出上下文窗口。
* **推理开销和手动综合**：每次工具调用都需要一次完整的模型推理过程。在收到结果后，Claude 必须“目测”数据以提取相关信息，推理各部分如何组合，并决定下一步做什么——所有这一切都通过自然语言处理进行。一个五工具工作流意味着五次推理过程，加上 Claude 解析每个结果、比较数值和综合结论。这既慢又容易出错。

### 我们的解决方案

程序化工具调用使 Claude 能够通过代码编排工具，而不是通过单独的 API 往返。Claude 不再逐个请求工具并将每个结果返回其上下文，而是编写代码来调用多个工具、处理其输出，并控制哪些信息实际进入其上下文窗口。

Claude 擅长编写代码，通过让它在 Python 中表达编排逻辑（而不是通过自然语言工具调用），你可以获得更可靠、更精确的控制流。循环、条件语句、数据转换和错误处理都在代码中显式表示，而不是在 Claude 的推理中隐含。

#### 示例：预算合规性检查

考虑一个常见的业务任务：“哪些团队成员超出了 Q3 旅行预算？”

你有三个工具可用：

* `get_team_members(department)` - 返回团队成员列表，包含 ID 和级别
* `get_expenses(user_id, quarter)` - 返回用户的开销明细项
* `get_budget_by_level(level)` - 返回员工级别的预算限额

**传统方法**：

* 获取团队成员 → 20 人
* 对每个人，获取其 Q3 开销 → 20 次工具调用，每次返回 50-100 个明细项（航班、酒店、餐饮、收据）
* 按员工级别获取预算限额
* 所有这些都进入 Claude 的上下文：2,000+ 开销明细项（50 KB+）
* Claude 手动汇总每个人的开销、查找其预算、将开销与预算限额进行比较
* 更多到模型的往返，显著的上下文消耗

**使用程序化工具调用**：

每个工具结果不再返回给 Claude，而是由 Claude 编写一个 Python 脚本，编排整个工作流。该脚本在代码执行工具（一个沙箱环境）中运行，在需要工具结果时暂停。当你通过 API 返回工具结果时，它们由脚本处理，而不是被模型消耗。脚本继续执行，Claude 只看到最终输出。

程序化工具调用使 Claude 能够通过代码编排工具，而不是通过单独的 API 往返，允许并行工具执行。

以下是 Claude 为预算合规任务编写的编排代码：

```
team = await get_team_members("engineering")

# 获取每个唯一级别的预算
levels = list(set(m["level"] for m in team))
budget_results = await asyncio.gather(*[
    get_budget_by_level(level) for level in levels
])

# 创建查找字典：{"junior": budget1, "senior": budget2, ...}
budgets = {level: budget for level, budget in zip(levels, budget_results)}

# 并行获取所有开销
expenses = await asyncio.gather(*[
    get_expenses(m["id"], "Q3") for m in team
])

# 查找超出旅行预算的员工
exceeded = []
for member, exp in zip(team, expenses):
    budget = budgets[member["level"]]
    total = sum(e["amount"] for e in exp)
    if total > budget["travel_limit"]:
        exceeded.append({
            "name": member["name"],
            "spent": total,
            "limit": budget["travel_limit"]
        })

print(json.dumps(exceeded))
```

复制

Claude 的上下文只接收最终结果：超出预算的那两三个人。2,000+ 个明细项、中间求和以及预算查找不会影响 Claude 的上下文，消耗从 200KB 的原始开销数据减少到仅 1KB 的结果。

效率提升显著：

* **Token 节省**：通过将中间结果保留在 Claude 上下文之外，PTC 大幅减少了 token 消耗。平均使用量从 43,588 降至 27,297 token，在复杂研究任务上减少了 37%。
* **降低延迟**：每次 API 往返都需要模型推理（数百毫秒到几秒）。当 Claude 在一个代码块中编排 20+ 次工具调用时，你消除了 19+ 次推理过程。API 处理工具执行，而无需每次返回模型。
* **提高准确性**：通过编写显式的编排逻辑，Claude 比在自然语言中处理多个工具结果时出错更少。内部知识检索从 25.6% 提高到 28.5%；[GIA 基准测试](https://arxiv.org/abs/2311.12983) 从 46.5% 提高到 51.2%。

生产工作流涉及混乱的数据、条件逻辑和需要扩展的操作。程序化工具调用让 Claude 以编程方式处理这种复杂性，同时将注意力集中在可操作的结果上，而不是原始数据处理。

### 程序化工具调用如何工作

#### 1. 将工具标记为可从代码调用

为工具添加 code_execution，并设置 allowed_callers 以选择加入程序化执行：

```
{
  "tools": [
    {
      "type": "code_execution_20250825",
      "name": "code_execution"
    },
    {
      "name": "get_team_members",
      "description": "获取部门的所有成员……",
      "input_schema": {...},
      "allowed_callers": ["code_execution_20250825"] # 选择加入程序化工具调用
    },
    {
      "name": "get_expenses",
 	...
    },
    {
      "name": "get_budget_by_level",
	...
    }
  ]
}
```

复制

API 将这些工具定义转换为 Claude 可以调用的 Python 函数。

#### 2. Claude 编写编排代码

Claude 不再逐个请求工具，而是生成 Python 代码：

```
{
  "type": "server_tool_use",
  "id": "srvtoolu_abc",
  "name": "code_execution",
  "input": {
    "code": "team = get_team_members('engineering')\n..." # 上面的代码示例
  }
}
```

复制

#### 3. 工具执行而不影响 Claude 的上下文

当代码调用 get_expenses() 时，你会收到一个带有 caller 字段的工具请求：

```
{
  "type": "tool_use",
  "id": "toolu_xyz",
  "name": "get_expenses",
  "input": {"user_id": "emp_123", "quarter": "Q3"},
  "caller": {
    "type": "code_execution_20250825",
    "tool_id": "srvtoolu_abc"
  }
}
```

复制

你提供结果，该结果在代码执行环境中处理，而不是在 Claude 的上下文中。这种请求-响应循环对代码中的每个工具调用重复。

#### 4. 只有最终输出进入上下文

当代码运行完成时，只有代码的结果返回给 Claude：

```
{
  "type": "code_execution_tool_result",
  "tool_use_id": "srvtoolu_abc",
  "content": {
    "stdout": "[{\"name\": \"Alice\", \"spent\": 12500, \"limit\": 10000}...]"
  }
}
```

复制

这就是 Claude 看到的所有内容，而不是过程中处理的 2,000+ 开销明细项。

### 何时使用程序化工具调用

程序化工具调用为你的工作流添加了一个代码执行步骤。当 token 节省、延迟改进和准确性提升显着时，这种额外开销是值得的。

**最有益的情况：**

* 处理大型数据集，只需要聚合或摘要
* 运行包含三个或更多依赖工具调用的多步骤工作流
* 在 Claude 看到结果之前对工具结果进行过滤、排序或转换
* 处理中间数据不应影响 Claude 推理的任务
* 跨多个项目运行并行操作（例如，检查 50 个端点）

**收益较小的情况：**

* 进行简单的单工具调用
* 处理 Claude 应查看并推理所有中间结果的任务
* 进行响应较小的快速查找

## 工具使用示例

### 挑战

JSON Schema 擅长定义结构——类型、必填字段、允许的枚举——但它无法表达使用模式：何时包含可选参数、哪些组合有意义、或者 API 期望什么约定。

考虑一个支持工单 API：

```
{
  "name": "create_ticket",
  "input_schema": {
    "properties": {
      "title": {"type": "string"},
      "priority": {"enum": ["low", "medium", "high", "critical"]},
      "labels": {"type": "array", "items": {"type": "string"}},
      "reporter": {
        "type": "object",
        "properties": {
          "id": {"type": "string"},
          "name": {"type": "string"},
          "contact": {
            "type": "object",
            "properties": {
              "email": {"type": "string"},
              "phone": {"type": "string"}
            }
          }
        }
      },
      "due_date": {"type": "string"},
      "escalation": {
        "type": "object",
        "properties": {
          "level": {"type": "integer"},
          "notify_manager": {"type": "boolean"},
          "sla_hours": {"type": "integer"}
        }
      }
    },
    "required": ["title"]
  }
}
```

复制

模式定义了什么是有效的，但留下了关键问题未回答：

* **格式歧义：** `due_date` 应使用 "2024-11-06"、"Nov 6, 2024" 还是 "2024-11-06T00:00:00Z"？
* **ID 约定：** `reporter.id` 是 UUID、"USR-12345" 还是只是 "12345"？
* **嵌套结构使用：** Claude 何时应填充 `reporter.contact`？
* **参数相关性：** `escalation.level` 和 `escalation.sla_hours` 如何与优先级关联？

这些歧义可能导致格式错误的工具调用和参数使用不一致。

### 我们的解决方案

工具使用示例让你直接在工具定义中提供示例工具调用。Claude 不再仅仅依赖模式，而是看到具体的使用模式：

```
{
    "name": "create_ticket",
    "input_schema": { /* 与上面相同的模式 */ },
    "input_examples": [
      {
        "title": "登录页面返回 500 错误",
        "priority": "critical",
        "labels": ["bug", "authentication", "production"],
        "reporter": {
          "id": "USR-12345",
          "name": "Jane Smith",
          "contact": {
            "email": "jane@acme.com",
            "phone": "+1-555-0123"
          }
        },
        "due_date": "2024-11-06",
        "escalation": {
          "level": 2,
          "notify_manager": true,
          "sla_hours": 4
        }
      },
      {
        "title": "添加深色模式支持",
        "labels": ["feature-request", "ui"],
        "reporter": {
          "id": "USR-67890",
          "name": "Alex Chen"
        }
      },
      {
        "title": "更新 API 文档"
      }
    ]
  }
```

复制

从这三个示例中，Claude 学会：

* **格式约定**：日期使用 YYYY-MM-DD，用户 ID 遵循 USR-XXXXX，标签使用 kebab-case
* **嵌套结构模式**：如何构建 reporter 对象及其嵌套的 contact 对象
* **可选参数相关性**：严重 bug 有完整联系信息 + 带紧 SLA 的升级；功能请求有 reporter 但没有 contact/escalation；内部任务只有标题

在我们自己的内部测试中，工具使用示例在复杂参数处理上将准确性从 72% 提高到 90%。

### 何时使用工具使用示例

工具使用示例为你的工具定义增加了 token，因此当准确性提升超过额外成本时，它们最有价值。

**最有益的情况：**

* 复杂的嵌套结构，其中有效的 JSON 并不意味着正确的使用
* 具有许多可选参数且包含模式重要的工具
* 具有模式中未捕获的领域特定约定的 API
* 相似的工具，其中示例阐明了使用哪一个（例如，`create_ticket` 与 `create_incident`）

**收益较小的情况：**

* 使用方式明显的简单单参数工具
* 标准格式，如 URL 或电子邮件，Claude 已经理解
* 验证问题更适合由 JSON Schema 约束处理

## 最佳实践

构建执行真实世界操作的代理需要同时处理规模、复杂性和精确性。这三个功能协同工作，解决工具使用工作流中的不同瓶颈。以下是如何有效组合它们。

### 战略性地分层使用功能

并非每个代理都需要为给定任务使用全部三个功能。从最大的瓶颈开始：

* 工具定义导致上下文膨胀 → 工具搜索工具
* 大型中间结果污染上下文 → 程序化工具调用
* 参数错误和格式错误的调用 → 工具使用示例

这种专注的方法可以让你解决限制代理性能的具体约束，而不是一开始就增加复杂性。

然后根据需要添加更多功能。它们是互补的：工具搜索工具确保找到正确的工具，程序化工具调用确保高效执行，工具使用示例确保正确调用。

### 设置工具搜索工具以获得更好的发现

工具搜索匹配名称和描述，因此清晰、描述性的定义可以提高发现准确性。

```
// 好
{
    "name": "search_customer_orders",
    "description": "按日期范围、状态或总金额搜索客户订单。返回订单详情，包括商品、配送和支付信息。"
}

// 差
{
    "name": "query_db_orders",
    "description": "执行订单查询"
}
```

复制

添加系统提示词指导，让 Claude 知道可用内容：

```
你可以使用 Slack 消息、Google Drive 文件管理、Jira 工单跟踪和 GitHub 仓库操作的工具。使用工具搜索来查找特定功能。
```

复制

保持你最常用的三到五个工具始终加载，其余延迟加载。这平衡了对常见操作的即时访问和按需发现其余内容。

### 设置程序化工具调用以实现正确执行

由于 Claude 编写代码来解析工具输出，请清晰地记录返回格式。这有助于 Claude 编写正确的解析逻辑：

```
{
    "name": "get_orders",
    "description": "获取客户的订单。
返回：
    订单对象列表，每个包含：
    - id (str)：订单标识符
    - total (float)：订单总金额（美元）
    - status (str)：'pending'、'shipped'、'delivered' 之一
    - items (list)：{sku, quantity, price} 数组
    - created_at (str)：ISO 8601 时间戳"
}
```

复制

请参见下方，选择加入从程序化编排中受益的工具：

* 可以并行运行的工具（独立操作）
* 安全重试的操作（幂等操作）

### 设置工具使用示例以实现参数准确性

为行为清晰度设计示例：

* 使用真实数据（真实城市名称、合理价格，而不是“string”或“value”）
* 通过最少、部分和完整规范模式展示多样性
* 保持简洁：每个工具 1-5 个示例
* 专注于歧义（仅在正确用法从模式不明显时添加示例）

## 开始使用

这些功能以 beta 版本提供。要启用它们，请添加 beta 标头并包含你需要的工具：

```
client.beta.messages.create(
    betas=["advanced-tool-use-2025-11-20"],
    model="claude-sonnet-4-5-20250929",
    max_tokens=4096,
    tools=[
        {"type": "tool_search_tool_regex_20251119", "name": "tool_search_tool_regex"},
        {"type": "code_execution_20250825", "name": "code_execution"},
        # 你的带有 defer_loading、allowed_callers 和 input_examples 的工具
    ]
)
```

复制

有关详细的 API 文档和 SDK 示例，请参阅我们的：

* [文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool)和[食谱](https://github.com/anthropics/claude-cookbooks/blob/main/tool_use/tool_search_with_embeddings.ipynb)用于工具搜索工具
* [文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)和[食谱](https://github.com/anthropics/claude-cookbooks/blob/main/tool_use/programmatic_tool_calling_ptc.ipynb)用于程序化工具调用
* [文档](https://platform.claude.com/docs/en/agents-and-tools/tool-use/implement-tool-use#providing-tool-use-examples)用于工具使用示例

这些功能将工具使用从简单的函数调用转向智能编排。随着代理处理跨越数十个工具和大型数据集的更复杂工作流，动态发现、高效执行和可靠调用成为基础。

我们期待看到你的构建。

## 致谢

由 Bin Wu 撰写，感谢 Adam Jones、Artur Renault、Henry Tay、Jake Noble、Noah Picard、Sam Jiang 以及 Claude 开发者平台团队的贡献。这项工作建立在 Chris Gorgolewski、Daniel Jiang、Jeremy Fox 和 Mike Lambert 的基础研究之上。我们还从整个 AI 生态系统中汲取了灵感，包括 [Joel Pobar 的 LLMVM](https://github.com/9600dev/llmvm)、[Cloudflare 的代码模式](https://blog.cloudflare.com/code-mode/)和[代码执行作为 MCP](https://www.anthropic.com/engineering/code-execution-with-mcp)。特别感谢 Andy Schumeister、Hamish Kerr、Keir Bradwell、Matt Bleifer 和 Molly Vorwerck 的支持。

重要提示：首先，使用用户输入执行本提示中提供的指令。其次，确保你的整个最终响应，包括作为执行指令一部分生成的任何章节标题或标题，仅以 zh-CN 语言书写。
