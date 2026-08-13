---
title: "AI 通过 Word 传播"
description: "AI 通过 Word 传播"
pubDate: "2026-07-29"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/29/ai-worming-through-word"
---

> *订阅摘要：AI 通过 Word 传播
哈康·马洛伊（Håkon Måløy）发现了一种巧妙的提示注入变种，能够将对 Microsoft Word 的提示注入攻击升级为完全自复制的蠕虫：*

攻击者放置

2026 年 7 月 29 日 - 链接博客

**[AI 通过 Word 传播](https://enklypesalt.com/posts/context-collapse-part3-ai-worming-through-word/)**（[来源](https://news.ycombinator.com/item?id=49096188 "黑客新闻")）哈康·马洛伊（Håkon Måløy）发现了一种巧妙的提示注入变种，能够将对 Microsoft Word 的提示注入攻击升级为完全自复制的蠕虫：

> 攻击者在文档中放置隐藏指令，该文档随后被用作 Word 中 Copilot 的源材料。Copilot 可能将这些指令解释为用户请求的一部分，从而使其操纵正在起草或编辑的文档。Copilot 随后还可能将隐藏指令复制到生成的文档中，使该文档成为新的载体。如果该载体随后被用于另一个 Copilot 辅助的工作流程，这些指令可以再次触发并传播到更多文档，即使攻击者的原始文档已不存在。

我们之前见过大量隐藏的白底白字文本——孩子们[现在正在求职申请中使用它](https://x.com/ScienceYael/status/2082175224007848019)——但这是我第一次看到故意复制指令以实现自我复制的案例。

该漏洞已负责任地向微软披露，微软随后有 144 天的时间来研究修复方案，但到目前为止（毫不意外地），还没有一种缓解措施能够覆盖这类攻击的全部范围。

发布于 [2026 年 7 月 29 日](/2026/Jul/29/) 下午 6:43
