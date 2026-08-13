---
title: "SQLite 压缩文本历史原型"
description: "SQLite 压缩文本历史原型"
pubDate: "2026-08-09"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/9/sqlite-text-history-prototype"
---

> *摘要：研究：SQLite 压缩文本历史原型
        我一直对在关系数据库中存储修订历史记录的选项感兴趣。在遛狗的时候，我有了一个新想法：如何 *

2026年8月9日

[研究](/elsewhere/research/)
[SQLite 压缩文本历史原型](https://github.com/simonw/research/tree/main/sqlite-text-history-prototype#readme)
— SQLite 压缩文本历史原型将 `WholeBlobHistoryStore`（每次编辑时重写一个压缩的历史 blob）与 `ChunkedHistoryStore`（密封压缩块以改善长历史的可扩展性）进行比较。两者都保留先前的文本和时间戳，默认跳过未更改的替换，并使用 `BEGIN IMMEDIATE` 序列化写入者以实现原子更新。

我一直对在关系数据库中存储修订历史记录的选项非常感兴趣。在遛狗的时候，我有了一个新想法：把每个先前版本的全文放入一个大的 JSON 字符串数组中，然后对整个数组应用 zlib 或 zstd 压缩，怎么样？由于所有重复的字符串，这样肯定能压缩得很好。

ChatGPT iPhone 应用中的新版 [GPT‑Live 语音模式](https://openai.com/index/introducing-gpt-live/) 已经变得非常好，所以我和它讨论了原型。你仍然无法分享语音对话的 URL，但以下是我从转录中复制的内容，作为一种真正的意识流：

> 我有一个有趣的想法，关于一种方案，用于尽可能高效地保存一段不断编辑的文本的所有先前版本，放在 SQLite 数据库的嗯，列中。好吧，我以前构建过这类系统，而且总是很难想出有效的方法来做这件事。比如，最简单的方式是，对于字符串的每一个先前副本，也就是之前之前的值，你都有一行。但如果是一份长文档，比如 20 千字节的数据，那就意味着每次编辑都会往数据库里再增加 20 千字节的数据，对吧。所以，我现在在想的是，嗯，压缩会非常有效，对吧？如果你把所有那些不同的，嗯，每一个每一个版本的这个文档，从最早开始的所有版本，都捆绑在一起，然后对它们应用一个好的压缩算法，那基本上应该会消除大量的冗余——嗯，冗余文本，对吧。嗯，所以我在想的是，一个非常非常简单的机制如何？在这个表上，嗯，嗯，有一个历史列，它是一个 blob，是个 BLOB，所以它存储二进制数据，然后你只需放入一个 Zlib 或者甚至 ZSTD 嗯 压缩过的 JSON 文本数组，里面包含所有之前的文档，那么你可能会有两列，对吧？你会有一列是这个神奇的 JSON 文本数组，你还有第二列，是 JSON 时间戳数组，那个根本不需要压缩，对吧？时间戳可以是，呃——它是一个整数数组，对吧？Unix 整数。但这就是整个方案。

然后我停止了语音模式，并向 GPT-5.6 Sol Pro 输入了以下文本提示：

> `使用 Python 并围绕这个想法构建实验原型`

它运行了 38 分钟，并给出了[这个答案](https://gist.github.com/simonw/4e255c53aebdb610553d02cdce17ac30)以及你在[这个文件夹](https://github.com/simonw/research/tree/main/sqlite-text-history-prototype)中看到的文件。

这种方法效果非常好！对一份文档进行 1,000 次模拟修订，产生了 20.4 MB 的原始修订文本，经 Zstandard 压缩后成为 80.3 KB 的 JSON 数组。

为了避免在每次编辑时对整个数组进行解压和重新压缩的开销，Sol 建议将历史记录拆分为多行，每行最多包含 128 次修订或 3MB 未压缩的 JSON。

发布于 [2026年8月9日](/2026/Aug/9/) 晚上 10:05
