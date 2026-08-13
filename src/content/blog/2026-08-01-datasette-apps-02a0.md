---
title: "datasette-apps 0.2a0"
description: "datasette-apps 0.2a0"
pubDate: "2026-08-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/1/datasette-apps"
---

> *订阅源摘要：发布：datasette-apps 0.2a0

使用 Datasette Agent 创建和编辑 Datasette Apps 时，对其进行改进的变更：

新的 app_debug() 工具允许智能体（不可见地）打开应用并测试它 us*

2026年8月1日

[发布](/elsewhere/release/)
[datasette-apps 0.2a0](https://github.com/datasette/datasette-apps/releases/tag/0.2a0)
— 存在于 Datasette 中的应用

> 使用 [Datasette Agent](https://agent.datasette.io/) 创建和编辑 Datasette Apps 时，对其进行改进的变更：
>
> * 新增 `app_debug()` 工具，允许智能体（不可见地）打开应用并使用 JavaScript 测试它。 [#33](https://github.com/datasette/datasette-apps/pull/33)
> * 新增 `app_list()` 工具，用于列出用户有权限编辑的应用，以便智能体可以编辑它们。 [#36](https://github.com/datasette/datasette-apps/issues/36)

这个 `app_debug()` 工具相当巧妙：它的工作方式是将应用显示在 `opacity: 0` 的 iframe 中，并设置 `pointer-events: none`（因此它无法被看到或与之交互），然后在该沙盒 iframe 中执行智能体提供的 JavaScript。这意味着智能体可以对应用进行冒烟测试，确认它是否正常工作，甚至可以做诸如测量不同元素尺寸之类的事情。

这使用了 [datasette-agent 0.4a0](https://simonwillison.net/2026/Jul/31/datasette-agent/) 中新增的 `context.browser_task()` 机制。

发布于 [2026年8月1日](/2026/Aug/1/) 晚上 9:23
