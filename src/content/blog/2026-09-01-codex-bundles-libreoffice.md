---
title: "Codex 捆绑了 LibreOffice"
description: "Codex 捆绑了 LibreOffice"
pubDate: "2026-09-01"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Sep/1/codex-libreoffice"
---

> *摘要：我在用 OmniDiskSweeper 翻找 `~/.cache/` 文件夹时，发现了个有意思的东西。OpenAI Codex 桌面应用（现已直接[更名](https://help.openai.com/en/articles/20001276-moving-to-the-new-chatgpt-desktop-app)为 ChatGPT）在一个叫 `codex-primary-runtime` 的文件夹里塞了 1.7GB 的东*

2026年9月1日

我在用 [OmniDiskSweeper](https://www.omnigroup.com/more) 翻找 `~/.cache/` 文件夹时，发现了个有意思的东西。OpenAI Codex 桌面应用（现已直接[更名](https://help.openai.com/en/articles/20001276-moving-to-the-new-chatgpt-desktop-app)为 ChatGPT）在一个叫 `codex-primary-runtime` 的文件夹里塞了 1.7GB 的东西，包括一套完整的 Python 安装、一套完整的 Node.js 安装，以及 [Poppler](https://poppler.freedesktop.org)、git 和开源办公套件 [LibreOffice](https://en.wikipedia.org/wiki/LibreOffice)（2010 年从 OpenOffice.org 分支而来）的原生二进制文件：

`~/.cache/codex-runtimes/codex-primary-runtime/plugins/openai-primary-runtime/plugins/documents` 文件夹中包含了一些技能文件，用于告知 Codex 如何查找和使用这些二进制文件。

发布于 [2026年9月1日](/2026/Sep/1/) 下午 7:03
