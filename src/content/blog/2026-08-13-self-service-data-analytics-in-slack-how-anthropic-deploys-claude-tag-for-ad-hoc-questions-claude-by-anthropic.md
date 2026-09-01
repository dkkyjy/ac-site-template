---
title: "Slack 中的自助式数据分析：Anthropic 如何部署 Claude Tag 处理即席问题 | Anthropic 的 Claude"
description: "---"
pubDate: "2026-08-13"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/self-service-data-analytics-in-slack-how-anthropic-deploys-claude-tag-for-ad-hoc-questions"
---

# Slack 中的自助式数据分析：Anthropic 如何部署 Claude Tag 处理即席问题 | Anthropic 的 Claude

---

在我们[上一篇文章](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude)中，我们介绍了如何通过三个核心组件使 Claude 以约 95% 的准确率回答数据分析问题：

* 一个受治理的语义层；
* 一组编码了我们分析规范的技能文件；以及
* 一套用于衡量性能的评估套件。

那篇文章聚焦于 [Claude Code](https://claude.com/product/claude-code)（我们数据科学家和数据工程师的主要开发平台），以及提升智能体准确性的最佳实践。

本文探讨了 Anthropic 数据团队如何将这一基础应用于公司其他成员的工作场景——借助 [Claude Tag](https://claude.com/product/tag)（公开测试版），它是我们在 Slack 中部署数据分析智能体的基础。任何人都可以向它提出数据相关问题，并获得与分析师所使用的**同一套受治理定义**相支撑的答案。

为便于说明，此处为 Claude Tag 对话的虚构再现。其中的细节、名称和工具均非真实。

## 在 Slack 中部署数据分析智能体的最佳实践

让一个智能体做到*准确*，与将它*部署到非分析师也能使用的地方*，事实证明是截然不同的两件事。我们不会重复上一篇文章中关于准确性的建议，因为那些建议在此仍然适用。

相反，我们将分享过去一年中关于如何在 Slack 中部署数据分析智能体的五个最重要经验，以及您应如何思考分发、权限、数据新鲜度和可观测性。
