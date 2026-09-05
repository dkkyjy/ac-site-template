---
title: "介绍 Hy4 预览版"
description: "介绍 Hy4 预览版"
pubDate: "2026-08-29"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/29/hy4"
---

> *摘要：介绍 Hy4 预览版
全新的开源权重纯文本（不含视觉）大语言模型，由中国公司腾讯今日发布：770B 总参数量，49B 活跃参数量，1M token 上下文窗口，Hugging Face 上 1.56TB。
T*

2026年8月29日 - 链接博客

**[介绍 Hy4 预览版](https://hy.tencent.ai/research/hy4-preview)**。中国公司腾讯今日发布的全新开源权重纯文本（不含视觉）大语言模型：770B 总参数量，49B 活跃参数量，1M token 上下文窗口，[Hugging Face 上 1.56TB](https://huggingface.co/tencent/Hy4-preview)。

相比他们七月份发布的 [Hy3](https://huggingface.co/tencent/Hy3)（295B 总参数、21B 活跃参数、256,000 上下文、598GB），这次的规模有了大幅提升。

我最近开始使用模型聊天模板来更好地了解它们的能力。以下是 Hy4 在 Hugging Face 上的 [chat\_template.jinja](https://huggingface.co/tencent/Hy4-preview/blob/main/chat_template.jinja)，其中包含以下部分：

```
{%- if not reasoning_effort is defined %}
     {%- set reasoning_effort = 'high' %}
{%- elif reasoning_effort not in ['high', 'no_think'] %}
     {%- if reasoning_effort is none %}
         {{- raise_exception('reasoning_effort error : None, should be no_think/high') }}
     {%- else %}
         {{- raise_exception('reasoning_effort error : ' + reasoning_effort + ', should be no_think/high') }}
     {%- endif %}
{%- endif %}
```

看起来推理力度只有两个级别："high"（默认）和 "no\_think"（关闭推理）。

我用默认的高推理模式，通过 [OpenRouter](https://openrouter.ai/tencent/hy4-preview#apps) 尝试了我的"生成一只鹈鹕骑自行车的 SVG"提示词，并 [得到了这个结果](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fcb69816b3fb940f2782569a82a523af1)：

引用其推理过程：

> [...] 也许加个头盔？可以增强骑行主题，但可能会遮挡头部。也许一顶小骑行帽或头盔？用户没要求；可以加个红色头盔吗？可能挺可爱的。但鹈鹕嘴那么大；头盔可能会遮挡。还是不加为好。
>
> 也许加副墨镜？不。
>
> 也许加点水？不。

有趣的是，推理过程使用了略显简略的英语，大概是因为对于隐藏的推理文本而言，完美的语法既无必要也不节省 token。

发布于 [2026年8月29日](/2026/Aug/29/) 晚上 11:53
