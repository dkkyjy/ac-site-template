---
title: "Claude Code 现在使用了用 Rust 编写的 Bun"
description: "Claude Code 现在使用了用 Rust 编写的 Bun"
pubDate: "2026-07-19"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/19/claude-code-in-bun-in-rust"
---

> *提要：在《用 Rust 重写 Bun》中，Jarred Sumner 声称：

Claude Code v2.1.181（6 月 17 日发布）及之后版本使用了 Bun 的 Rust 移植版。启动速度在 Linux 上提升了 10%，但除此之外几乎没什么人注意到。无聊是好事。*

2026 年 7 月 19 日

在 [用 Rust 重写 Bun](https://bun.com/blog/bun-in-rust) 一文中，Jarred Sumner 声称：

> Claude Code v2.1.181（6 月 17 日发布）及之后版本使用了 Bun 的 Rust 移植版。启动速度在 Linux 上提升了 10%，但除此之外几乎没什么人注意到。无聊是好事。

我决定检查一下我自己的 Claude Code 安装，看看能否找到它正在使用用 Rust 编写的 Bun 的证据。

我发现了下面这两个命令很有说服力：

```
strings ~/.local/bin/claude | grep -m1 'Bun v1'
```

对于我来说，这条命令输出的是 `Bun v1.4.0 (macOS arm64)`。目前 [GitHub 上 Bun 的最新版本](https://github.com/oven-sh/bun/releases) 是 [v1.3.14](https://github.com/oven-sh/bun/releases/tag/bun-v1.3.14)，发布于 5 月 12 日，所以 Claude 中这个 v1.4.0 版本号支持了他们正在提供尚未发布的 Bun 预览版的说法。

（**更新**：Rust 版本已作为 [Bun canary 版](https://bun.com/docs/installation#canary-builds) 发布——运行 `bun upgrade --canary` 将安装 [此版本](https://github.com/oven-sh/bun/releases/tag/canary)。）

```
strings ~/.local/bin/claude | grep -Eo 'src/[[:alnum:]_./-]+\.rs'
```

这条命令输出了一份包含 [563 个文件名](https://gist.github.com/simonw/c92fb0f67b114ac26e3b95a09ddccfdc) 的列表，开头几个是：

```
src/runtime/bake/dev_server/mod.rs
src/runtime/bake/production.rs
src/bundler/bundle_v2.rs
```

看起来，用 Rust 编写的 Bun 确实正在数百万台不同的设备上投入生产运行。正如 Jarred 所说：“无聊是好事”。

**更新**：这里有一个来自 [Ajan Raj](https://twitter.com/ajanraj25/status/2078825794701242697) 的巧妙技巧：

```
cat > /tmp/bun-version.ts <<'EOF'
console.log("embedded bun:", Bun.version);
process.exit(0);
EOF
BUN_OPTIONS="--preload=/tmp/bun-version.ts" claude --version
```

对于我来说，这条命令输出的是 `1.4.0`。

这是 [5 月 17 日的提交](https://github.com/oven-sh/bun/commit/b18bf6d1d0a92238f240bfd125f0e3b3461b9243#diff-7ae45ad102eab3b6d7e7896acd08c427a9b25b346470d7bc6507b6481575d519)，它将 `package.json` 中的版本更新为 1.4.0。自那以后该版本号未再更改，但也尚未出现在除 `canary` 之外的任何标签发布中。

发表于 [2026 年 7 月 19 日](/2026/Jul/19/) 凌晨 3:54
