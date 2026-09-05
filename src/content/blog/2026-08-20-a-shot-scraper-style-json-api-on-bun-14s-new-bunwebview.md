---
title: "基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API"
description: "基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API"
pubDate: "2026-08-20"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/20/bun-webview-json-api"
---

> *摘要：研究：基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API
        今天，万众期待的 Bun 1.4 终于发布了，这是几个月前那次臭名昭著的 Rust 重写以来首个稳定版本。
我*

2026年8月20日

[研究](/elsewhere/research/)
[基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API](https://github.com/simonw/research/tree/main/bun-webview-json-api#readme)
— 这是一个零依赖、大约 150 行的 TypeScript 服务，证明了 Bun 1.4 的实验性 `Bun.WebView` 无需 Puppeteer 或 Playwright，即可提供 shot-scraper 风格的 JSON API，用于 JavaScript 求值和 PNG/JPEG/WebP 截图。它为每个请求创建一个浏览器标签页，在支持并发的同时，通过 `/javascript`、`/screenshot` 和 `/healthz` 以 JSON 形式返回页面结果和错误。

今天，万众期待的 [Bun 1.4 正式发布](https://bun.com/blog/bun-v1.4)，这是几个月前那次臭名昭著的 Rust 重写[以来](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/)的首个稳定版本。

有趣的是，Rust 重写在发布说明中被轻描淡写地带过，而发布说明中却引入了令人眼花缭乱的新特性，并声称修复了 2,900 个额外的 bug：

> Bun 1.4 从 Node.js 测试套件中新增了 +1,517 个测试——这是自 Bun 1.0 以来 Node.js 兼容性最大的一次飞跃。Bun v1.4 还修复了超过 2,900 个问题。它将空闲 CPU 使用率降低了 5 倍，内存使用率最多降低 35%，在 Linux 上的启动速度提升了 50%。它新增了 [`Bun.Image`](https://bun.com/blog/bun-v1.4#bun-image)、[`Bun.WebView`](https://bun.com/blog/bun-v1.4#bun-webview)、[`Bun.markdown`](https://bun.com/blog/bun-v1.4#bun-markdown)、[`Bun.cron()`](https://bun.com/blog/bun-v1.4#bun-cron)、[`Bun.Terminal`](https://bun.com/blog/bun-v1.4#bun-terminal)、[`bun run --parallel`](https://bun.com/blog/bun-v1.4#bun-run-parallel)、[`bun test --parallel`](https://bun.com/blog/bun-v1.4#bun-test-parallel)、[`bun audit fix`](https://bun.com/blog/bun-v1.4#bun-audit-fix)、[`bun dedupe`](https://bun.com/blog/bun-v1.4#bun-dedupe) 和 [`bun prune`](https://bun.com/blog/bun-v1.4#bun-prune)。此外，它将 Bun 从 Zig 重写为 Rust。

其中最让我眼前一亮的是 `Bun.WebView`，它为 Bun 核心添加了一等公民级别的浏览器自动化支持，可使用 macOS WebKit，或通过 Chrome DevTools 协议（CDP）控制本地 Chromium 进程。

我让 Claude Code for web 构建了一个 Web API 原型，该 API 能够加载网页并对其执行 JavaScript，灵感来自我的 [shot-scraper javascript](https://shot-scraper.datasette.io/en/stable/javascript.html) 命令行工具——部分原因是想看看这样一个服务需要多少内存。

这是[该 TypeScript 服务器实现](https://github.com/simonw/research/blob/main/bun-webview-json-api/server.ts)，看起来需要 192MB-256MB 的容器才能在复杂网页上运行完整的 Chrome——通过 cgroups 进行了测试。

发布于 [2026年8月20日](/2026/Aug/20/) 下午 3:37
</think>

# 基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API

         **日期：** 2026-08-20 15:37 UTC
         **链接：** https://simonwillison.net/2026/Aug/20/bun-webview-json-api/
         **标签：** 浏览器, javascript, ai, rust, typescript, 生成式ai, 大语言模型, 编程代理, bun

         ---

         > *摘要：研究：基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API
        今天，万众期待的 Bun 1.4 终于发布，这是几个月前那次臭名昭著的 Rust 重写以来首个稳定版本。
我*

2026年8月20日

[研究](/elsewhere/research/)
[基于 Bun 1.4 全新 Bun.WebView 的 shot-scraper 风格 JSON API](https://github.com/simonw/research/tree/main/bun-webview-json-api#readme)
— 这是一个零依赖、大约 150 行的 TypeScript 服务，证明了 Bun 1.4 的实验性 `Bun.WebView` 无需 Puppeteer 或 Playwright，即可提供 shot-scraper 风格的 JSON API，用于 JavaScript 求值以及 PNG/JPEG/WebP 截图。它为每个请求创建一个浏览器标签页，在支持并发的同时，通过 `/javascript`、`/screenshot` 和 `/healthz` 以 JSON 形式返回页面结果与错误。

今天，万众期待的 [Bun 1.4 正式发布](https://bun.com/blog/bun-v1.4)，这是几个月前那次臭名昭著的 Rust 重写[以来](https://simonwillison.net/2026/Jul/8/rewriting-bun-in-rust/)的首个稳定版本。

有趣的是，Rust 重写在发布说明中只被轻描淡写地带过，而发布说明中却引入了令人眼花缭乱的新特性，并声称修复了 2,900 个额外的 bug：

> Bun 1.4 从 Node.js 测试套件中新增了 +1,517 个测试——这是自 Bun 1.0 以来 Node.js 兼容性最大的一次飞跃。Bun v1.4 还修复了超过 2,900 个问题。它将空闲 CPU 使用率降低了 5 倍，内存使用率最多降低 35%，在 Linux 上的启动速度提升了 50%。它新增了 [`Bun.Image`](https://bun.com/blog/bun-v1.4#bun-image)、[`Bun.WebView`](https://bun.com/blog/bun-v1.4#bun-webview)、[`Bun.markdown`](https://bun.com/blog/bun-v1.4#bun-markdown)、[`Bun.cron()`](https://bun.com/blog/bun-v1.4#bun-cron)、[`Bun.Terminal`](https://bun.com/blog/bun-v1.4#bun-terminal)、[`bun run --parallel`](https://bun.com/blog/bun-v1.4#bun-run-parallel)、[`bun test --parallel`](https://bun.com/blog/bun-v1.4#bun-test-parallel)、[`bun audit fix`](https://bun.com/blog/bun-v1.4#bun-audit-fix)、[`bun dedupe`](https://bun.com/blog/bun-v1.4#bun-dedupe) 和 [`bun prune`](https://bun.com/blog/bun-v1.4#bun-prune)。此外，它将 Bun 从 Zig 重写为 Rust。

其中最让我眼前一亮的是 `Bun.WebView`，它为 Bun 核心添加了一等公民级别的浏览器自动化支持，可使用 macOS WebKit，或通过 Chrome DevTools 协议（CDP）控制本地 Chromium 进程。

我让 Claude Code for web 构建了一个 Web API 原型，该 API 能够加载网页并对其执行 JavaScript，灵感来自我的 [shot-scraper javascript](https://shot-scraper.datasette.io/en/stable/javascript.html) 命令行工具——部分原因是想看看这样一个服务需要多少内存。

这是[该 TypeScript 服务器实现](https://github.com/simonw/research/blob/main/bun-webview-json-api/server.ts)，看起来需要 192MB-256MB 的容器才能在复杂网页上运行完整的 Chrome——通过 cgroups 进行了测试。

发布于 [2026年8月20日](/2026/Aug/20/) 下午 3:37
