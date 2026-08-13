---
title: "开源AI差距地图"
description: "开源AI差距地图"
pubDate: "2026-07-03"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/3/open-source-ai-gap-map"
---

> *摘要：开源AI差距地图
        Current AI 是“一个构建AI公共选项的全球合作伙伴”，于2025年2月在巴黎的AI行动峰会上作为非营利组织成立，并获得了大量资本支持（$4*

2026年7月3日 - 链接博客

**[开源AI差距地图](https://map.currentai.org)**。 [Current AI](https://www.currentai.org) 是“一个构建AI公共选项的全球合作伙伴”，于2025年2月在巴黎的AI行动峰会上作为非营利组织成立，并获得了大量资本支持（已承诺4亿美元）。

他们几天前[发布了差距地图](https://www.currentai.org/blogs/introducing-the-gap-map-v0-1)——试图索引当前开源AI的状态：

> Gap Map v0.1 详细列出了421个产品：266个软件工具和库、85个模型、50个数据集和20个硬件项目，由228个组织生产。这些产品被组织成14个类别，分布在技术栈的3个层次（模型组件、产品/用户体验和基础设施）。剩余的24,400个工件构成了开源AI生态系统中未分类的长尾，在它们被研究和引用之前不会获得评分。

这个地图本身值得探索，但我对底层数据更感兴趣——这些数据以MIT许可证发布在[currentai-org/os-ai-map](https://github.com/currentai-org/os-ai-map) GitHub账户中：1,184个YAML文件，以及用于收集它们的笔记本、模式和其他脚本。

由于文件托管在GitHub上，您可以使用Datasette Lite探索其中一些——这里是将[项目跟踪的16,185个GitHub仓库](https://lite.datasette.io/?csv=https://github.com/currentai-org/os-ai-map/blob/main/warehouse/catalog/goodailist/repos.csv#/data/repos?_sort_desc=stars)作为CSV文件加载到Datasette Lite中的结果。

发布于[2026年7月3日](/2026/Jul/3/) 晚上10:04
