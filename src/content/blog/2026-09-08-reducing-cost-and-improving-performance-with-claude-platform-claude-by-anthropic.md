---
title: "使用 Claude 平台降低成本并提升性能 | Anthropic 旗下 Claude"
description: "性能与成本往往被视为一种取舍：想花更少的钱，就得接受更差的结果。但在实践中我们发现，许多使用 Claude 平台的应用只需做好三件事，就能在性能不降的前提下大幅削减成本：最大化提示缓存命中率、在升级到前沿 Claude 模型时消除提示中的反模式、以及根据任务需求校准推理强度。我们已将上述指南整合进 [`claude-api` 技能](https://github.com/anthropics/sk"
pubDate: "2026-09-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/reducing-cost-and-improving-performance-with-claude-platform"
---

# 使用 Claude 平台降低成本并提升性能 | Anthropic 旗下 Claude

---

性能与成本往往被视为一种取舍：想花更少的钱，就得接受更差的结果。但在实践中我们发现，许多使用 Claude 平台的应用只需做好三件事，就能在性能不降的前提下大幅削减成本：最大化提示缓存命中率、在升级到前沿 Claude 模型时消除提示中的反模式、以及根据任务需求校准推理强度。我们已将上述指南整合进 [`claude-api` 技能](https://github.com/anthropics/skills/tree/main/skills/claude-api)。在本文中，我们将展示 Claude Code 配合 `claude-api` 如何常常找到既降低成本又保持甚至提升性能的方法。

## **提示缓存**

在 Claude 生成回复之前，它会先将你的提示处理为内部工作状态。这一步称为*预填充（prefill）*，是处理输入中最昂贵的环节。提示缓存会保存该状态（即键值缓存，即 KV 缓存）：当新请求以相同前缀开头时，Claude 会直接读取缓存而非重新计算。缓存读取的计费仅为完整输入价格的[一小部分](https://platform.claude.com/docs/en/about-claude/pricing)。

要确保有效利用提示缓存，有几点实际注意事项。首先，提示缓存绑定到特定模型。其次，提示缓存的读取必须在提示的整个跨度上保持*逐字节精确*。最后，提示缓存具有[有限的生存时间](https://platform.claude.com/docs/en/build-with-claude/prompt-caching#ttl-support)（TTL）。

基于以上要点，有以下几条实用建议：

* **避免在对话中途更改推理强度或思考设置**。这些设置会渲染在你的内容之前，因此属于缓存前缀的一部分。在 Claude Opus 5 和 Fable 5.1 上，你可以[在对话中途更新推理强度](https://platform.claude.com/docs/en/build-with-claude/effort#changing-effort-mid-conversation)而不会破坏缓存。

* **将易变值排除在前缀之外**。系统提示中的动态时间戳或 ID 可能在不同模型调用之间发生变化，从而导致缓存失效。

* **避免工具定义自行重新排序**。使用 Claude Messages API 时，[提示按固定顺序组装](https://platform.claude.com/docs/en/build-with-claude/prompt-caching#structuring-your-prompt)，工具定义渲染在顶部。任何对工具定义的修改都会导致缓存失效。

* **分叉对话时要格外小心**。子代理和分支仅在分叉前缀与父级逐字节相同、使用相同模型且推理强度一致时，才能共享父级的缓存。

* **避免同步工具调用和子代理的存活时间超过缓存 TTL**。如果代理在长时间运行的工具调用或子代理上阻塞，缓存可能在结果返回之前就已过期。下一轮对话必须重写缓存，费用为正常输入价格的 1.25 倍（1 小时缓存为 2 倍），而非廉价的读取价格。

### 如何修复

我们在提示缓存管理方面[积累了](https://claude.com/blog/lessons-from-building-claude-code-prompt-caching-is-everything)一些经验教训：

* **密切监控提示缓存命中率**。[Claude 控制台](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics)提供提示缓存诊断功能，包括缓存未命中的原因分析（图 1）。如果命中率意外下降，[缓存诊断 API](https://platform.claude.com/docs/en/build-with-claude/cache-diagnostics)会精确告诉你两个请求在何处产生了分歧。

图 1. Claude 控制台可通过比较连续请求并精确定位提示前缀分歧的位置，来诊断意外的缓存未命中。

* **延迟加载不常用的工具**。预先声明所有工具，但将不常用的工具标记为 [defer\_loading](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-use-with-prompt-caching#defer-loading-and-cache-preservation)：它们不会出现在缓存前缀中，仅在 Claude 通过工具搜索查找时才被追加到对话中，从而保持缓存有效。

* **将系统提示的更新以消息形式应用**。[Claude 平台](https://platform.claude.com/docs/en/build-with-claude/mid-conversation-system-messages#when-to-use-a-mid-conversation-system-message)允许你在对话中途以消息的形式添加系统指令，而非编辑系统提示本身，从而保留缓存。

* **组织请求结构，使稳定部分保持不动**。先添加静态上下文（工具定义和系统提示），再将不断增长的对话内容放在其后（图 2）。

图 2. 组织提示结构，确保动态内容追加在稳定前缀的末尾。

* **在提示缓存即将失效时再更改模型或推理强度**。某些操作，如[压缩（compaction）](https://platform.claude.com/docs/en/build-with-claude/compaction)，本身就会重写缓存的大部分（即对话内容）。这是[切换模型或推理强度的好时机](https://cognition.com/blog/devin-fusion)，因为你反正已经要承担一次缓存未命中的费用。

* **随对话增长移动缓存断点**。使用 Claude 平台，你可以设置[自动缓存](https://platform.claude.com/docs/en/build-with-claude/prompt-caching#automatic-caching)，自动将缓存断点应用到最后一个可缓存的块。

* **预热缓存**。为降低延迟，发送一个带有 `max_tokens: 0` 和显式缓存断点的请求。这会处理提示并写入缓存，但不生成任何内容。如果在会话开始时执行此操作（例如在用户输入期间），第一个真实请求就能命中预热好的缓存。

* **不要超出提示缓存的 TTL**。5 分钟的缓存 TTL 从请求开始时刻起算。如果代理在工具调用或子代理请求上阻塞且运行时间超过 5 分钟，父级缓存将在结果返回前过期。在这种情况下，考虑为前缀[设置 1 小时的 TTL](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence)。

## **指令**

提示中可能会积累用于修补模型缺陷的指令。这些指令可能相对于[最新 Claude 模型](https://x.com/trq212/status/2080710971228918066)的能力已经过时。以下是常见的提示"反模式"，它们会削弱前沿 Claude 模型的表现，并可能在不经意间增加成本：

* **验证仪式**。类似"*仔细检查你的工作*"或"*回复前验证两遍*"的指令，往往会被前沿模型字面执行，从而浪费 token。

* **详尽度和强调增强器**。"*尽可能详尽*""*关键：你必须始终……*"等表述，在与前沿模型配合时可能导致冗余输出和额外的工具调用。

* **强制流程和草稿区脚手架**。固定步骤流程（如"*在草稿区中逐步思考*"）或推理模板是前沿模型并不需要的仪式。这些脚手架会叠加在原生推理之上，消耗不必要的 token。

* **过时的示例**。针对旧模型缺陷调优的少样本示例，可能教会前沿模型在不需要的请求上模仿冗长的推理链。

* **矛盾的规则**。前沿模型在指令遵循方面表现更优。矛盾的指令（"始终按政策退款"与"未经升级审批绝不退款"）可能被前沿模型更字面地执行，导致性能下降。

* **过时的配置**。为旧一代 Claude 模型编写的设置（如手动思考预算），在升级到前沿模型时可能被 Claude 平台拒绝。

### **如何修复**

我们已更新 `claude-api` 技能，新增了一个专门检测这些反模式的命令。在 Claude Code 中，对你的提示、技能或工具描述运行 `/claude-api prompt-audit`。审计范围涵盖工作目录中的一切，包括调用 Claude API 的应用代码以及 Claude Code 自身的配置（如 [CLAUDE.md](http://claude.md) 或技能）。

例如，我们在一项客户支持基准测试上测试了从 Opus 4.8 到 Opus 5 的模型迁移。我们从一份干净的提示出发，每次植入一个反模式（一个已弃用的思考设置、一对矛盾的退款规则、一个手动草稿区、"验证两遍"、"尽可能详尽"以及一个强制六步流程），共生成六份遗留提示。

我们分别在 Opus 4.8 上运行、在仅更改模型 ID 的 Opus 5 上运行、以及在每份提示运行一次 `/claude-api prompt-audit` 后的 Opus 5 上运行（图 3 展示了六份提示的平均结果）。

图 3. 从 Opus 4.8 迁移到 Opus 5 期间，提示反模式的影响。

在 Opus 5 上，验证仪式（"*验证两遍*"）会在每次退款时重复查询订单，浪费不必要的 token。强调增强器（"*尽可能详尽*"）则变成了数十次不必要的知识库搜索。

运行 `/claude-api prompt-audit` 后，反模式被移除，成本平均降低 14.6%，准确率平均提升 5.3%。成本下降是因为多余的工具调用和重复推理被消除。准确率提升则出于三个原因。已弃用的思考设置导致 API 直接拒绝所有路由请求。矛盾的退款规则使 Opus 5 在询问客户确认期间扣留了四笔应退款项。而手动草稿区与 Opus 5 内置的思考功能发生冲突：在三个工单中，它将工具调用写在了推理内部却从未执行。

## **推理强度**

[推理强度（Effort）](https://platform.claude.com/docs/en/build-with-claude/effort)告诉 Claude "该投入多少精力"。在低推理强度下，Claude 通常能更快得出结论。在高推理强度下，Claude 会深思熟虑、验证并探索替代方案后再作答。

同一模型在不同推理强度下的成本与性能表现可能存在差异。例如，在 FrontierCode Diamond（最难的 50 个任务）上，Claude Fable 5 在低推理强度下得分为 11.5%，每个任务成本 5.35 美元。在最大推理强度下，Fable 5 得分达到 30.9%，每个任务成本 19.00 美元；提高推理强度使得分提升约 2.7 倍（+19 个百分点），而成本仅增加约 3.5 倍（图 4）。

在 Claude Fable 5.1 上，Humanity's Last Exam（不使用工具）呈现出陡峭的曲线，但最后一步收益递减。低推理强度下得分约 53%，每道题约 0.30 美元；最大推理强度下得分约 61%，每道题约 2.23 美元；最后一步提升到最大强度仅增加约 0.5 个百分点，却要多付 46% 的成本。这一增量落在基准测试的逐次运行噪声范围内，你多花了钱却看不到可衡量的提升。

图 4. Fable 5 在 FrontierCode Diamond 上不同推理强度下的性能与成本对比。

推理强度可能向两个方向出现偏差：

* **认为越高越好**。高推理强度可能导致*过度思考*。Claude 花费的斟酌时间超过了任务所需，增加了成本和延迟，甚至可能降低答案质量。只有在仍有证据可寻时，斟酌才有价值。

* **偏向低推理强度**。设置过低，Claude 在证据不足时便停止。它发起更少的工具调用，可能仅凭第一次搜索结果作答而非第三次。在困难步骤上思考更少，跳过本应自行执行的检查。答案看似完整，实则建立在部分信息之上。

### **如何修复**

校准推理强度有一些实用的方法：

* **在低推理强度下测试更强的模型**。更强模型在低推理强度下的成本可能低于较弱模型在高推理强度下的成本。例如，在 CursorBench 3.2 上，Claude Fable 5.1 在低推理强度下[达到了](https://www-cdn.anthropic.com/0339e6a7c5c7b87f5c07798616dc32c215d14235/Claude%20Fable%205.1%20&%20Claude%20Mythos%205.1%20System%20Card.pdf) Fable 5 在高推理强度下的性能，而成本仅为三分之一（图 5）。两点使新模型更便宜：低推理强度下每个任务的工作量更少，且 Fable 5.1 的提示缓存读取价格为每百万 token 0.25 美元，而 Fable 5 为 1.00 美元。即便按 Fable 5 的价格计算，Fable 5.1 在低推理强度下的成本也低约 40%。

图 5. Fable 5 与 Fable 5.1 在 CursorBench 3.2 上不同推理强度下的对比。

* **理解你的任务特征。** 在[一系列推理强度](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence#tune-effort)上测量应用性能，是理解特定任务成本-性能权衡的有效方式。在未饱和的评估中，若性能-成本曲线在不同推理强度下基本平坦，则说明该任务不受推理算力的限制；提高推理强度并无益处。

这种校准通常涉及跨模型和推理强度运行评估。在 Claude Code 中，`/claude-api hillclimb` 可自动完成这一搜索：它将你的评估集拆分为训练集和测试集，提出配置变更方案，并读取训练集中的失败案例来修复发现的问题。

我们在一个客户支持基准测试上运行了该工具，从 Opus 4.8 的默认（高）推理强度开始。爬山搜索首先尝试 Opus 5 在低推理强度下的配置，并应用 prompt-audit 移除强制工具调用仪式、草稿区步骤和矛盾规则。该配置以 98.9% 的训练集准确率超越了 Opus 4.8 基线，并将成本降至每张工单 2.6 美分。

图 6. 通过更新模型选择、推理强度和提示，爬山搜索同时改善了成本与性能。

随后它进一步降级到 Sonnet 5 低推理强度，成本更低，每张工单仅 1 美分，但准确率降至 88.9%。Claude 阅读了训练集中失败的工单，在提示中添加了路由规则和退款上限交叉引用，将 Sonnet 5 的准确率恢复到 98.9%，而成本保持不变。

在 14 张搜索过程中从未见过的留出工单上，最终配置得分 90.5%，而原始配置仅为 78.6%，成本约为原来的五分之一。

## **自动化成本削减**

提示缓存、指令和推理强度是降低成本的常见杠杆。我们的[文档](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence#cut-spend-without-losing-quality)涵盖了更多内容。要对使用 Claude API 的应用代码进行全面成本审计，我们新增了 `/claude-api cost-optimize`：它会分析你的支出分布，应用成本削减措施，并在你提供评估集的情况下，展示节省与性能之间的权衡。

`cost-optimize` 首先定位 token 的去向：如果你拥有 Claude 管理员 API 密钥，则从组织的[用量和成本报告](https://platform.claude.com/docs/en/manage-claude/usage-cost-api)中获取；如果你的应用记录了每次 API 响应中的用量对象，则从中提取；若两者皆无，则通过阅读你的请求构建代码进行估算。

然后它按优先级排列可用的节省措施，从提示缓存开始，接着是精简每次请求携带的内容（包括一次 prompt-audit）、限制输出长度，以及对无人值守的工作进行[批处理](https://platform.claude.com/docs/en/build-with-claude/batch-processing)。如果你提供了评估集，它会进一步计算不同推理强度和模型选择下的成本与性能。

我们在四个公开基准测试上运行了该工具，以 Sonnet 5 作为基线（图 7）：

* **LegalBench（成本降低约 58%）：** `cost-optimize` 建议跨任务缓存共享前缀、设置低推理强度，并通过批处理 API 处理任务。思考 token 从 102,779 降至 8,284，但通过率仍在噪声范围内，成本下降约 58%。

* **tau2-bench retail（成本降低约 73%）：** 通过实现带显式断点放置的提示缓存，`cost-optimize` 在保持通过率不变的前提下将支出削减了 73%。

* **OfficeQA Pro（成本降低约 52%）：** `cost-optimize` 添加了批处理和文档缓存，将成本从 136.20 美元降至 64.87 美元。

* **SWE-bench Verified（成本降低约 55%）：** `cost-optimize` 发现默认配置已正确缓存。节省来自将推理强度设为中等，并将代理输出限制为几句简洁的表述。每个任务的中位步骤数从 29 降至 17，提示 token 从 7520 万降至 3370 万。

 

图 7. 使用 /claude-api cost-optimize 后各基准测试的成本与性能变化。

## **快速上手**

当你已迁移到前沿 Claude 模型并希望检查现有提示时，从 `/claude-api prompt-audit` 开始。它会扫描工作目录中的提示、技能和工具描述。这些可以是调用 Claude API 的应用代码，也可以是 Claude Code 的配置（CLAUDE.md、技能）。它会移除常见的反模式，这些反模式会削弱前沿模型的表现。

当你的应用使用 Claude API 且你希望进行成本审计时，使用 `/claude-api cost-optimize`。它会分析 token 支出，然后测试不同的优化杠杆：它应用 prompt-audit，同时检查通过提示缓存、批处理无人值守工作或限制输出来降低成本的方法。如果你提供评估集，它会衡量推理强度和模型选择的权衡。

最后，使用 `/claude-api hillclimb` 进行成本和性能的迭代搜索。给定一个评估集，Claude 将其拆分为训练集和测试集，然后提出旨在降低成本同时保持基线性能的应用更新方案。Claude 通过阅读训练集中的失败案例来引导搜索，最终配置在留出的测试集上评分。

了解更多：

* 参阅我们的文档，[见此](https://platform.claude.com/docs/en/about-claude/models/optimizing-for-cost-and-intelligence#cut-spend-without-losing-quality)
* 参阅我们的示例手册，[见此](https://platform.claude.com/cookbook/cost-optimization-cost-optimization#prompt-caching)
