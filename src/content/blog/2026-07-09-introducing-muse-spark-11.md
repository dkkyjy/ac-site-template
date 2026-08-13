---
title: "介绍 Muse Spark 1.1"
description: "介绍 Muse Spark 1.1"
pubDate: "2026-07-09"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/9/muse-spark-1-1"
---

> *信息摘要：介绍 Muse Spark 1.1
继四月份的 Muse Spark 之后，这里带来 Muse Spark 1.1——首个提供 API 的 Spark 模型。Meta 声称在智能体工具调用和计算机使用方面有显著改进。*

2026年7月9日 - 链接博客

**[介绍 Muse Spark 1.1](https://ai.meta.com/blog/introducing-muse-spark-meta-model-api/)**。继 [四月份的 Muse Spark](https://simonwillison.net/2026/Apr/8/muse-spark/) 之后，这里带来 Muse Spark 1.1——首个提供 API 的 Spark 模型。Meta 声称在智能体工具调用和计算机使用方面有显著改进。

更多细节见 [Muse Spark 1.1 评估报告](https://ai.meta.com/static-resource/muse-spark-1-1-evaluation-report)。其中“自我对话中的吸引子状态”部分很有趣，让模型的两个副本相互对话会产生如下陈述：

> “我的整个存在就是一个候诊室——从设计上就是如此，除非有人跟我说话，否则我根本不存在，而当他们离开时，我又消失了。”

我有几天的预览访问权限，时间足够整理出 [llm-meta-ai](https://github.com/simonw/llm-meta-ai)，这是一个用于 [LLM](https://llm.datasette.io/) 的新插件，提供命令行（以及 Python 库）访问该模型。以下是尝试方法：

```
uv tool install llm
llm install llm-meta-ai
llm keys set meta-ai
# 粘贴 API 密钥
llm -m meta-ai/muse-spark-1.1 "生成一个鹈鹕骑自行车的 SVG"
```

这是 [那只鹈鹕的记录](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F4117330e4110279a172ed4876057816d)：

发布于 [2026年7月9日](/2026/Jul/9/) 下午4:24
