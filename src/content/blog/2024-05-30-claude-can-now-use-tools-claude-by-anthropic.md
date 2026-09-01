---
title: "Claude 现已支持工具使用 | Anthropic 出品 Claude"
description: "---"
pubDate: "2024-05-30"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/tool-use-ga"
---

# Claude 现已支持工具使用 | Anthropic 出品 Claude

---

工具使用功能使 Claude 能够与外部工具和 API 进行交互，现已在 Anthropic Messages API、Amazon Bedrock 及 Google Cloud 的 Vertex AI 上面向整个 Claude 3 模型系列全面开放。借助工具使用功能，Claude 可以执行任务、处理数据，并提供更加动态——也更加准确——的响应。

## 工具使用

为 Claude 定义一套工具集，并以自然语言描述您的需求。Claude 随后将自动选择合适的工具来完成该任务，并在适当时机执行相应操作：

* **从非结构化文本中提取结构化数据**：从发票中提取姓名、日期和金额，减少手动数据录入工作。
* **将自然语言请求转换为结构化 API 调用**：让团队通过简单指令即可自助完成常见操作（例如"取消订阅"）。
* **通过搜索数据库或调用网络 API 回答问题**：在客服聊天机器人中为用户提供即时、准确的响应。
* **通过软件 API 自动化简单任务**：在数据录入或文件管理中节省时间、减少错误。
* **编排多个高速 Claude 子代理处理精细化任务**：根据与会者的可用时间自动找到最优会议时间。

## 更出色的开发者体验

为了更便捷地利用 Claude 3 模型配合工具的智能能力，我们还内置了多项功能，帮助开发者进一步定制终端用户体验。

* **工具使用配合流式输出可缩短等待时间，打造更具参与感的交互体验**：流式输出使客服聊天机器人等应用能够实时响应，实现更流畅、更自然的对话。
* **强制工具使用允许开发者指定 Claude 的工具选择**：开发者可以指定 Claude 应使用哪些工具，也可以将选择权交给 Claude，从而帮助构建更具针对性和更高效的应用。
* **工具同样支持图像**：Claude 可以在实时应用中结合图像输入进行处理。

在测试期间，许多开发者使用 Opus 构建了功能丰富的面向用户的智能助手。为了进一步提升这一体验，Opus 的输出中现在将包含 <thinking> 标签，以阐明 Claude 的推理过程，并简化开发者的调试流程。目前，我们的 Claude 3 模型尚不支持并行工具调用。

## 客户案例：StudyFetch

AI 原生学习平台 [StudyFetch](https://www.claude.com/customers/studyfetch) 利用 Claude 的工具使用能力驱动其个性化 AI 导师 Spark.E。通过集成工具来追踪学生进度、浏览课程资料和讲座内容，并创建交互式用户界面，StudyFetch 为全球学生打造了一个更具吸引力的学习环境。

"配备工具使用功能的 Claude 既准确又具有成本效益，如今已支撑我们实时的语音 AI 辅导课程。短短几天内，我们就将工具集成到了我们的平台中，"StudyFetch 首席技术官兼联合创始人 Ryan Trattner 表示。"由此，我们的 AI 导师 Spark.E 能够以代理方式自主运作——展示交互式界面、在上下文中追踪学生进度，并浏览讲座和课程资料。自部署配备工具使用功能的 Claude 以来，我们观察到用户正面反馈增长了 42%。"

## 客户案例：Intuned

Intuned 是一个浏览器自动化平台，利用 Claude 驱动其云平台中的数据提取功能。借助 AI 驱动的数据提取，Intuned 能够大幅提升开发者在构建和执行更可靠的浏览器自动化任务时的体验。

"Claude 3 Haiku 配合工具使用功能对我们来说是一次革命性的突破。在接入该模型并运行基准测试后，我们发现其质量、速度和价格的组合无与伦比，"Intuned 联合创始人 Faisal Ilaiwi 表示。"Haiku 正在帮助我们将客户的数据提取任务扩展到一个全新的层次。"

## 客户案例：Hebbia

[Hebbia](https://www.claude.com/customers/hebbia) 正在为领先的金融和法律服务机构打造 AI 知识工作者。他们使用 Claude 3 Haiku 来驱动多个复杂的多步骤客户工作流。

"我们利用 Claude 3 Haiku 生成实时建议、自动化提示词编写，并从长文档中提取关键元数据，"Hebbia 产品经理 Divya Mehta 分享道。"Claude 3 Haiku 的工具使用功能为我们的平台解锁了实时生成可靠建议和提示词的能力与速度。"

## 快速上手

您现在即可在 Anthropic Messages API、Amazon Bedrock 及 Google Cloud 的 Vertex AI 上开始使用工具使用功能。如需了解更多详情，请参阅我们的[文档](https://docs.anthropic.com/en/docs/tool-use)、[工具使用教程](https://github.com/anthropics/courses/tree/master/tool_use)以及[Anthropic 工具使用食谱](https://platform.claude.com/cookbook/tool-use-calculator-tool)。
