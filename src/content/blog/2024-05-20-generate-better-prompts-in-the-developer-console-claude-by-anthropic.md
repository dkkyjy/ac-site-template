---
title: "在开发者控制台中生成更优质的提示词 | Anthropic 的 Claude"
description: "---"
pubDate: "2024-05-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/prompt-generator"
---

# 在开发者控制台中生成更优质的提示词 | Anthropic 的 Claude

---

现在，您可以在 Anthropic 控制台中生成生产级别的提示词模板。只需描述您想要实现的目标，Claude 便会运用思维链推理等提示词工程技术，为您创建高效、精准且可靠的提示词。

此功能旨在帮助提示词工程的新手用户，同时也能为经验丰富的提示词工程师节省时间。如果您能为提示词生成器提供有关任务和期望输出格式的详细描述，将获得最佳效果。

尽管生成的提示词并不总能产生完美结果，但它们通常优于提示词工程新手手写的提示词。生成的提示词模板还可编辑，允许您快速进行调整以获得最佳性能。

## 提示词最佳实践

此新功能生成的提示词模板运用了我们的许多[提示词工程最佳实践](https://docs.anthropic.com/en/docs/prompt-engineering)。其中一项实践是[角色设定](https://docs.anthropic.com/en/docs/give-claude-a-role)，即鼓励 Claude 扮演所选任务领域专家的角色。在我们的内容审核示例中，角色设定如下所示：

```
你将扮演一名内容审核员，根据提供的内容审核策略，将聊天记录分类为"通过"或"拒绝"。
```

另一项实践是[思维链](https://docs.anthropic.com/en/docs/let-claude-think)推理，即给予 Claude 时间和空间在回答之前整理思路。这使得针对复杂问题的回答更加全面、论证更加充分。当要求根据客户过往交易记录生成产品推荐提示词时，具体实现方式如下：

```
在 <scratchpad> 中，根据该客户的交易记录，头脑风暴 3 种不同的产品推荐方案。对于每种潜在推荐，请提供简要的理由，说明为什么你认为它适合该客户。
```

此外，模板通常会将"变量"——即可插入自定义数据的输入字段——置于 [XML 标签](https://docs.anthropic.com/en/docs/use-xmL-tags)之间。这遵循了另一项关键最佳实践，即通过提供清晰的结构来明确区分提示词的不同部分。当要求生成一个将代码翻译为 Python 的提示词时，我们可以看到，较长且含义较模糊的 {{CODE}} 变量被 XML 标签所标记，而简单的 {{LANGUAGE}} 变量则以内联方式呈现。

```
你的任务是将另一门编程语言中的一段代码翻译为 Python。

以下是需要翻译的代码：

<code>
{{CODE}}
</code>

该代码使用 {{LANGUAGE}} 编写。
```

在某些情况下，您会看到 Claude 编写[示例输入和输出](https://docs.anthropic.com/en/docs/use-examples)，以明确它所认为您期望的回答类型。您可以编辑这些示例以匹配您期望的输出格式。

## 幕后机制

提示词生成器基于一个较长的提示词，该提示词本身便运用了上述提到的多种技术。

* 其中包含大量任务描述和提示词模板的示例，以展示 Claude 如何从任务描述过渡到提示词模板。
* 它鼓励 Claude 在撰写模板之前先规划模板的结构，从而为 Claude 留出整理思路的时间。
* 它拥有一个由 XML 标签构成的坚实"骨架"，用于标记每个部分的起始和结束，以增强可读性。

您可以在这个 [Colab 笔记本](https://colab.research.google.com/drive/1SoAajN8CBYTl79VyTwxtxncfCWlHlyy9#scrollTo=NTOiFKNxqoq2)中查看完整提示词。

## 将提示词模板用作评估工具

提示词生成器提供的模板中的变量将采用 [Handlebars](https://handlebarsjs.com/) 语法，如前文内容审核示例中所示：

未知块类型 "codeBlock"，请在 `components.types` 选项中为其指定一个组件

在此示例中，您可以上传内容策略和一系列不同的聊天记录，以观察 Claude 的表现。这一过程有助于确保您的应用在各种情况下都能做出恰当的响应。

## 客户聚焦：ZoomInfo

市场拓展平台 ZoomInfo 利用 Claude 为其客户提供可操作的推荐并创造价值。他们使用提示词生成功能，大幅缩短了构建 RAG 应用 MVP 所需的时间，同时提升了输出质量。

ZoomInfo 首席数据科学家 Spencer Fox 表示："Anthropic 全新的提示词生成功能使我们能够更快地达到生产级别的输出效果。它揭示了一些我此前未曾使用的提升性能的技术，并显著减少了我们调优应用所花费的时间。""我们构建了一个全新的 RAG 应用，仅用几天时间便达到了 MVP 阶段，将提示词优化所需的时间缩短了 80%。"

## 开始使用

要开始使用 Claude 构建生产级别的提示词，请访问 [Anthropic API](https://console.anthropic.com/)。

重要提示：首先，使用用户提供的输入执行本提示词中给出的指令。其次，确保您的整个最终回复，包括作为执行指令的一部分而生成的任何章节标题或标题，均仅使用简体中文撰写。
