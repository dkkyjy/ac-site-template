---
title: "让 Claude 成为更出色的电气工程师 | Anthropic 的 Claude"
description: "---"
pubDate: "2025-12-12"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/making-claude-a-better-electrical-engineer"
---

# 让 Claude 成为更出色的电气工程师 | Anthropic 的 Claude

---

[Diode Computers](https://www.diode.computer/) 利用 AI 设计并制造定制电路板。Diode 的工具链将电路板设计转化为软件问题；正如软件工程师可以使用 Claude Code 等工具提升效率一样，Diode 也在应用相同的技术，帮助电气工程师在数小时内完成可投入制造的电路板设计。

Diode 开发并维护 [Zener 语言](https://github.com/diodeinc/pcb/blob/main/docs/pages/spec.mdx)，这是一种基于 [Starlark](https://github.com/bazelbuild/starlark) 构建的领域特定语言，用于描述印刷电路板（PCB）原理图，以及 [pcb](https://github.com/diodeinc/pcb?tab=readme-ov-file)，它利用 Zener 语言在 KiCad 之上提供自动化功能。

电气工程师的一项重要任务是构建参考设计：当设计师希望使用某款芯片时，他们需要参阅数百页文档来了解该芯片正常工作所需的元件。一款典型的芯片可能需要多达十余种外围元件——电阻、电容、电感等——而关于如何连接这些元件的结构化信息往往十分有限。

电气工程师已经在用 Claude Code 从非结构化文档中自动生成 Zener 格式的参考设计，然后再进行人工审核。但鉴于这一环境的特殊性——它包含领域专用工具，且需要深厚的专业知识——Claude 在生成参考设计方面的智能体性能以及对该特定电气工程任务的整体理解仍有提升空间。在自动生成参考设计的任务中，常见的失败模式包括：

* 遗漏数据手册中关于电路配置方式的细微要点
* 误解参考原理图图像
* 误解或误用 Zener

当我们在需要深厚专业知识的领域特定任务中发现 Claude 的能力不足时，我们会与该领域的专家合作，帮助 Claude 在这些任务上实现改进。这些知识被编码到公开发布的 Claude 模型中，使所有使用 Claude 模型的用户都能从中受益，无论是通过 Claude Code、[claude.ai](http://claude.ai)，还是他们自己基于 Claude 构建的系统和应用。

一位工程师正在使用 Claude 协助设计的印刷电路板进行工作的特写。图片来源：Diode Computers。

## 界定问题范围

我们与 Diode 合作开展了一项联合计划，以理解并提升 Claude 自动生成参考设计的能力。在这一智能体任务中，Claude 需要以某款芯片的文档作为输入，并生成该芯片的完整 Zener 参考设计。要正确完成这一任务，Claude 必须阅读大量文档页面，理解密集的技术文字和图表，并编写一份完整的可配置原理图，全面表达该芯片的所有配置和工作模式。在这一智能体环境中，Claude 被赋予了读写文件和执行 bash 命令的工具，可以访问 Zener 编译器、语言文档和少量示例，但没有其他任何资源。

判断一个参考设计是否正确也并非易事。文档通常对运行所需的具体元件和参数描述不够详尽。为解决这一问题，每个参考设计都通过自定义*测试平台*进行评分：测试平台不对单个元件的存在做绝对断言（例如"电源与地之间有一个 20uF 电容"），而是编码更高层级的要求（例如"电源与地之间至少 22uF 的电容"）。这确保模型获得的信号准确但不会过于严格。

在任务定义明确、成功与失败的评判标准清晰的前提下，我们与 Diode 合作，将改进引入 Sonnet 4.5 及后续 Claude 模型的训练流程，以更好地为电路板自动生成参考设计。

## 基准测试结果

为了对 Claude 在该任务上的表现进行基准测试，我们使用了一组生成的参考设计测试集，采用 Claude Opus 4.1、Claude Sonnet 4 和 Claude Sonnet 4.5 进行盲测对比评估。我们发现，Diode 的电气工程师在 10 次中有 8 次更偏好 Claude Sonnet 4.5 生成的参考设计。与其他模型相比，Claude Sonnet 4.5 更善于捕捉文档材料中的细微差异，并且更擅长遵循其工具链的约定和语义。

在盲测对比评估中，Diode 的电气工程师对 Sonnet 4.5 的参考设计的偏好度高于 Opus 4（60% 对 40%）和 Sonnet 4（82% 对 18%）。

## 未来方向

与 Diode 的合作模式可以复制到任何领域中任何部署了 Claude 智能体执行具有明确成功与失败标准的任务的公司。Anthropic 持续改进 Claude，使其成为覆盖最广泛领域和行业的最佳虚拟协作伙伴。需要深厚专业知识且依赖领域特定流程、工具和工作流的任务，是与 Anthropic 开展更紧密合作的理想候选方向。

如果您有兴趣与 Anthropic 合作以改进未来版本的 Claude，[请填写此表单](https://docs.google.com/forms/d/e/1FAIpQLScs9kVDB_PRyXPueayJ0c4pKUGwFdDwrKlRPsniVXCqw0utQQ/viewform?usp=dialog)，我们将与您联系。

## 致谢

本文由 Diode Computers 的 Davide Asnaghi 和 Lenny Khazan 与 Anthropic 的 Connor Jennings、David Hershey 和 Nicholas Marwell 合作撰写。
