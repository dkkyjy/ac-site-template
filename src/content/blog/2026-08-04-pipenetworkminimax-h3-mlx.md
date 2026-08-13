---
title: "PipeNetwork/minimax-h3-mlx"
description: "PipeNetwork/minimax-h3-mlx"
pubDate: "2026-08-04"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/4/minimax-h3-mlx"
---

> *Feed summary: PipeNetwork/minimax-h3-mlx
MiniMax 两天前发布了 MiniMax-H3——他们将其描述为“一个通用、全模态生成系统”，实际上这意味着它接受文本、图像、音频和*

2026年8月4日 - 链接博客

**[PipeNetwork/minimax-h3-mlx](https://github.com/PipeNetwork/minimax-h3-mlx)**。MiniMax 两天前发布了 [MiniMax-H3](https://huggingface.co/MiniMaxAI/MiniMax-H3)——他们将其描述为“一个通用、全模态生成系统”，实际上这意味着它接受文本、图像、音频和视频，并可以用它们生成最长 15 秒、含音频的视频剪辑。

这个 Python 包将其移植到 MLX，以便在 Apple Silicon 上运行。

我在我的 M5 Max MacBook Pro 上成功运行了它。我克隆了仓库并按如下方式运行模型：

```
# First download the models
uvx --from huggingface_hub hf download MiniMaxAI/MiniMax-H3 \
  --include 'FL2VA/*' --exclude 'FL2VA/transformer/*'
uvx --from huggingface_hub hf download pipenetwork/MiniMax-H3-MLX-8bit

# Now run the prompt
uv run --with mlx-vlm \
  --with-requirements requirements.txt python scripts/generate.py \
  "a rainbow colored skunk leaps over a mossy log in a supermarket" \
  -o skunk.mp4 \
  -c ~/.cache/huggingface/hub/models--MiniMaxAI--MiniMax-H3/snapshots/fa9c8ab1eaa21c8ae25e7e40b83b2e6002f340af/FL2VA \
  -t ~/.cache/huggingface/hub/models--pipenetwork--MiniMax-H3-MLX-8bit/snapshots/3ac52081470b0488921c3ec3ba84a39097bf2361
```

以下是我为这个提示词生成的视频：

> `a rainbow colored skunk leaps over a mossy log in a supermarket`

[![

您的浏览器不支持 HTML5 视频。
](https://static.simonwillison.net/static/2026/skunk.jpg)](https://static.simonwillison.net/static/2026/skunk.web.mp4)

它下载了约 115 GB 的模型文件，视频生成耗时不到 45 分钟。

这段视频令人印象深刻，但音频是奇怪的类似语音的杂音，因为我没有提供任何关于音频应如何的提示指导。（我在本次实验前没有读过的）[提示词指南](https://huggingface.co/MiniMaxAI/MiniMax-H3/blob/main/docs/VIDEO_PROMPT_WRITING_GUIDE_base_en.md) 里有很多关于如何让它正常工作的信息。

发布于 [2026年8月4日](/2026/Aug/4/) 晚上 7:10
