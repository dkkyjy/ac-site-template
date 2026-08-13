---
title: "仅用500字节构建世界地图"
description: "仅用500字节构建世界地图"
pubDate: "2026-07-04"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/4/building-a-world-map-with-only-500-bytes"
---

> *Feed 摘要：仅用500字节构建世界地图
Iwo Kadziela（在 Codex 协助下）找到了一种方法，仅用 445 字节的数据就能生成一幅可信的 ASCII 世界地图：

关键技巧是使用 deflate 压缩*

2026年7月4日 - 链接博客

**[仅用500字节构建世界地图](https://www.experimentlog.com/blog/building-a-world-map-with-only-500-bytes)**（[来自](https://news.ycombinator.com/item?id=48747762 "Hacker News")） Iwo Kadziela（在 Codex 协助下）找到了一种方法，仅用 445 字节的数据就能生成一幅可信的 ASCII 世界地图：

关键技巧是使用 deflate 压缩，然后通过这段简洁的 JavaScript 代码将其整合起来。我以前不知道可以用 `data:` URI 来使用 `fetch()`：

```
fetch('data:;base64,1ZpLsgIxCEXnrM...==').then(
  r => r.body.pipeThrough(new DecompressionStream('deflate-raw'))
).then(
  s => new Response(s).text()
).then(
  t => b.innerHTML = '<pre style=font-size:.65vw>' + t
)
```

发布于 [2026年7月4日](/2026/Jul/4/) 晚上11:09
