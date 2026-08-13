---
title: "Inkling：我们的开放权重模型"
description: "Inkling：我们的开放权重模型"
pubDate: "2026-07-16"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/16/inkling"
---

> *Feed 摘要：Inkling：我们的开放权重模型  
Mira Murati 的 Thinking Machines Lab 刚刚发布了他们的第一个开放权重模型。Inkling 是一个“总参数 975B、激活参数 41B 的混合专家 Transformer”*

2026 年 7 月 16 日 - 链接博客

**[Inkling：我们的开放权重模型](https://thinkingmachines.ai/news/introducing-inkling/)**（[来源](https://news.ycombinator.com/item?id=48924912 "Hacker News")）Mira Murati 的 Thinking Machines Lab 刚刚发布了他们的第一个开放权重模型。Inkling 是一个“总参数 975B、激活参数 41B 的混合专家 Transformer”——一个采用 Apache-2.0 许可证的多模态模型，在 45 万亿 tokens 的文本、图像、音频和视频数据上训练而成。

他们还承诺推出 Inkling-Small，一个 276B（12B 激活参数）的模型，但该模型仍在测试中，权重将在“工作完成后”发布。

[模型卡](https://thinkingmachines.ai/model-card/inkling/)比我预想的美国 AI 实验室的模型卡要短得多。它链接到更简短的[训练数据文档](https://thinkingmachines.ai/training-data-documentation/)，其中几乎没有任何有趣的内容——最好用以下两段话来概括：

> Thinking Machines Lab 用于开发其 AI 服务的数据集包括公共领域的内容以及可能受知识产权保护的内容。
>
> Thinking Machines Lab 的服务是使用从开放互联网和可公开访问的数据存储库获得的公开可用内容开发的。某些数据集也来自第三方。

据 Thinking Machines 自己承认，这并不是一个前沿模型。相反，它旨在作为一个强大的基础模型，用于使用他们自己的 [Tinker 训练平台](https://thinkingmachines.ai/tinker/)进行微调：

> Inkling 并不是当今可用的最强整体模型，无论开放还是封闭。相反，一系列品质使其成为适合定制的良好开放权重基础：多模态能力、高效推理，以及可在 Tinker 上进行微调。

这次发布有很多值得喜欢的地方。它采用 Apache-2.0 许可，看起来与中国开源的开放权重模型具有竞争力——很高兴看到美国开放权重生态系统迎来了新的可行竞争者，加入了 NVIDIA Nemotron 和 Gemma 4 的行列。

以下是我使用 `curl` 命令通过 Thinking Machines API 生成的骑自行车鹈鹕的 SVG 尝试：

```
curl "https://tinker.thinkingmachines.dev/services/tinker-prod/oai/api/v1/chat/completions" \
  -H "Authorization: Bearer $TINKER_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "thinkingmachines/Inkling",
    "messages": [
      {"role": "user", "content": "Generate an SVG of a pelican riding a bicycle"}
    ],
    "stream": false
  }'
```

完整[响应在此](https://gist.github.com/simonw/8117ac4376371dd3fc2b5dbce27e0855)。

由于它是一个多模态模型，我让它描述自己的图像（在我将其渲染为 JPEG 之后），发送了以下 JSON：

```
{
  "model": "thinkingmachines/Inkling",
  "messages": [{
    "role": "user",
    "content": [
      {"type": "image_url", "image_url": {"url": "data:image/jpeg;base64,$BASE_64_JPEG"}},
      {"type": "text", "text": "Describe this image in detail"},
    ],
  }],
  "stream": false
}
```

以下是[完整响应](https://gist.github.com/simonw/dccebd0581f5c4056c5b0ca5bdc38cd9#reasoning_content)。它认为自己画了一只“鹳或海鸥”：

> 这是一幅欢快的平面矢量卡通插画，描绘了一只白色的鸟儿骑着一辆自行车穿过绿色风景。
>
> **鸟儿：** 中央角色是一只风格化的白色鸟儿——类似于鹳或海鸥——有着丰满圆润的身体。它有一个又长又突出的橙色喙，微微张开，露出一条黑色线条作为嘴巴。它的眼睛又小又黑，带有一个微小的白色高光。鸟儿有黑色翅膀和尾羽，边缘白色，细长的橙色腿末端是黑色爪子，搁在自行车橙色的脚踏板上。
>
> **自行车：** 自行车有一个简单的黑色车架，两个大轮子配有黑色轮胎、灰色内圈和黑色辐条呈十字形排列。车把是黑色的，黑色的曲柄上连接着鲜橙色的脚踏板。鸟儿身体后方可见一个白色座椅。
>
> **环境：** 场景以清澈的浅蓝色天空为背景。有两朵毛茸茸的白色云朵——一朵在左侧，另一朵较小的靠近鸟儿头部右侧。右上角有一轮明亮的黄色太阳。底部是连绵起伏的绿色山丘，自行车停放在平滑弯曲的顶部边缘上。
>
> 整体风格活泼、简单、色彩丰富，线条清晰，氛围明亮而阳光。

发布于 [2026 年 7 月 16 日](/2026/Jul/16/) 15:35
