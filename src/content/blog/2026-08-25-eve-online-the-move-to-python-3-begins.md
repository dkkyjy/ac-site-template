---
title: "《EVE Online》：迈向 Python 3 的征程正式开启！"
description: "《EVE Online》：迈向 Python 3 的征程正式开启！"
pubDate: "2026-08-25"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/25/eve-online-move-to-python-3"
---

> *摘要：《EVE Online》：迈向 Python 3 的征程正式开启！
EVE Online 二十多年来一直是 Python 大规模应用中最引人入胜的典型案例之一。
他们自上线以来一直运行在 Stackless Python 上，直到*

2026 年 8 月 25 日 - Link Blog

**[《EVE Online》：迈向 Python 3 的征程正式开启！](https://www.eveonline.com/news/view/the-move-to-python-3-begins)**（[转引自](https://lobste.rs/s/e1oalq/move_python_3_begins "Lobster.rs")）EVE Online 二十多年来一直是 Python 大规模应用中最引人入胜的典型案例之一。

他们自 2003 年上线以来一直运行在 [Stackless Python](https://github.com/stackless-dev/stackless/wiki/) 上，而最近一次重大升级还是在 16 年前的 2010 年，当时升级到了 [Stackless Python 2.7](https://www.eveonline.com/news/view/stackless-python-2.7)。

他们向 Python 3 的升级将首先使用 [futurize](https://python-future.org/futurize.html) 脚本对 240 万行代码进行处理，随后对 Python 2 与 Python 3 行为存在差异的约 2 万处进行仔细的人工审查——例如 `1 / 2` 在 Python 2 中结果为 `0`，而在 Python 3 中结果为 `0.5`。

此次公告中并未提及他们计划如何替代 Stackless，但在去年的大会上，他们曾展示过 [Carbon 中的调度：告别 Stackless Python](https://youtu.be/-x299qHLQs0)，介绍了他们如何在较新游戏 EVE Frontier 的 Carbon 引擎中，使用其（现已开源的）[carbonengine/scheduler](https://github.com/carbonengine/scheduler) 库替代了 Stackless。

发布于 [2026 年 8 月 25 日](/2026/Aug/25/) 晚上 10:59
