---
title: "你的可执行文件就是一个 SQLite 数据库"
description: "你的可执行文件就是一个 SQLite 数据库"
pubDate: "2026-08-24"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/24/your-executable-is-a-sqlite-database"
---

> *摘要：你的可执行文件就是一个 SQLite 数据库
Farid Zakaria 介绍了一种巧妙的 Linux 模式，用于创建一个 SQLite 数据库文件，使其能够直接作为可执行二进制文件使用。
该技巧将 SQLite 文件*

2026年8月24日 - 链接博客

**[你的可执行文件就是一个 SQLite 数据库](https://fzakaria.com/2026/08/23/your-executable-is-a-sqlite-database)**（[转引自](https://news.ycombinator.com/item?id=49415271 "Hacker News")）Farid Zakaria 介绍了一种巧妙的 Linux 模式，用于创建一个 SQLite 数据库文件，使其能够直接作为可执行二进制文件使用。

该技巧将 SQLite 文件格式中的 4 字节应用程序 ID（位于文件第 68 字节处）设置为 SELF，即 Structured Executable & Linkable Format（结构化可执行与链接格式）的缩写。随后，ELF 可执行格式的各个组件被分别排列到多个不同的 SQLite 表中，使用[此表结构](https://github.com/fzakaria/selfdb/blob/main/schema/self.sql)。

其 `self-exec` 解释器（[C 代码在此](https://github.com/fzakaria/selfdb/blob/main/loader/self-exec.c)）便可提取并执行所需的各个组件。

你还可以额外使用一种名为 [binfmt\_misc](https://docs.kernel.org/admin-guide/binfmt-misc.html) 的 Linux 机制，教会内核在遇到匹配该二进制模式的文件时自动执行它。Farid 在这里使用的是 NixOS，但如果没有 NixOS，我认为注册方式大致如下：

```
printf '%s\n' ':self:M:68:SELF::/usr/local/bin/self-exec:' \
   > /proc/sys/fs/binfmt_misc/register
```

发布于 [2026年8月24日](/2026/Aug/24/) 上午 11:38
