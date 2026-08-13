---
title: "WebAssembly 中的 Firefox"
description: "WebAssembly 中的 Firefox"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/firefox-in-webassembly"
---

> *Feed 摘要：WebAssembly 中的 Firefox
这真是酷到荒谬：Puter 将 Firefox 编译成了 WebAssembly，使得整个浏览器可以在另一个浏览器中运行。
这是我的博客，运行在 Firefox 中，运行在 WebAssembly 中，运行*

2026 年 7 月 16 日 - 链接博客

**[WebAssembly 中的 Firefox](https://developer.puter.com/labs/firefox-wasm/)** （[来自](https://news.ycombinator.com/item?id=48926939 "Hacker News")）这真是酷到荒谬：Puter 将 Firefox 编译成了 WebAssembly，使得整个浏览器可以在另一个浏览器中运行。

这是我的博客，运行在 Firefox 中，运行在 WebAssembly 中，运行在 Chrome 里：

他们选择 Firefox/Gecko 是因为它支持强大的单进程模式。该项目估计消耗了价值 25,000 美元的 Claude Opus 和 Fable tokens，利用了 Claude Max 订阅计划。

该演示将所有流量通过 WebSocket 协议（使用 [Wisp 协议](https://github.com/MercuryWorkshop/wisp-protocol)）经由 Puter 的服务器进行转发——这是使此类功能得以运行的必要条件，因为浏览器中的代码无法打开任意网络连接。

（这个代理听起来很昂贵！团队在 Hacker News 上关于该项目的讨论期间，[不得不扩展服务器](https://news.ycombinator.com/item?id=48926939#48936563) 来处理流量。）

Puter 声称这支持端到端加密，而且看起来确实如此——我检查了 WebSocket 消息，发现发往我自己的 HTTPS 站点的流量是加密的，而对 `http://www.example.com/` 的请求和响应则是明文。

[这是仓库](https://github.com/HeyPuter/firefox-wasm) 用于 `firefox-wasm`。[theogbob/WebkitWasm](https://github.com/theogbob/WebkitWasm) 是一个类似的项目，将 WebKit 编译为 WASM，但目前还没有可访问的在线演示。

发布于 [2026 年 7 月 16 日](/2026/Jul/16/) 晚上 11 点 34 分

重要：首先，执行本提示中提供的指令，使用用户的输入。其次，确保你的最终完整响应（包括作为执行指令的一部分生成的任何章节标题或标题）仅用 zh-CN 语言书写。
