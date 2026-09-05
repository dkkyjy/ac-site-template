---
title: "引入动态工作流 | Claude by Anthropic"
description: "---"
pubDate: "2026-05-28"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/introducing-dynamic-workflows-in-claude-code"
---

# 引入动态工作流 | Claude by Anthropic

---

**更新：** 动态工作流现已全面可用。

今天，我们在 Claude Code 中引入了动态工作流，帮助 Claude 端到端地处理最具挑战性的任务。通常需要按季度规划的工作，现在只需数天即可完成。Claude 会动态编写编排脚本，在单次会话中运行数十到数百个并行子代理，并在任何结果交付给您之前自行检查工作。

有些问题对单个代理的单次处理来说过于庞大，尤其是在复杂、遗留的代码库中：跨整个服务的缺陷排查、涉及数百个文件的迁移、在您下定决心之前希望从各个角度进行压力测试的计划。动态工作流可以端到端地处理所有这些任务。

动态工作流现已在 Claude Code CLI、Desktop 和 VS code 扩展中全面可用，适用于 Pro、Max、Team 和 Enterprise 套餐，同时也可通过 Claude API、Amazon Bedrock、Vertex AI 和 Microsoft Foundry 使用。

注意：动态工作流消耗的 token 可能远超典型的 Claude Code 会话，因此我们建议先从范围明确的任务开始，以了解其在您工作中的使用情况。

为获得最佳体验，请在使用动态工作流时开启自动模式。此后，您有两种方式启动工作流：

1. 直接要求 Claude 创建动态工作流（例如，“创建一个工作流”），或
2. 开启一项名为 `ultracode` 的新 Claude Code 专属设置。该设置可通过努力程度菜单访问，将努力程度设为 xhigh，同时让 Claude 自动决定何时使用工作流来处理您的任务。

## **动态工作流实战**

早期访问用户和 Anthropic 内部团队已将动态工作流用于广泛的用例，包括：

* **代码库级缺陷排查、基于性能分析器的优化审计和安全审计：** Claude 并行搜索服务或仓库，然后对每项发现进行独立验证，确保报告呈现的是真实问题。同样的模式也适用于加固流程：跨整个代码库的认证检查、输入验证和不安全模式。
* **大规模迁移和现代化改造工作：** Claude 可以端到端处理框架替换、API 弃用、跨数千文件的语言移植。**‍**
* **需要双重核查的关键工作：** 当错误答案的代价很高时，工作流让 Claude 对问题进行独立尝试，并派出对抗性代理在结果呈现给您之前尝试破坏该结果。
