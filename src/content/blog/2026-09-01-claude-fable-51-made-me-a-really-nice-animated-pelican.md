---
title: "Claude Fable 5.1 给我做了一个非常精美的动画鹈鹕"
description: "Claude Fable 5.1 给我做了一个非常精美的动画鹈鹕"
pubDate: "2026-09-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Sep/1/claude-fable-5-1"
---

> *摘要：今天是 Claude Fable（和 Mythos）5.1 发布日。Anthropic 表示 Fable 5.1"在编程、知识工作和长时间运行的问题求解任务上树立了新的标杆"。他们的公告花了相当可观的篇幅介绍科学研*

## Claude Fable 5.1 给我做了一个非常精美的动画鹈鹕

2026年9月1日

今天是 [Claude Fable（和 Mythos）5.1 发布日](https://www.anthropic.com/claude-fable-and-mythos-5-1)。Anthropic 表示 Fable 5.1"在编程、知识工作和长时间运行的问题求解任务上树立了新的标杆"。他们的公告花了相当可观的篇幅介绍科学研究，吹嘘在全新的 [Terminal-Bench-Science 0.1](https://www.terminal-bench-science.ai) 基准测试上取得了 52.6% 的成绩（该基准最早于 [8月27日](https://www.tbench.ai/news/terminal-bench-science-0-1) 公布），相比 Fable 5 的 24.7%、Opus 5 的 29.0% 和 GPT-5.6 Sol 的 22.4% 有了大幅提升。其他基准测试的分数也有小幅提升，但没有一个像科学基准那样令人印象深刻。

但它的"鹈鹕"能力到底如何呢？

早在七月，[我就写过](https://simonwillison.net/2026/Jul/16/kimi-k3/) 我对鹈鹕基准测试的信心正在消退——它与模型在其他任务上表现好坏之间的关联，似乎不像 [2025年](https://simonwillison.net/2025/Jun/6/six-months-in-llms/) 那样紧密了。我现在从中获得的最有价值的洞察，是同一模型家族内部的对比，尤其是同一提示词在不同推理强度级别下的对比。

Fable 5.1 有五个推理级别：low（低）、medium（中）、high（高）、xhigh（超高）、max（最大）——而且没有完全关闭推理的选项。

我修复了 [llm-anthropic](https://github.com/simonw/llm-anthropic) 中的一个 [问题](https://github.com/simonw/llm-anthropic/issues/88)，该问题导致推理轨迹无法被正确记录，然后运行了一些提示词。

以下是 [所有推理级别的完整鹈鹕集合](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7)，每个都附有完整的推理记录。我在这里逐一展示：

#### 低和中，都没有推理？

接下来是一个小谜团。这是我在 `low` 级别下得到的结果：

[记录](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options) 中没有显示任何摘要推理 token，输出 token 数量为 1,998。在 Claude 中，输出 token 数量包含推理 token。耗时 23.8 秒，费用为 [10.017 美分](https://www.llm-prices.com/#it=27&ot=1998&sel=claude-fable-5-1)。

我把级别调到 `medium`，得到了这个：

奇怪的是，这个也 [没有显示推理文本](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options-1)，且使用了 1,977 个输出 token——比 `low` 还少了 21 个。耗时 23 秒，费用为 [9.912 美分](https://www.llm-prices.com/#it=27&ot=1977&sel=claude-fable-5-1)。

所以对于这个特定的提示词（"生成一个骑自行车的鹈鹕的 SVG"），Fable 5.1 在 `low` 和 `medium` 两个设置下似乎完全跳过了推理。

#### 高

下面是 `high`——29.6 秒，2,612 个输出 token，[13.087 美分](https://www.llm-prices.com/#it=27&ot=2612&sel=claude-fable-5-1)：

这个确实做了一*点*推理，[摘要在这里](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning)：

> 我正在规划一个骑自行车的鹈鹕的 SVG 布局，包含天空和地面背景，一辆带两个辐条轮、车架、车座和车把的自行车，以及一只白色身体、长脖子、橙色喙的鹈鹕，骑在车上方。

和 `low` 和 `medium` 相比，其实差别真的不大。

#### 超高

到了 `xhigh`，情况变得*截然不同*。36,767 个输出 token，7 分 51 秒，[1.83 美元](https://www.llm-prices.com/#it=27&ot=36767&sel=claude-fable-5-1)！

推理轨迹[相当冗长](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-1)，其中包含这样的细节：

> 添加眼睛、翅膀向下延伸到车把握把处、橙色腿伸向踏板，以及一小片尾羽，同时故意让鹈鹕相对于自行车显得过大，以营造喜剧效果。[...]
>
> 我会接受这种轻微的厚重感，把它当作一种可爱，而不是过度工程化地去修正它。

#### 最大

将推理强度设为 `max`，我得到了 Anthropic 所有模型中我见过的最好的鹈鹕。65,927 个输出 token，13 分 54 秒，[3.30 美元](https://www.llm-prices.com/#it=27&ot=65927&sel=claude-fable-5-1)：

这个有很多值得欣赏的地方。背景很雅致，腿清晰地分在车架两侧，脚踩在踏板上，翅膀搭在车把上，鹈鹕戴了一顶可爱的蓝色帽子，车筐里还有一条鱼。

它仍然远不及 [Gemini 3.7 Flash](https://simonwillison.net/2026/Aug/13/llm-gemini/) 那种花哨程度，但我并没有*要求*花哨——我要的是一个 SVG，得到的就是一个 SVG。

[那段推理轨迹](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-2)中的一些亮点：

> 在两只脚附近添加踏板形状，远处的脚在第二条腿后面部分可见。我在考虑是否加一条小围巾或一顶帽子来增加角色感，但倾向于保持简洁，避免画面杂乱。
>
> 现在我在纠结是戴自行车头盔还是保留鹈鹕标志性的冠羽——喙和喉囊已经清楚地传达出"鹈鹕"的身份，所以头盔可以强化自行车主题而不至于丢失辨识度，不过它可能会和冠羽争夺视觉空间。
>
> 我意识到喙在 (484,84) 的位置会和圆顶头盔重叠，所以我需要缩小头盔，让它只覆盖头顶部分，调整弧线的端点使其位置更高、更窄，这样喙就能干净利落地接在前端而不产生碰撞。[...]
>
> 我正在添加一个更深的尖端区域来表现初级飞羽，然后重新考虑后缘，用扇形羽毛曲线代替一条平滑的线，让效果更自然。[...]
>
> 现在我在检查头盔上通风孔的位置，确保它们离头盔边缘足够远——考虑到描边宽度和圆头端点——并确认每个通风孔都在头盔的圆形边界之内。[...]
>
> 我决定不加车铃和轮胎高光，因为它们是不必要的添加。现在我在重新考虑前叉的弧度——当前的控制点把形状拉向了后方，但前叉应该向前弯曲以形成正确的前叉角度，所以我需要把控制点向右移来修正前叉的倾斜。

#### 好吧，让我们把它动起来

在 Hacker News 上，[swalsh 评论](https://news.ycombinator.com/item?id=49525378#49526455)了那个最大级别的鹈鹕：

> 既然鹈鹕基准已经解决了，能出个动画版吗？

我不想再花 3 美元，所以我拿最大级别的鹈鹕，把它管道传给了默认的 High 推理级别：

```
llm logs -cx | llm -m claude-fable-5-1 -s 'animate this'
```

6,121 个输入 token，26,201 个输出 token = [1.37 美元](https://www.llm-prices.com/#it=6121&ot=26201&sel=claude-fable-5-1)。结果[看起来像这样](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F87282467acb3652e0f99c85155554a32)，这里导出为视频，因为有些人浏览动画 SVG 有困难：

[![

您的浏览器不支持 HTML5 视频。
](https://static.simonwillison.net/static/2026/fable-5.1-max.webp)](https://static.simonwillison.net/static/2026/fable-5.1-animated-720-crf30-15fps.mp4)

视频里的轮子旋转方向是反的，但我认为这是转换为 MP4 时的一个瑕疵——在原始 SVG 中它们看起来方向是正确的。

发布于 [2026年9月1日](/2026/Sep/1/) 晚上 11:57 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
</think>

# Claude Fable 5.1 给我做了一个非常精美的动画鹈鹕

         **日期：** 2026-09-01 23:57 UTC
         **链接：** https://simonwillison.net/2026/Sep/1/claude-fable-5-1/
         **标签：** ai, generative-ai, llms, anthropic, claude, pelican-riding-a-bicycle, llm-reasoning, llm-release

         ---

         > *摘要：今天是 Claude Fable（和 Mythos）5.1 发布日。Anthropic 表示 Fable 5.1"在编程、知识工作和长时间运行的问题求解任务上树立了新的标杆"。他们的公告花了相当可观的篇幅介绍科学研*

## Claude Fable 5.1 给我做了一个非常精美的动画鹈鹕

2026年9月1日

今天是 [Claude Fable（和 Mythos）5.1 发布日](https://www.anthropic.com/claude-fable-and-mythos-5-1)。Anthropic 表示 Fable 5.1"在编程、知识工作和长时间运行的问题求解任务上树立了新的标杆"。他们的公告花了相当可观的篇幅介绍科学研究，吹嘘在全新的 [Terminal-Bench-Science 0.1](https://www.terminal-bench-science.ai) 基准测试上取得了 52.6% 的成绩（该基准最早于 [8月27日](https://www.tbench.ai/news/terminal-bench-science-0-1) 公布），相比 Fable 5 的 24.7%、Opus 5 的 29.0% 和 GPT-5.6 Sol 的 22.4% 有了大幅提升。其他基准测试的分数也有小幅提升，但没有一个像科学基准那样令人印象深刻。

但它的"鹈鹕"能力到底如何呢？

早在七月，[我就写过](https://simonwillison.net/2026/Jul/16/kimi-k3/) 我对鹈鹕基准测试的信心正在消退——它与模型在其他任务上表现好坏之间的关联，似乎不像 [2025年](https://simonwillison.net/2025/Jun/6/six-months-in-llms/) 那样紧密了。我现在从中获得的最有价值的洞察，是同一模型家族内部的对比，尤其是同一提示词在不同推理强度级别下的对比。

Fable 5.1 有五个推理级别：low（低）、medium（中）、high（高）、xhigh（超高）、max（最大）——而且没有完全关闭推理的选项。

我修复了 [llm-anthropic](https://github.com/simonw/llm-anthropic) 中的一个 [问题](https://github.com/simonw/llm-anthropic/issues/88)，该问题导致推理轨迹无法被正确记录，然后运行了一些提示词。

以下是 [所有推理级别的完整鹈鹕集合](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7)，每个都附有完整的推理记录。我在这里逐一展示：

#### 低和中，都没有推理？

接下来是一个小谜团。这是我在 `low` 级别下得到的结果：

[记录](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options) 中没有显示任何摘要推理 token，输出 token 数量为 1,998。在 Claude 中，输出 token 数量包含推理 token。耗时 23.8 秒，费用为 [10.017 美分](https://www.llm-prices.com/#it=27&ot=1998&sel=claude-fable-5-1)。

我把级别调到 `medium`，得到了这个：

奇怪的是，这个也 [没有显示推理文本](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#options-1)，且使用了 1,977 个输出 token——比 `low` 还少了 21 个。耗时 23 秒，费用为 [9.912 美分](https://www.llm-prices.com/#it=27&ot=1977&sel=claude-fable-5-1)。

所以对于这个特定的提示词（"生成一个骑自行车的鹈鹕的 SVG"），Fable 5.1 在 `low` 和 `medium` 两个设置下似乎完全跳过了推理。

#### 高

下面是 `high`——29.6 秒，2,612 个输出 token，[13.087 美分](https://www.llm-prices.com/#it=27&ot=2612&sel=claude-fable-5-1)：

这个确实做了一*点*推理，[摘要在这里](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning)：

> 我正在规划一个骑自行车的鹈鹕的 SVG 布局，包含天空和地面背景，一辆带两个辐条轮、车架、车座和车把的自行车，以及一只白色身体、长脖子、橙色喙的鹈鹕，骑在车上方。

和 `low` 和 `medium` 相比，其实差别真的不大。

#### 超高

到了 `xhigh`，情况变得*截然不同*。36,767 个输出 token，7 分 51 秒，[1.83 美元](https://www.llm-prices.com/#it=27&ot=36767&sel=claude-fable-5-1)！

推理轨迹[相当冗长](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-1)，其中包含这样的细节：

> 添加眼睛、翅膀向下延伸到车把握把处、橙色腿伸向踏板，以及一小片尾羽，同时故意让鹈鹕相对于自行车显得过大，以营造喜剧效果。[...]
>
> 我会接受这种轻微的厚重感，把它当作一种可爱，而不是过度工程化地去修正它。

#### 最大

将推理强度设为 `max`，我得到了 Anthropic 所有模型中我见过的最好的鹈鹕。65,927 个输出 token，13 分 54 秒，[3.30 美元](https://www.llm-prices.com/#it=27&ot=65927&sel=claude-fable-5-1)：

这个有很多值得欣赏的地方。背景很雅致，腿清晰地分在车架两侧，脚踩在踏板上，翅膀搭在车把上，鹈鹕戴了一顶可爱的蓝色帽子，车筐里还有一条鱼。

它仍然远不及 [Gemini 3.7 Flash](https://simonwillison.net/2026/Aug/13/llm-gemini/) 那种花哨程度，但我并没有*要求*花哨——我要的是一个 SVG，得到的就是一个 SVG。

[那段推理轨迹](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F17318f748f8c2b476051ddc2ebeb94a7#reasoning-2)中的一些亮点：

> 在两只脚附近添加踏板形状，远处的脚在第二条腿后面部分可见。我在考虑是否加一条小围巾或一顶帽子来增加角色感，但倾向于保持简洁，避免画面杂乱。
>
> 现在我在纠结是戴自行车头盔还是保留鹈鹕标志性的冠羽——喙和喉囊已经清楚地传达出"鹈鹕"的身份，所以头盔可以强化自行车主题而不至于丢失辨识度，不过它可能会和冠羽争夺视觉空间。
>
> 我意识到喙在 (484,84) 的位置会和圆顶头盔重叠，所以我需要缩小头盔，让它只覆盖头顶部分，调整弧线的端点使其位置更高、更窄，这样喙就能干净利落地接在前端而不产生碰撞。[...]
>
> 我正在添加一个更深的尖端区域来表现初级飞羽，然后重新考虑后缘，用扇形羽毛曲线代替一条平滑的线，让效果更自然。[...]
>
> 现在我在检查头盔上通风孔的位置，确保它们离头盔边缘足够远——考虑到描边宽度和圆头端点——并确认每个通风孔都在头盔的圆形边界之内。[...]
>
> 我决定不加车铃和轮胎高光，因为它们是不必要的添加。现在我在重新考虑前叉的弧度——当前的控制点把形状拉向了后方，但前叉应该向前弯曲以形成正确的前叉角度，所以我需要把控制点向右移来修正前叉的倾斜。

#### 好吧，让我们把它动起来

在 Hacker News 上，[swalsh 评论](https://news.ycombinator.com/item?id=49525378#49526455)了那个最大级别的鹈鹕：

> 既然鹈鹕基准已经解决了，能出个动画版吗？

我不想再花 3 美元，所以我拿最大级别的鹈鹕，把它管道传给了默认的 High 推理级别：

```
llm logs -cx | llm -m claude-fable-5-1 -s 'animate this'
```

6,121 个输入 token，26,201 个输出 token = [1.37 美元](https://www.llm-prices.com/#it=6121&ot=26201&sel=claude-fable-5-1)。结果[看起来像这样](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F87282467acb3652e0f99c85155554a32)，这里导出为视频，因为有些人浏览动画 SVG 有困难：

[![

您的浏览器不支持 HTML5 视频。
](https://static.simonwillison.net/static/2026/fable-5.1-max.webp)](https://static.simonwillison.net/static/2026/fable-5.1-animated-720-crf30-15fps.mp4)

视频里的轮子旋转方向是反的，但我认为这是转换为 MP4 时的一个瑕疵——在原始 SVG 中它们看起来方向是正确的。

发布于 [2026年9月1日](/2026/Sep/1/) 晚上 11:57 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
