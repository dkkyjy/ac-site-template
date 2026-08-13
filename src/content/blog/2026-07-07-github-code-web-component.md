---
title: "github-code Web 组件"
description: "github-code Web 组件"
pubDate: "2026-07-07"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/7/github-code-component"
---

> *摘要：工具：github-code Web 组件
        > 一个使用 GPT-5.5 和以下提示构建的实验性 Web 组件：
        >
        > 让我们构建一个用于嵌入 GitHub 代码的 Web 组件
        >
        > 它接受类似这样的 URL：*

2026 年 7 月 7 日

[工具](/elsewhere/tool/)
[github-code Web 组件](https://tools.simonwillison.net/github-code-component)

一个使用 GPT-5.5 和[以下提示](https://gist.github.com/simonw/0e3db21947b5ae7e29e8a4f69a0b0617)构建的实验性 Web 组件：

> `让我们构建一个用于嵌入 GitHub 代码的 Web 组件`
>
> `<github-code href="https://github.com/simonw/sqlite-ast/blob/437c759129154f05296324a7f82aa1246340dd14/sqlite_ast/parser.py#L9-L18"></github-code>`
>
> `它接受类似这样的 URL，将其转换为 https://raw.githubusercontent.com/simonw/sqlite-ast/437c759129154f05296324a7f82aa1246340dd14/sqlite_ast/parser.py，然后使用 fetch() 获取它，并显示指定的行范围——带有行号，但没有语法高亮`
>
> `给我展示一个预览浏览器，这样我就能看到你的成果`

以下是它嵌入此页面时的样子：

发布于 [2026 年 7 月 7 日](/2026/Jul/7/) 下午 4:18
