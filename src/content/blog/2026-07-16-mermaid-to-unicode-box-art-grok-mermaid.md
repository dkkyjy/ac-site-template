---
title: "Mermaid 转 Unicode 框图 (grok-mermaid)"
description: "Mermaid 转 Unicode 框图 (grok-mermaid)"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/grok-mermaid"
---

> *供稿摘要: 工具: Mermaid 转 Unicode 框图 (grok-mermaid)
        在探索新开源 Grok CLI 编码代理的代码库时，我遇到了 xai-grok-markdown/src/mermaid.rs，一个“自包含的 *

2026年7月16日

[工具](/elsewhere/tool/)
[Mermaid 转 Unicode 框图 (grok-mermaid)](https://tools.simonwillison.net/grok-mermaid)

在[探索代码库](https://simonwillison.net/2026/Jul/15/grok-build/)时，针对新开源的 Grok CLI 编码代理，我遇到了 [xai-grok-markdown/src/mermaid.rs](https://github.com/xai-org/grok-build/blob/b189869b7755d2b482969acf6c92da3ecfeffd36/crates/codegen/xai-grok-markdown/src/mermaid.rs)，这是一个用 Rust 编写的“自包含的终端 Mermaid 图渲染器”。

我觉得通过 WebAssembly 在浏览器中尝试它应该会很有趣。这是我[在 Claude Code for web（Fable 5）中运行的提示](https://github.com/simonw/tools/pull/293#issue-4897479396)，而这就是最终工具的样子：

发布于 [2026年7月16日](/2026/Jul/16/) 凌晨 12:33
