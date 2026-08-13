---
title: "simonw/pedalican"
description: "simonw/pedalican"
pubDate: "2026-07-14"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/14/pedalican"
---

> *Feed摘要：simonw/pedalican
显然五月份首次宣布时我没注意，但今天我不小心在Codex Desktop中激活了一个“宠物”——一个小动画机器人，让人想起Clippy*

2026年7月14日 - 链接博客

**[simonw/pedalican](https://github.com/simonw/pedalican)**。显然五月份[首次宣布](https://twitter.com/OpenAIDevs/status/2050301642717950166)这些时我没留意，但今天我不小心在Codex Desktop中激活了一个“宠物”——一个小动画机器人，让人想起[Clippy](https://en.wikipedia.org/wiki/Office_Assistant)——然后我才知道你可以创建自己的宠物。

于是我照做了，现在我的桌面上有一只可爱的小鹈鹕骑着自行车蹦蹦跳跳，给我更新我的Codex任务进展。

[![

您的浏览器不支持HTML5视频。
](https://static.simonwillison.net/static/2026/pedalican-first-frame.jpg)](https://static.simonwillison.net/static/2026/pedalican.mp4)

这个过程最有趣的地方在于观察自定义宠物是如何创建的。我告诉它我想要一只骑自行车的鹈鹕作为自定义宠物，然后GPT-5.6 Sol xhigh完成了剩下的工作，使用多轮与[gpt-image-2](https://developers.openai.com/api/docs/models/gpt-image-2)交互来生成所需的精灵素材。

我让它制作了[详细的笔记](https://github.com/simonw/pedalican-pet/blob/main/notes-on-creating-a-pet.md)并记录了所有[中间步骤](https://github.com/simonw/pedalican-pet/tree/main/run)。我的GitHub仓库包含每张生成的图像和组合后的精灵表，以及每个动画循环的GIF，例如这张名为[waving.gif](https://github.com/simonw/pedalican-pet/blob/main/run/qa/previews/waving.gif)的：

那个GIF是由`gpt-image-2`生成的[单张图像](https://github.com/simonw/pedalican-pet/blob/main/run/api-generation/waving.png)合成的，看起来像这样：

而*那张*图像是通过执行[这个提示](https://github.com/simonw/pedalican-pet/blob/main/run/prompts/rows/waving.md)（针对最初生成的[角色参考图像](https://github.com/simonw/pedalican-pet/blob/main/run/api-generation/base.png)）创建的，而角色参考图像则是由[这个提示](https://github.com/simonw/pedalican-pet/blob/main/run/prompts/base-pet.md)生成的，该提示的结构如下：

> “为Codex宠物Pedalican创建一个干净的全身体参考精灵。”
>
> “宠物身份：一只紧凑可爱的幼年鹈鹕，圆润的奶油白色身体，柔软的珊瑚橙色喙和脚，骑着一辆小小的天蓝色自行车 [...]”
>
> “在完全平坦的纯品红色 #FF00FF 色度键背景上放置一个居中姿势。保持完整宠物可见、紧凑、在192x208尺寸下清晰可读，并易于动画化。[...]”

我一直在寻找利用图像生成来创建简单游戏就绪精灵的方法，所以花了一些时间深入研究这个机制，看看它是如何工作的。

关键的实现细节是开源的——特别是以下两个技能，均采用Apache 2.0许可证：

* [hatch-pet](https://github.com/openai/skills/tree/49f948faa9258a0c61caceaf225e179651397431/skills/.curated/hatch-pet) 来自 `openai/skills`
* [imagegen](https://github.com/openai/codex/tree/f90e7deea6a715bbd153044af6f475eefa749177/codex-rs/skills/src/assets/samples/imagegen) 来自 `openai/codex`

是的，GPT-5.6 Sol确实想出了“Pedalican”这个名字。我很喜欢！

发布于[2026年7月14日](/2026/Jul/14/) 晚上10点29分
