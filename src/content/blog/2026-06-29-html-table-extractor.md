---
title: "HTML表格提取器"
description: "HTML表格提取器"
pubDate: "2026-06-29"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jun/29/html-table-extractor"
---

2026年6月29日

[工具](/elsewhere/tool/)
[HTML表格提取器](https://tools.simonwillison.net/html-table-extractor)
— 从粘贴内容中提取表格，并将其转换为多种格式。粘贴包含表格的HTML、富文本或纯文本，该工具会自动检测并显示每个表格的预览，然后允许你将其导出为HTML、Markdown、CSV、TSV或JSON格式。你还可以通过搜索页面标题直接从维基百科导入表格。

这是我不断增长的粘贴转换工具集合中的又一个新成员。该工具可接受来自浏览器的粘贴富文本（带有嵌入式HTML表格），并将每个检测到的表格转换为HTML、Markdown、CSV、TSV或JSON格式。

试一试：选择维基百科上[旧金山湾区城市和城镇列表](https://en.wikipedia.org/wiki/List_of_cities_and_towns_in_the_San_Francisco_Bay_Area)页面的所有内容，然后直接粘贴到工具中：

类似地，我最近[重建](https://github.com/simonw/tools/commit/f278e977751dbc1948baedfc2f26b6de870f60e6)了我的[富文本转Markdown](https://tools.simonwillison.net/rich-text-to-markdown)工具，增加了对表格的支持并改进了用户界面。

**更新**：事实证明，维基百科有一个开放的CORS API，用于检索任何页面的完整渲染HTML内容——[演示在此](https://tools.simonwillison.net/cors-fetch#url=https%3A%2F%2Fen.wikipedia.org%2Fw%2Fapi.php%3Faction%3Dparse%26page%3DList_of_cities_and_towns_in_the_San_Francisco_Bay_Area%26prop%3Dtext%26format%3Djson%26origin%3D%2A)——所以我让[Codex](https://gist.github.com/simonw/f226fe96f464ec7d81d6996cb466436d)添加了搜索维基百科页面的功能，然后自动导入并显示该页面中的任何表格。

发布于[2026年6月29日](/2026/Jun/29/)晚上11:38
