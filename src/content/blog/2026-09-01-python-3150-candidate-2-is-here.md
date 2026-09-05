---
title: "Python 3.15.0 候选版本 2 已发布！"
description: "Python 3.15.0 候选版本 2 已发布！"
pubDate: "2026-09-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Sep/1/python-315-rc-2"
---

> *摘要：Python 3.15.0 候选版本 2 已发布！
Hugo van Kemenade（Python 3.14 和 3.15 的发布经理）宣布 Python 3.15 的最终候选版本，计划于十月发布：

进入候*

2026 年 9 月 1 日 - 链接 博客

**[Python 3.15.0 候选版本 2 已发布！](https://discuss.python.org/t/python-3-15-0-candidate-2-is-here/108841)**（[来源](https://bsky.app/profile/hugovk.dev/post/3muhjndhw322i "@hugovk.dev")）Hugo van Kemenade（Python 3.14 和 3.15 的发布经理）宣布 Python 3.15 的最终候选版本，计划于十月发布：

> 进入候选版本阶段后，在此候选版本与最终版本之间，仅允许经过审查的、明确的缺陷修复类代码变更。[...]
>
> 我们**强烈建议**第三方 Python 项目的维护者在此阶段为 3.15 做好准备工作，并在 PyPI 上发布 Python 3.15 的 wheel 包，以便为 3.15.0 的最终发布做好准备，同时帮助其他项目进行自身的测试。基于 Python 3.15.0 候选版本构建的任何二进制 wheel 包**都将兼容** Python 3.15 的后续版本。

早在 2021 年，我就通过针对 Python 3.10 运行测试套件[发现了一个 Python 3.10 的缺陷](https://simonwillison.net/2021/Oct/9/finding-and-reporting-a-bug/)……但当时我并没有在 RC 阶段进行此项操作，因此那个缺陷已经随版本发布了！从那以后，我一直更加关注这些候选版本。

新的 RC 版本尚未在 GitHub Actions 上可用——请持续关注 [actions/python-versions](https://github.com/actions/python-versions/releases)。不过目前你可以在测试矩阵中添加以下内容：

```
strategy:
  matrix:
    python-version: ["3.14", "3.15"]

steps:
   - uses: actions/setup-python@v7
    with:
      python-version: ${{ matrix.python-version }}
      allow-prereleases: true
      check-latest: true
```

[allow-prereleases](https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#allow-pre-releases) 和 [check-latest](https://github.com/actions/setup-python/blob/main/docs/advanced-usage.md#check-latest-version) 标志意味着今天它将针对 RC1 进行测试，当 RC2 发布后，它将自动切换到该版本（之后稳定版发布后也会自动切换）。

发布于 [2026 年 9 月 1 日](/2026/Sep/1/) 下午 2:59
