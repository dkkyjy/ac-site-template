---
title: "让Claude成为更优秀的电气工程师"
description: "[Diode Computers](https://www.diode.computer/) 利用人工智能设计和制造定制电路板。Diode的工具链将电路板设计转化为软件问题；就像软件工程师可以借助Claude Code等工具提高效率一样，Diode正在应用同样的技术帮助电气工程师在数小时内设计出可投产的电路板。"
pubDate: "2025-12-12"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/making-claude-a-better-electrical-engineer"
---
            [Diode Computers](https://www.diode.computer/) 利用人工智能设计和制造定制电路板。Diode的工具链将电路板设计转化为软件问题；就像软件工程师可以借助Claude Code等工具提高效率一样，Diode正在应用同样的技术帮助电气工程师在数小时内设计出可投产的电路板。

Diode开发并维护着[Zener语言](https://github.com/diodeinc/pcb/blob/main/docs/pages/spec.mdx)，这是一种基于[Starlark](https://github.com/bazelbuild/starlark)构建的领域特定语言，用于描述印刷电路板（PCB）原理图，以及[pcb](https://github.com/diodeinc/pcb?tab=readme-ov-file)，它利用Zener语言在KiCad之上提供自动化功能。

电气工程师的一项重要任务是构建参考设计：当设计师想要使用某个特定芯片时，他们需要查阅数百页的文档来了解该芯片正常运行所需的组件。一个典型的芯片可能需要多达十几个辅助部件——电阻、电容、电感等——而关于如何连接它们的信息往往结构松散且稀少。

电气工程师已经在使用Claude Code从非结构化文档中自动生成Zener参考设计，然后进行审查。但鉴于该环境的新颖性——包括领域特定的工具以及对深厚专业知识的要求——Claude在生成参考设计的代理性能以及对该特定电气工程任务的一般理解方面仍有改进空间。在自动生成参考设计的任务中，常见的失败模式包括：

* 遗漏数据手册中关于电路应如何配置的细微差别
* 错误解读参考原理图图像
* 误解或误用Zener

当我们在需要深厚专业知识的特定领域任务中发现Claude能力的不足时，我们可以与领域专家合作，教导Claude改进这些任务。这些知识被编码到公开发布的Claude模型中，以便所有Claude模型的用户——无论是在Claude Code、[claude.ai](http://claude.ai)还是他们自己基于Claude的系统和应用中——都能受益。

工程师在Claude帮助下设计的印刷电路板前工作的特写。图片来源：Diode Computers。

## 界定问题范围

我们与Diode合作开展了一项联合计划，以了解并提升Claude自动生成参考设计的能力。在这个代理任务中，Claude被要求以芯片的文档作为输入，并为其生成完整的Zener参考设计。要正确完成此任务，Claude必须阅读大量文档页面，理解密集的技术文字和图表，并编写一个完整的可配置原理图，充分表达芯片的所有配置和操作模式。在此代理设置中，Claude拥有读取和写入文件以及执行bash命令的工具，并且可以访问Zener编译器、该语言的文档以及一些示例，除此之外别无其他。

判断参考设计是否正确也并非易事。文档对于操作所需的具体组件和参数往往规定不足。为了解决这个问题，每个参考设计都使用定制的*测试平台*进行评分：测试平台不采用关于单个组件存在的绝对断言（例如“电源与地之间有一个20uF电容”），而是编码更高级别的要求（例如“电源与地之间至少有22uF的电容”）。这确保了模型获得的信号准确但不过于严格。

鉴于这个定义明确的任务以及判断任务成败的清晰标准，我们与Diode合作，在Sonnet 4.5及后续Claude模型的训练中引入了改进，以更好地自动生成电路板的参考设计。

## 对结果进行基准测试

为了对Claude在此任务上的表现进行基准测试，我们使用一组生成的参考设计作为测试集，进行了盲测正面交锋评估，涉及Claude Opus 4.1、Claude Sonnet 4和Claude Sonnet 4.5。我们发现，Diode的电气工程师在10次中有8次更偏好Claude Sonnet 4.5的参考设计。与其他模型相比，Claude Sonnet 4.5更善于捕捉文档材料中的细微差别，并且更擅长遵循其工具链的约定和语义。

在盲测正面交锋评估中，Diode的电气工程师更偏好Sonnet 4.5的参考设计，相较于Opus 4（60%对比40%）和Sonnet 4（82%对比18%）。

## 未来方向

与Diode的合作模式可以复制到任何领域或行业的任何公司，只要Claude以代理方式部署在具有明确成败标准的任务上。Anthropic正在不断改进Claude，使其成为最广泛领域和行业中最佳的虚拟协作者。需要深厚专业知识以及特定领域流程、工具和工作流的任务和工作流，是与Anthropic进行更紧密合作的绝佳候选。

如果您有兴趣与Anthropic合作改进未来版本的Claude，[请填写此表格](https://docs.google.com/forms/d/e/1FAIpQLScs9kVDB_PRyXPueayJ0c4pKUGwFdDwrKlRPsniVXCqw0utQQ/viewform?usp=dialog)，我们将与您联系。

## 致谢

本文由Diode Computers的Davide Asnaghi和Lenny Khazan，以及Anthropic的Connor Jennings、David Hershey和Nicholas Marwell合作撰写。
