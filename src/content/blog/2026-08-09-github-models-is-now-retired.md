---
title: "GitHub Models 现已退役"
description: "GitHub Models 现已退役"
pubDate: "2026-08-09"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/9/github-models-is-now-retired"
---

> *摘要：GitHub Models 现已退役。*
我今天才看到这个消息，当时我的 simonw/research 仓库的 GitHub Actions 运行失败，并出现了以下错误信息：

GitHub Models 暂时不可用，作为 *

2026 年 8 月 9 日 - 链接博客

**[GitHub Models 现已退役](https://github.blog/changelog/2026-07-30-github-models-is-now-retired/)**。我今天才看到这个消息，当时我的 [simonw/research](https://github.com/simonw/research) 仓库的 GitHub Actions 运行失败，并出现了以下错误信息：

> GitHub Models 暂时不可用，这是既定退役间歇期的一部分。

这条消息已经过时了，因为退役已经完成。

GitHub Models 是一支形状奇怪的鸭子。GitHub 提供了一个模型游乐场工具，以及一个跨多个不同 LLM 提供商的统一 API，最大的好处是，在 GitHub Actions 中运行的代码可以使用该环境中已有的 GitHub API 密钥来执行提示词。

这使得构建符合 GitHub Next 的 [Continuous AI](https://githubnext.com/projects/continuous-ai/) 概念的东西变得容易。

GitHub 没有透露关闭背后的原因，但我打赌它符合这样一个模式：编码智能体模式使得提供免费或补贴令牌的成本变得高得令人望而却步。

我的工作流会调用一次 LLM，使用[这里的代码](https://github.com/simonw/research/blob/43fa54a74ca2350bb28c2c32fbb16d42c78c442f/README.md?plain=1#L104-L113)为 [README](https://github.com/simonw/research/blob/main/README.md) 创建文件夹摘要。我已将 GitHub Models 换成了带有月度消费限额的 OpenAI API 密钥，现在正在使用 GPT-5.6 Luna 生成摘要。

发布于 [2026 年 8 月 9 日](/2026/Aug/9/) 晚上 10:48
