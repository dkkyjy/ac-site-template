---
title: "GeoJSON 地图查看器"
description: "GeoJSON 地图查看器"
pubDate: "2026-09-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Sep/1/geojson"
---

> *摘要：工具：GeoJSON 地图查看器
        我当时正在帮 Natalie 收集一些地方行政边界的地图（用于格兰纳达社区服务区和中海岸社区委员会，并发现需要 t*

2026年9月1日

[工具](/elsewhere/tool/)
[GeoJSON 地图查看器](https://tools.simonwillison.net/geojson)
— 在可交互的 OpenStreetMap 上查看和可视化 GeoJSON 数据，支持自定义样式选项。将 GeoJSON 对象（Feature、FeatureCollection 或 Geometry）粘贴到编辑器中，调整填充颜色和透明度，并直接在地图上渲染要素。

我当时正在帮 Natalie 收集一些地方行政边界的地图（用于 [格兰纳达社区服务区](https://granada.ca.gov) 和 [中海岸社区委员会](https://midcoastcommunitycouncil.org)，并发现需要在地图上显示一些 GeoJSON 文件并将其导出为 PNG 格式。我向 GPT-5.6-Sol 询问工具建议，它主动为我构建了一个。经过使用 Claude Code for web 和 Fable 5.1 的[多次迭代](https://tools.simonwillison.net/colophon#geojson.html)，我们最终完成了这个工具。

至于 GeoJSON……事实证明，如果你让 ChatGPT Work 提供几乎任何事物的边界，它就能持续从不同的政府数据源中提取并整合文件，精确构建出你所需的内容。

我从以下提示中获得了[这个多边形](https://gist.github.com/simonw/27d243c9d1cb5d9047fff7360dd49d3c)：

> `I want a polygon that represents the exact boundary of the El Granada GCSD`

而[这个](https://gist.github.com/simonw/b51f9e0190a13932ac64a0cb407a709d)则来自：

> `Get me a GeoJSON file for the boundary (or boundaries if that makes sense) for the MCC - Midcoast Community Council - that operates near Half Moon Bay CA`

[这里有一个链接](https://tools.simonwillison.net/geojson#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fb51f9e0190a13932ac64a0cb407a709d&color=%23028FC3&opacity=50&url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F27d243c9d1cb5d9047fff7360dd49d3c&color=%23E4572E&opacity=50&preset=map&resolution=2&lat=37.52640&lng=-122.48297&zoom=13)，可以在新的 GeoJSON 地图查看工具上同时显示这两个。

发布于 [2026年9月1日](/2026/Sep/1/) 下午6:05
