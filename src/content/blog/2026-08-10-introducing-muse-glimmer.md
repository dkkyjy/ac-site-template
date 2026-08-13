---
title: "介绍 Muse Glimmer"
description: "介绍 Muse Glimmer"
pubDate: "2026-08-10"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/10/introducing-muse-glimmer"
---

> *摘要：介绍 Muse Glimmer
Meta 重返开放权重游戏！Muse Glimmer 是一个全新的 30B 模型，采用干净的 Apache 2.0 许可证（比旧的蹩脚 Llama 许可证更上一层楼）。
他们声称要 *

2026年8月10日 - 链接博客

**[介绍 Muse Glimmer](https://research.meta.ai/blog/introducing-muse-glimmer-open-agentic-model)** ([来源](https://news.ycombinator.com/item?id=49241679 "Hacker News")) Meta 重返开放权重游戏！Muse Glimmer 是一个全新的 30B 模型，采用干净的 Apache 2.0 许可证（比旧的蹩脚 Llama 许可证更上一层楼）。

他们声称已经针对我在本地模型中寻找的那些功能进行了优化：

> * **端到端智能体任务完成。** Muse Glimmer 在包括 DeepSearch QA、MCP-Atlas、𝛕-Bench 和 SWE-Bench 在内的完整任务基准测试中取得了较高的成功率，这些测试衡量了它在脚手架内工作、编写和调试代码以及从头到尾解决多轮请求的能力。
> * **可靠的工具使用。** 该模型处理广泛的函数调用，在扩展工作流程中以精确的模式调用工具。
> * **多步推理。** Muse Glimmer 在长程范围内进行链式推理，在复杂、扩展的工作流程中维持连贯的计划。 [...]

这是我用 LM Studio 的 [18.16 GB 版模型](https://lmstudio.ai/models/muse-glimmer) 生成的 [一只鹈鹕](https://gist.github.com/simonw/f20d4cd0ea7596990f7910ead616493e)：

我还用我的 [llm-coding-agent](https://github.com/simonw/llm-coding-agent) 插件试了试，针对 Datasette 的一个全新检出运行，提示词为：

> `how does auth work?`

[这是响应](https://gist.github.com/simonw/3623676275da1a6078b2500dcc78116f#response-24)，位于一个长转录文本的末尾，该文本显示了它为探索代码库而进行的所有工具调用。我使用 `llm-lmstudio` 运行它，并应用了 [这个补丁](https://github.com/agustif/llm-lmstudio/pull/14) 以升级它与 [LLM 0.32](https://llm.datasette.io/en/stable/changelog.html#v0-32) 的兼容性。

我非常喜欢这种规模的模型，因为如果一台机器有 32 GB 或更多内存（我的有 128GB），它会留下充足的空间，可以同时运行其他应用程序。

Glimmer 是一个视觉模型，所以我让它描述这张图片：

`llm -m lmstudio/meta/muse-glimmer -a https://static.inaturalist.org/photos/714731804/large.jpg 'describe image'`

以下是 [我得到的回复](https://gist.github.com/simonw/288ef0dd33a913455d1b1ecc329637e0#response)：

> 照片中是一个岩石堆成的防波堤式海岸，正值阴天，水面平滑、灰蒙蒙一片，背景柔焦中隐约可见一条码头/栈桥线。
>
> 在前景中，两只大型褐色鹈鹕（*Pelecanus occidentalis*）栖息在杂乱的灰白色石块上。它们具有该物种特征性的长而向下弯曲的橙黄色喙，带有大喉囊、细长的脖子，以及背部和翅膀上斑驳的棕灰色羽毛。左侧的鹈鹕略微转向镜头，似乎在梳理羽毛或将喙靠在胸前；头顶和后颈可见一片淡白色斑块，并竖起一小撮羽冠。右侧的鹈鹕主要面向前方/右方，头抬起，喙指向右下方，同样有淡白色的头部斑纹，翅膀上明显的横纹和较深的羽毛清晰可见。
>
> 在两只鹈鹕周围的岩石间，散布着几只小得多的深色鸟类——类似海鸥/燕鸥的鸟，羽毛呈柔和的棕灰色。一只深色鸟停在最左边的一块岩石上，另一只带褐色的鸟站在右侧鹈鹕的右边，一只带红色喙的灰色鸟在前景右下角，还有一只深色小鸟位于画面最右边缘。
>
> 整体光线平坦而漫射，使水面和天空呈现出一种柔和、近乎单色的色调，与质感丰富的岩石和鹈鹕细致的羽毛形成对比。构图将这两只大鸟作为主体，映衬在平静的水面和低矮的岩石栖息地之上。

发布于 [2026年8月10日](/2026/Aug/10/) 晚上 11:56
