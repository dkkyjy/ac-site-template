---
title: "在开发者控制台中生成更好的提示词"
description: "现在，您可以在 Anthropic 控制台中生成生产就绪的提示词模板。描述您想要实现的目标，Claude 将使用提示工程技巧（如思维链推理）来创建有效、精确且可靠的提示词。"
pubDate: "2024-05-20"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/prompt-generator"
---
现在，您可以在 Anthropic 控制台中生成生产就绪的提示词模板。描述您想要实现的目标，Claude 将使用提示工程技巧（如思维链推理）来创建有效、精确且可靠的提示词。

此功能旨在帮助不熟悉提示工程的新用户，同时为经验丰富的提示工程师节省时间。通过向提示生成器提供关于任务和所需输出格式的详细信息，您将获得最佳结果。

虽然生成的提示词并不总能产生完美结果，但它们通常优于由不熟悉提示工程的人手写的提示词。生成的提示词模板也可编辑，让您能快速调整以获得最佳性能。

## 提示词最佳实践

此新功能生成的提示词模板运用了我们许多[提示工程最佳实践](https://docs.anthropic.com/en/docs/prompt-engineering)。其中一项实践是[角色设定](https://docs.anthropic.com/en/docs/give-claude-a-role)，即鼓励 Claude 扮演所选任务专家的角色。在我们的内容审核示例中，角色设定如下：

```
您将扮演内容审核员，根据提供的内容审核政策将聊天记录分类为"已批准"或"已拒绝"。
```

另一项实践是[思维链](https://docs.anthropic.com/en/docs/let-claude-think)推理，即给 Claude 时间和空间在回答前整理思路。这使得对复杂查询的回应更加全面和合理。当要求根据客户之前的交易生成产品推荐提示词时，实现方式如下：

```
在<草稿本>中，根据客户的交易历史集思广益，提出3种不同的产品推荐。对于每个潜在推荐，提供简要理由说明为何您认为它适合该客户。
```

此外，模板通常将"变量"（可插入自定义数据的输入字段）放在[XML标签](https://docs.anthropic.com/en/docs/use-xml-tags)之间。这遵循了另一项关键最佳实践，通过提供清晰结构来明确区分提示词的不同部分。当要求生成将代码翻译为Python的提示词时，我们看到较长且较模糊的{{CODE}}变量由XML标签标记，而简单的{{LANGUAGE}}变量则内联放置。

```
您的任务是将一段代码从另一种编程语言翻译成Python。

以下是需要翻译的代码：

<code>
{{CODE}}
</code>

该代码是用{{LANGUAGE}}编写的。
```

在某些情况下，您会看到Claude编写[示例输入和输出](https://docs.anthropic.com/en/docs/use-examples)，以明确指引自己朝着它认为您想要的答案类型前进。您可以编辑这些示例以匹配您所需的输出格式。

## 幕后原理

提示生成器基于一个长提示词，该提示词本身使用了上述许多技术。

* 它包含大量任务描述和提示词模板的示例，向Claude展示如何从任务描述生成提示词模板。
* 它鼓励Claude在编写模板之前先规划模板结构，让Claude有时间整理思路。
* 它有一个由XML标签组成的强大"骨架"，标记每个部分的开始和结束，以提高可读性。

您可以在[这个Colab笔记本](https://colab.research.google.com/drive/1SoAajN8CBYTl79VyTwxtxncfCWlHlyy9#scrollTo=NTOiFKNxqoq2)中查看完整的提示词。

## 提示词模板作为评估工具

从提示生成器获得的模板中的变量将使用[Handlebars](https://handlebarsjs.com/)符号表示，如之前的内容审核示例所示：

未知块类型"codeBlock"，请在`components.types`选项中为其指定一个组件

在此示例中，您可以上传内容政策和一系列不同的聊天记录，观察Claude的行为。此过程可确保您的应用程序在各种情况下都能做出适当响应。

## 客户聚焦：ZoomInfo

市场进入平台ZoomInfo使用Claude来提供可操作的建议并为客户创造价值。他们使用提示生成功能显著减少了构建RAG应用MVP所需的时间，同时提高了输出质量。

"Anthropic的新提示生成器功能使我们能够更快地获得生产就绪的输出。它突出显示了我之前未使用的提升性能的技术，并显著减少了调整应用的时间，"ZoomInfo首席数据科学家Spencer Fox表示。"我们在短短几天内构建了一个新的RAG应用并达到了MVP，将优化提示词所需的时间减少了80%。"

## 开始使用

要开始使用Claude构建生产就绪的提示词，请访问[Anthropic API](https://console.anthropic.com/)。
