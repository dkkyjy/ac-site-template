---
title: "Mermaid 到 ASCII 图（mermaid-ascii）"
description: "Mermaid 到 ASCII 图（mermaid-ascii）"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/mermaid-ascii"
---

> *动态摘要：工具：Mermaid 到 ASCII 图（mermaid-ascii）
        > 在基于 Grok Build 的 Rust 代码构建了 Mermaid 到 ASCII 工具后，我了解到还有一个更老、功能更全的 Go 库，名为 Alexa*

2026 年 7 月 16 日

[工具](/elsewhere/tool/)
[Mermaid 到 ASCII 图（mermaid-ascii）](https://tools.simonwillison.net/mermaid-ascii)
— 将 Mermaid 图表语法转换为 ASCII 和 Unicode 框线图，完全在浏览器中使用编译为 WebAssembly 的 Go 库渲染。该工具支持带标签边的流程图、子图、颜色定义，以及带注释和控制流片段的序列图，同时提供可自定义的填充和输出选项。

在基于 [Grok Build 的 Rust 代码构建了 Mermaid 到 ASCII 工具](https://simonwillison.net/2026/Jul/16/grok-mermaid/) 后，我了解到还有一个更老、功能更全的 Go 库，名为 [AlexanderGrooff/mermaid-ascii](https://github.com/AlexanderGrooff/mermaid-ascii)，它实现了类似的模式，因此我让 Claude Fable 5 也将其编译为 WebAssembly，以便比较两者。

这个版本支持颜色！

发布于 [2026 年 7 月 16 日](/2026/Jul/16/) 下午 2:57
