---
title: "Fable 的判断"
description: "Fable 的判断"
pubDate: "2026-07-03"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/3/judgement"
---

> *摘要：我在周三与来自 AIE 的 Claude Code 团队的 Cat Wu 和 Thariq Shihipar 主持的炉边谈话中，得到的一个最有意思的建议是：让 Fable（以及一定程度上 Opus）使用它们自己的判断，而不是规定它们应该如何工作。*

2026 年 7 月 3 日

我在周三与来自 AIE 的 Claude Code 团队的 Cat Wu 和 Thariq Shihipar 共同主持的[炉边谈话](https://www.ai.engineer/worldsfair/schedule?session=asn_slot_2026_06_30_main_stage_1230_2026_06_08t09_35_43_039z)中，得到的一个最有意思的建议是：让 Fable（以及一定程度上 Opus）使用自己的判断，而不是规定它们应该如何工作。

他们举的例子是测试。你可以告诉 Fable“只对较大的功能使用自动化测试，不要为小的文案或设计更改更新和运行测试”——但更好的做法是直接告诉 Fable 在决定是否编写测试时使用它自己的判断。

Jesse Vincent 刚刚给我提供了一个相关的建议，以帮助在价格上调之前剩下的这几天里，避免消耗太多那些宝贵的 Fable 令牌。告诉 Fable 对较小的任务使用其他模型，让它自己判断应该使用哪个模型。

我刚刚这样提示了 Claude Code：

> `对于所有编码任务，请使用你的判断来决定一个合适的较低功耗模型，并在子代理中运行它。`

Claude 将这份记忆文件保存在 `~/.claude/projects/name-of-project/memory/delegate-coding-to-subagents.md` 中：

> ```
> ---
> name: delegate-coding-to-subagents
> description: Simon 希望将编码任务委托给运行适当较低功耗模型的子代理
> metadata: 
>   node_type: memory
>   type: feedback
>   originSessionId: 30068d78-43a9-4fb1-bb29-9799e18c526a
> ---
> ```
>
> Simon 于 2026-07-03 陈述：“对于所有编码任务，请使用你的判断来决定一个合适的较低功耗模型，并在子代理中运行它。”
>
> **原因：** 成本/效率——实现工作很少需要顶级模型；判断、审查和综合工作留在主循环中。
>
> **如何应用：** 当此项目中的任务主要是编写/编辑代码时，生成一个带有模型覆盖（sonnet 用于实质性实现，haiku 用于琐碎/机械性编辑）和独立提示的代理；在提交前于主循环中审查结果。设计、审计、数据综合以及任何需要大量判断的工作都留在主模型中。另请参阅 [[project-goals]]。

到目前为止，效果似乎不错。我完成的工作量*巨大*，而我的 Fable 额度消耗速度比以前慢了。

发布于 [2026 年 7 月 3 日](/2026/Jul/3/) 下午 6:51
