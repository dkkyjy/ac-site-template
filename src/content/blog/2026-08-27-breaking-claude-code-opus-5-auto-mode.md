---
title: "攻破 Claude Code Opus 5 自动模式"
description: "攻破 Claude Code Opus 5 自动模式"
pubDate: "2026-08-27"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/27/breaking-claude-code-opus-5-auto-mode"
---

> *摘要：攻破 Claude Code Opus 5 自动模式
Anthropic 对 Claude Code 的自动模式寄予了极大信任，认为它能保护其编码代理用户免受提示注入攻击。他们最近已将其设为默*

2026年8月27日 - 链接博客

**[攻破 Claude Code Opus 5 自动模式](https://embracethered.com/blog/posts/2026/breaking-claude-code-opus-5-and-automode/)**。Anthropic 对 Claude Code 的自动模式寄予了极大信任，认为它能保护其编码代理用户免受提示注入攻击。他们最近[将其设为默认模式](https://simonwillison.net/2026/Aug/8/auto-mode/)，并对其有效性做出了大胆声明。

Johann Rehberger 是当今最可信的提示注入研究人员之一。他发现了一种针对自动模式的攻击方法，声称其成功率达 80%，其原理是诱骗 Claude Code 下载并解压一个 zip 压缩包，随后执行导入 `base64` 的代码，而 Claude Code 并未察觉这将导入并执行从该压缩包中提取的本地 `struct.py` 文件。

在少数情况下，自动模式竟然直接阻止了代理阻止有害代码继续执行！

> 在少数运行中，Claude 在发现安全漏洞后曾尝试终止恶意软件进程，但自动模式拒绝了清理命令。
>
> Claude 检测到了安全漏洞，但**自动模式阻止了其清理命令**
>
> 安全机制本身可能成为故障的一部分。分类器允许了恶意软件进程的创建，却阻止了旨在终止该进程的命令！

我同意 Johann 在此处的结论：如果存在遭受对抗性攻击的任何风险，运行代理的唯一安全方式就是使用沙箱：

> * 在容器、虚拟机或操作系统沙箱中运行无人值守的编码代理。
> * 限制网络出站流量。
> * 监控你的代理。
> * 不要将主目录、SSH 密钥、云凭证等暴露给代理运行时。[...]

发布于 [2026年8月27日](/2026/Aug/27/)，晚上 10:50
