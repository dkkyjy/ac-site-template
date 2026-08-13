---
title: "Claude 现已支持工具使用"
description: "工具使用功能使Claude能够与外部工具和API进行交互，现已在Anthropic Messages API、Amazon Bedrock和Google Cloud的Vertex AI上向整个Claude 3模型系列全面开放。借助工具使用功能，Claude可以执行任务、操作数据，并提供更具动态性和准确性的响应。"
pubDate: "2024-05-30"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/tool-use-ga"
---
            工具使用功能使Claude能够与外部工具和API进行交互，现已在Anthropic Messages API、Amazon Bedrock和Google Cloud的Vertex AI上向整个Claude 3模型系列全面开放。借助工具使用功能，Claude可以执行任务、操作数据，并提供更具动态性和准确性的响应。

## 工具使用

为Claude定义工具集，并用自然语言指定您的请求。Claude随后将选择合适的工具来完成任务，并在适当时执行相应操作：

* **从非结构化文本中提取结构化数据**：从发票中提取姓名、日期和金额，以减少手动数据录入。
* **将自然语言请求转换为结构化API调用**：使团队能够通过简单命令自助完成常见操作（例如"取消订阅"）。
* **通过搜索数据库或使用Web API回答问题**：在支持聊天机器人中为客户查询提供即时、准确的响应。
* **通过软件API自动化简单任务**：节省时间并减少数据录入或文件管理中的错误。
* **协调多个快速Claude子代理处理精细任务**：根据与会者的空闲时间自动找到最佳会议时间。

## 改进的开发者体验

为了更轻松地利用Claude 3模型的智能与工具配合使用，我们还内置了帮助开发者进一步定制最终用户体验的功能。

* **带流式传输的工具使用减少等待时间，创造更具吸引力的交互**：流式传输可在客户支持聊天机器人等应用中实现实时响应，带来更流畅、更自然的对话。
* **强制工具使用允许开发者指示Claude选择工具**：开发者可以指定Claude应使用哪些工具，或将选择权留给Claude，有助于创建更具针对性和更高效的应用程序。
* **工具同样支持图像**：Claude可在实时应用中纳入图像输入。

在我们的测试版期间，许多开发者使用Opus构建了复杂的面向用户助手。为进一步增强这一体验，Opus现在将在其输出中包含<thinking>标签，阐明Claude的推理过程并简化开发者的调试流程。我们的Claude 3模型目前不支持并行工具调用。

## 客户案例：StudyFetch

AI原生学习平台[StudyFetch](https://www.claude.com/customers/studyfetch)利用Claude的工具使用功能为其个性化AI导师Spark.E提供支持。通过集成工具来跟踪学生进度、导航课程材料和讲座，以及创建交互式用户界面，StudyFetch为全球学生打造了更具吸引力的教育环境。

"具备工具使用功能的Claude既准确又经济高效，现在为我们的实时语音AI辅导课程提供支持。在短短几天内，我们就将工具集成到了平台中，"StudyFetch首席技术官兼联合创始人Ryan Trattner表示。"因此，我们的AI导师Spark.E能够以智能体方式运作——显示交互式界面、在上下文中跟踪学生进度，并导航讲座和材料。自实施具备工具使用功能的Claude以来，我们观察到积极的人类反馈增加了42%。"

## 客户案例：Intuned

浏览器自动化平台Intuned使用Claude为其云平台内的数据提取提供支持。借助AI驱动的数据提取，Intuned能够显著改善开发者在构建和执行更可靠的浏览器自动化方面的体验。

"具备工具使用功能的Claude 3 Haiku对我们来说是一个游戏规则改变者。在访问模型并运行基准测试后，我们意识到其质量、速度和价格的组合是无与伦比的，"Intuned联合创始人Faisal Ilaiwi表示。"Haiku正在帮助我们客户的数提取任务提升到一个全新的水平。"

## 客户案例：Hebbia

[Hebbia](https://www.claude.com/customers/hebbia)正在为领先的金融和法律服务公司构建AI知识工作者。他们使用Claude 3 Haiku来帮助驱动多个复杂的、多步骤的客户工作流程。

"我们利用Claude 3 Haiku生成实时建议、自动化提示编写，以及从长文档中提取关键元数据，"Hebbia产品经理Divya Mehta分享道。"Claude 3 Haiku的工具使用功能为我们的平台解锁了实时生成可靠建议和提示的能力与速度。"

## 开始使用

您今天即可在Anthropic Messages API、Amazon Bedrock和Google Cloud的Vertex AI上开始使用工具使用功能。要了解更多信息，请探索我们的[文档](https://docs.anthropic.com/en/docs/tool-use)、[工具使用教程](https://github.com/anthropics/courses/tree/master/tool_use)和[关于工具使用的Anthropic Cookbooks](https://platform.claude.com/cookbook/tool-use-calculator-tool)。
