---
title: "何时使用多代理系统（以及何时不应使用）| Claude by Anthropic"
description: "何时使用多代理系统（以及何时不应使用）| Claude by Anthropic"
pubDate: "2026-01-23"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: ""
---

# 何时使用多代理系统（以及何时不应使用）| Claude by Anthropic

**日期:** 2026-01-23 00:00 UTC  
**链接:** https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them

---

## 什么是多代理系统？

多代理系统是一种架构，其中多个 LLM 实例在独立的对话上下文中运行，并通过代码进行协调。每个代理负责任务的一个独立部分——例如，子代理进行研究，而编排器进行规划——这保护了上下文，实现了并行工作，并允许单个代理无法维持的专业化分工。

存在多种协调模式（代理群体、基于能力系统以及消息总线架构），但本文聚焦于编排器-子代理模式：一种层级模型，其中主代理生成并管理专门化的子代理来处理特定的子任务。这种模式提供了一种直接的协调模型，是新手团队尝试多代理系统的一个良好起点。我们将在下一篇文章中详细探讨其他模式。

如今，多代理系统常被应用于单个代理表现更好的场景，不过随着模型不断改进，这种权衡也在持续演变。在 Anthropic，我们看到一些团队花费数月时间构建复杂的多代理架构，结果却发现，对单个代理进行更好的提示工程就能取得相当的效果。

在构建多代理系统并与在生产中部署它们的团队合作后，我们确定了三种多代理系统始终优于单个代理的场景：当上下文污染导致性能下降时，当任务可以并行运行时，以及当专业化有助于改进工具选择或任务聚焦时。在这些场景之外，协调成本通常超过收益。

在本文中，我们将分享如何识别单个代理的局限性，确定多代理系统擅长的三种场景，并避免常见的实现错误。

## 从单个代理入手的理由

一个设计良好的单个代理配上合适的工具，能完成远超许多开发者预期的任务。

多代理系统会引入额外开销。每增加一个代理，就多了一个潜在故障点、一套需要维护的提示，以及一个可能出现意外行为的源头。

我们观察到，有些团队构建了复杂的多代理系统，分别设置规划、执行、审查和迭代的独立代理，结果却发现每次交接都会丢失上下文，并且在协调上花费的 token 比执行还多。在我们的测试中，多代理实现通常比单代理方法多消耗 3 到 10 倍的 token 来完成等效任务。这种开销源于跨代理复制上下文、代理之间的协调消息，以及为交接而汇总结果。

## 多代理系统的决策框架

当多代理架构能够解决单个代理无法克服的特定约束时，它们才提供价值。这意味着多代理架构应保留用于那些能带来明显收益、足以证明额外成本合理性的场景。托管基础设施也可以为你处理这些问题（参见 Claude 托管代理中的[多代理编排](https://claude.com/blog/new-in-claude-managed-agents)）。

以下模式代表了我们持续观察到投资回报为正的情况。

### 上下文保护

大型语言模型具有有限的上下文窗口，响应质量会随着上下文增长而下降。当代理的上下文累积了来自某个子任务的信息，而这些信息与后续子任务无关时，就会发生上下文污染。子代理提供了隔离，每个子代理在其自己的干净上下文中运行，专注于特定任务。

考虑一个客户支持代理，它需要在诊断技术问题的同时检索订单历史。如果每次订单查询都向上下文添加数千个 token，代理推理技术问题的能力就会下降。

**单代理方法：**

```
# 单代理将所有内容累积在上下文中
conversation_history = [
    {"role": "user", "content": "我的订单 #12345 无法使用"},
    {"role": "assistant", "content": "让我查看您的订单..."},
    # 工具结果增加了 2000+ token 的订单历史
    {"role": "user", "content": "... (订单详情、过往购买记录、配送信息) ..."},
    {"role": "assistant", "content": "现在让我诊断技术问题..."},
    # 上下文现已被代理不需要的订单详情污染
]
```

代理必须在上下文中保留 2000+ token 的不相关订单历史的情况下推理技术问题，从而分散注意力并降低响应质量。

**多代理方法：**

```
from anthropic import Anthropic

client = Anthropic()

class OrderLookupAgent:
    def lookup_order(self, order_id: str) -> dict:
        # 独立代理，拥有自己的上下文
        messages = [
            {"role": "user", "content": f"获取订单 {order_id} 的基本信息"}
        ]
        response = client.messages.create(
            model="claude-sonnet-4-5",
            max_tokens=1024,
            messages=messages,
            tools=[get_order_details_tool]
        )
        # 只返回必要信息
        return extract_summary(response)

class SupportAgent:
    def handle_issue(self, user_message: str):
        if needs_order_info(user_message):
            order_id = extract_order_id(user_message)
            # 只获取所需信息，而非完整历史
            order_summary = OrderLookupAgent().lookup_order(order_id)
            # 注入简洁摘要，而非完整上下文
            context = f"订单 {order_id}: {order_summary['status']}, 购买日期 {order_summary['date']}"
        
        # 主代理上下文保持干净
        messages = [
            {"role": "user", "content": f"{context}\n\n用户问题: {user_message}"}
        ]
        response = client.messages.create(
            model="claude-sonnet-4-5",
            max_tokens=2048,
            messages=messages
        )
        return response
```

订单查询代理处理完整的订单历史并提取摘要。主代理只接收其实际需要的 50-100 个 token，从而使上下文保持聚焦。

当子任务产生大量上下文（超过 1000 token），但大部分信息与主任务无关时；当子任务定义明确，具有清晰的信息提取标准时；以及对于需要先过滤再使用的查找或检索操作时，上下文隔离最为有效。

### 并行化

并行运行多个代理可以让你探索比单个代理更大的搜索空间。这种模式在搜索和研究任务中尤其有价值。

Anthropic 的研究团队在[我们如何构建多代理研究系统](https://www.anthropic.com/engineering/multi-agent-research-system)中记录了这一点。主代理分析查询，并生成多个子代理并行研究不同方面。每个子代理独立搜索，然后返回提炼后的发现。多代理搜索通过允许在更大的信息空间中进行探索，在准确性上相比单代理方法有显著提升。

核心实现是将问题分解为独立的方面，并发运行子代理，然后综合结果。

```
import asyncio
from anthropic import AsyncAnthropic

client = AsyncAnthropic()

async def research_topic(query: str) -> dict:
    # 主代理将查询分解为研究方面
    facets = await lead_agent.decompose_query(query)
    
    # 生成子代理并行研究每个方面
    tasks = [
        research_subagent(facet) 
        for facet in facets
    ]
    results = await asyncio.gather(*tasks)
    
    # 主代理综合发现
    return await lead_agent.synthesize(results)

async def research_subagent(facet: str) -> dict:
    """每个子代理拥有自己的上下文窗口"""
    messages = [
        {"role": "user", "content": f"研究: {facet}"}
    ]
    response = await client.messages.create(
        model="claude-sonnet-4-5",
        max_tokens=4096,
        messages=messages,
        tools=[web_search, read_document]
    )
    return extract_findings(response)
```

这种覆盖范围的提升是有代价的。多代理系统通常比单代理方法多消耗 3 到 10 倍的 token 来完成等效任务。这是因为每个代理需要自己的上下文，代理之间必须交换消息进行协调，并且结果在代理间传递时必须进行总结。虽然并行化有助于减少总执行时间（与顺序运行所有工作相比），但多代理系统往往比单代理系统耗时更长，因为总计算量大幅增加。

并行化的主要好处是全面性，而非速度。当你需要在大信息空间中搜索或调查复杂问题的多个角度时，并行代理可以比单个在其上下文限制内工作的代理覆盖更多领域。代价是更高的 token 使用量，以及通常更长的总执行时间，以换取更全面的结果。

### 专业化

不同的任务有时需要不同的工具集、系统提示或专业领域知识。与其为单个代理提供数十种工具，不如使用专注于其职责的专门化代理，并配合有针对性的工具集，这样可以提高可靠性。

#### **工具集专业化**

当代理可访问的工具太多时，性能会下降。以下三个信号表明工具专业化会有所帮助：

1. **数量。** 代理拥有过多工具（通常 20 个以上）时，难以选择合适的工具。
2. **领域混淆。** 当工具跨越多个不相关领域（数据库操作、API 调用、文件系统操作）时，代理会混淆哪个领域适用于当前任务。
3. **性能下降。** 添加新工具会降低现有任务的性能，表明代理的工具管理能力已达到极限。

#### **系统提示专业化**

不同任务有时需要不同的人格、约束或指示，而这些可能相互冲突。客户支持代理需要同理心和耐心；代码审查代理需要精确和批判。合规检查代理需要严格的规则遵循；头脑风暴代理需要创造性的灵活性。当单个代理必须在冲突的行为模式之间切换时，分离为具有定制系统提示的专业化代理可以产生更一致的结果。

每个专业化代理的效果取决于其指令的质量——改善单代理输出的[提示工程最佳实践](https://claude.com/blog/best-practices-for-prompt-engineering)同样适用于每个子代理的系统提示。

#### **领域知识专业化**

某些任务受益于深入的领域上下文，而这些上下文可能会压垮通才代理。法律分析代理可能需要关于判例法和监管框架的大量上下文。医学研究代理可能需要关于临床试验方法的专业知识。与其将所有领域上下文加载到单个代理中，不如让专业化代理携带与其特定职责相关的重点专业知识。

**示例：多平台集成。** 考虑一个集成系统，代理需要跨 CRM、营销自动化和消息平台工作。每个平台有 10-15 个相关 API 端点。拥有 40 多个工具的单个代理通常难以正确选择，会混淆跨平台的类似操作。将任务拆分为具有聚焦工具集和定制提示的专业化代理可以解决选择错误。

```
from anthropic import Anthropic

client = Anthropic()

# 具有聚焦工具集和定制提示的专业化代理
class CRMAgent:
    """处理客户关系管理操作"""
    system_prompt = """你是 CRM 专家。你管理联系人、
    机会和账户记录。在更新前始终验证记录所有权，
    并在相关记录间维护数据完整性。"""
    tools = [
        crm_get_contacts,
        crm_create_opportunity,
        # 8-10 个 CRM 专用工具
    ]

class MarketingAgent:
    """处理营销自动化操作"""
    system_prompt = """你是营销自动化专家。你管理
    活动、线索评分和电子邮件序列。优先考虑数据卫生
    并尊重联系人偏好。"""
    tools = [
        marketing_get_campaigns,
        marketing_create_lead,
        # 8-10 个营销专用工具
    ]

class OrchestratorAgent:
    """将请求路由到专业化代理"""
    def execute(self, user_request: str):
        response = client.messages.create(
            model="claude-sonnet-4-5",
            max_tokens=1024,
            system="""你协调平台集成。将请求路由到相应的专家：
- CRM：联系人记录、机会、账户、销售管道
- 营销：活动、线索培育、电子邮件序列、评分
- 消息：通知、警报、团队沟通""",
            messages=[
                {"role": "user", "content": user_request}
            ],
            tools=[delegate_to_crm, delegate_to_marketing, delegate_to_messaging]
        )
        return response
```

这种模式反映了有效的专业协作，即拥有与角色匹配的工具的专家比试图在所有领域保持专业知识的通才更有效地协作。然而，专业化引入了路由复杂性。编排器必须正确分类请求并委派给正确的代理，路由错误会导致结果不佳。维护多个专业化代理也会增加提示维护开销。当领域清晰可分离且路由决策无歧义时，专业化效果最佳。

## 超越单代理架构

除了一般框架之外，某些具体信号表明单代理模式已被超越：

**接近上下文限制。** 如果代理经常使用大量上下文且性能下降，则上下文压力可能是瓶颈。请注意，最近在上下文管理方面的进展（[如压缩](https://platform.claude.com/cookbook/tool-use-automatic-context-compaction)）正在减少这一限制，使单个代理能够在更长的范围内维持有效记忆。

**管理大量工具。** 当代理拥有 15-20 个以上工具时，模型会花费大量上下文和注意力来理解其选项。在采用多代理架构之前，考虑使用[工具搜索工具](https://docs.anthropic.com/en/docs/agents-and-tools/tool-use/tool-search-tool)，它允许 Claude 按需动态发现工具，而不是预先加载所有定义。这可以将[ token 使用量降低高达 85%](https://www.anthropic.com/engineering/advanced-tool-use)，同时提高工具选择准确性。

**可并行化的子任务。** 当任务自然分解为独立部分时（跨多个来源的研究、多个组件的测试），并行子代理可以显著提速。

这些阈值将随着模型改进而发生变化。当前限制代表实际指导原则，而非根本性约束。

## 以上下文为中心的分解

在采用多代理架构时，最重要的设计决策是如何在代理之间分配工作。我们观察到团队经常错误地进行此选择，导致协调开销抵消了多代理设计的好处。

关键见解是采用**以上下文为中心**的视角，而不是以问题为中心的视角来分解工作。

**以问题为中心的分解（通常适得其反）。** 按工作类型划分（一个代理编写功能，另一个编写测试，第三个审查代码）会产生持续的协调开销。每次交接都会丢失上下文。编写测试的代理缺乏对某些实现决策背后的原因的了解，代码审查者缺乏探索和迭代的上下文。

**以上下文为中心的分解（通常有效）。** 按上下文边界划分意味着处理某个功能的代理还应该处理其测试，因为它已经拥有必要的上下文。只有当上下文能够真正隔离时，才应该拆分工作。

这一原则源于观察多代理系统的失败模式。当代理按问题类型拆分时，它们会陷入“传话游戏”，来回传递信息，每次交接都会降低保真度。在一个按软件开发角色（规划者、实现者、测试者、审查者）专业化的代理实验中，子代理在协调上花费的 token 比实际工作还多。

**有效的分解边界包括：**

* **独立的研究路径。** 调查“亚洲市场趋势”与“欧洲市场趋势”可以并行进行，无需共享上下文。
* **具有清晰接口的独立组件。** 在定义良好的 API 契约下，前端和后端工作可以并行进行。
* **黑盒验证。** 只需要运行测试并报告结果的验证者不需要实现上下文。

**有问题的分解边界包括：**

* **同一工作的连续阶段。** 同一功能的规划、实现和测试共享太多上下文。
* **紧密耦合的组件。** 需要频繁来回沟通的组件应属于同一个代理。
* **需要共享状态的工作。** 需要频繁同步理解的代理应保持在一起。

## 验证子代理模式

一种跨领域始终表现良好的多代理模式是**验证子代理**。这是一个专用代理，其唯一职责是测试或验证主代理的工作。

值得注意的是，能力更强的编排器模型（如 Claude Opus 4.5）越来越能够直接评估子代理的工作，而无需单独的验证步骤。然而，当使用能力较弱的编排器、验证需要专用工具，或者你希望在工作流中强制执行显式验证检查点时，验证子代理仍然很有价值。

验证子代理之所以成功，是因为它们避免了传话游戏问题。验证本质上需要最小的上下文传递，因此验证者可以进行黑盒测试，而无需了解系统是如何构建的完整历史。

### 实现多代理系统

主代理完成一个工作单元。在继续之前，它会生成一个验证子代理，并附带待验证的工件、明确的成功标准以及用于执行验证的工具。

验证者不需要理解工件为何以这种方式构建。它只需要确定工件是否满足指定的标准。

```
from anthropic import Anthropic

client = Anthropic()

class CodingAgent:
    def implement_feature(self, requirements: str) -> dict:
        """主代理实现功能"""
        messages = [
            {"role": "user", "content": f"实现: {requirements}"}
        ]
        response = client.messages.create(
            model="claude-sonnet-4-5",
            max_tokens=4096,
            messages=messages,
            tools=[read_file, write_file, list_directory]
        )
        return {
            "code": response.content,
            "files_changed": extract_files(response)
        }

class VerificationAgent:
    def verify_implementation(self, requirements: str, files_changed: list) -> dict:
        """独立代理验证工作"""
        messages = [
            {"role": "user", "content": f"""
需求: {requirements}
更改的文件: {files_changed}

运行测试套件并验证：
1. 所有现有测试通过
2. 新功能按指定方式工作
3. 没有明显错误或安全问题

在标记为通过之前，必须运行完整的测试套件。
仅运行几个测试后不要标记为通过。
运行: pytest --verbose
仅当所有测试都通过且无失败时，才标记为 PASSED。
"""}
        ]
        response = client.messages.create(
            model="claude-sonnet-4-5",
            max_tokens=4096,
            messages=messages,
            tools=[run_tests, execute_code, read_file]
        )
        return {
            "passed": extract_pass_fail(response),
            "issues": extract_issues(response)
        }

def implement_with_verification(requirements: str, max_attempts: int = 3):
    for attempt in range(max_attempts):
        result = CodingAgent().implement_feature(requirements)
        verification = VerificationAgent().verify_implementation(
            requirements,
            result['files_changed']
        )
        
        if verification['passed']:
            return result
        
        requirements += f"\n\n之前的尝试失败: {verification['issues']}"
    
    raise Exception(f"在 {max_attempts} 次尝试后验证失败")
```

### 多代理系统应用

验证子代理在以下情况下有效：

* **质量保证。** 运行测试套件、检查代码风格、根据架构验证输出。
* **合规性检查。** 验证文档满足政策要求，根据规则检查输出。
* **输出验证。** 在交付前确认生成的内容符合规范。
* **事实核查。** 让独立代理验证生成内容中的声明或引用。

### 早期胜利问题

验证子代理最显著的失败模式是未经全面测试就将输出标记为通过。验证者运行一两个测试，观察它们通过，然后宣布成功。

缓解策略包括：

* **具体标准。** 指定“运行完整的测试套件并报告所有失败”，而不是“确保它正常工作”。
* **全面检查。** 要求验证者测试多个场景和边缘情况。
* **负面测试。** 指示验证者尝试应失败的输入，并确认它们确实失败。
* **明确指令。** “在标记为通过之前，必须运行完整的测试套件”这一指令至关重要。如果没有对全面验证的明确要求，验证代理就会走捷径。

## 在单代理与多代理系统之间做出选择

多代理系统功能强大，但并非普遍适用。在增加多个协调代理的复杂性之前，请确认：

1. **确实存在多代理可以解决的约束**，例如上下文限制、并行化机会或专业化需求。
2. **分解遵循上下文而非问题类型。** 按工作所需的上下文来分组，而不是按工作类型。
3. **存在清晰的验证点**，子代理可以在不需要完整上下文的情况下验证工作。

我们的建议？从最简单有效的方法开始，只有在有证据支持时才增加复杂性。

*这是关于多代理系统系列文章的第一篇。更多关于单代理模式的内容，请参阅* [*构建高效代理*](https://www.anthropic.com/engineering/building-effective-agents)*。关于上下文管理策略，请参阅* [*AI 代理的高效上下文工程*](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)*。深入了解我们如何构建多代理研究系统，请参阅* [*我们如何构建多代理研究系统*](https://www.anthropic.com/engineering/multi-agent-research-system)*。*

## 致谢

由 Cara Phillips 撰写，感谢 Paul Chen、Andy Schumeister、Brad Abrams 和 Theo Chu 的贡献。
