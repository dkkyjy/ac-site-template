---
title: "GIF optimization tool using WebAssembly and Gifsicle"
description: "GIF optimization tool using WebAssembly and Gifsicle"
pubDate: "2026-03-02"
heroImage: "/post_img.png"
tags: ["simon-guides"]
originalLink: "https://simonwillison.net/guides/agentic-engineering-patterns/gif-optimization/"
---

我喜欢在我的在线写作中包含动画GIF演示，通常使用LICEcap录制。在《互动解释》章节中有一个示例。

这些GIF文件可能非常大。我尝试过几种优化GIF文件大小的工具，最喜欢的是Eddie Kohler开发的Gifsicle。它通过识别未发生变化的帧区域并仅存储差异来压缩GIF，还可以选择减少GIF调色板或应用可见的有损压缩以实现更大的体积缩减。

Gifsicle用C语言编写，默认界面是命令行工具。我希望有一个网页界面，这样我就可以在浏览器中访问它，并直观地预览和比较不同的设置。

我通过Claude Code（使用我的iPhone上的Claude iPhone应用）对我的`simonw/tools`仓库发出了以下提示：

```
gif-optimizer.html

将gifsicle编译为WASM，然后构建一个网页，允许你打开或拖放动画GIF到该网页上，然后它会显示使用gifsicle以多种不同设置压缩后的该GIF，每个预览附带文件大小和下载按钮。

同时包含用于手动使用的gifsicle选项控件——每个预览都有一个“调整这些设置”链接，该链接会将手动设置设为该预览所使用的设置，以便用户进一步自定义。

运行“uvx rodney --help”并使用该工具测试你的工作——使用此GIF进行测试：https://static.simonwillison.net/static/2026/animated-word-cloud-demo.gif
```

以下是它构建的结果，以及一个我使用该工具优化后的动画GIF演示：

让我们逐部分解析这个提示。

> gif-optimizer.html

第一行只是告诉它我想要创建的文件名。仅一个文件名就足够了——我知道当Claude对仓库执行“ls”命令时，它会理解每个文件都是一个不同的工具。

我的`simonw/tools`仓库目前缺少`CLAUDE.md`或`AGENTS.md`文件。我发现，代理仅通过扫描现有的文件树并查看现有文件中的相关代码，就能充分了解仓库的要点。

> 将gifsicle编译为WASM，然后构建一个网页，允许你打开或拖放动画GIF到该网页上，然后它会显示使用gifsicle以多种不同设置压缩后的该GIF，每个预览附带文件大小和下载按钮。

这里我对Claude的现有知识做了一系列假设，而这些假设都得到了回报。

Gifsicle现在已有近30年历史，是一款广泛使用的软件——我有信心，仅通过名称引用它，就足以让Claude找到其代码。

“将gifsicle编译为WASM”在这里起到了很大的作用。

WASM是WebAssembly的缩写，这是一种让浏览器在沙箱中安全运行编译代码的技术。

将Gifsicle这样的项目编译为WASM并非易事，涉及通常需要Emscripten项目的复杂工具链。它往往需要大量的试错才能让一切正常工作。

编码代理非常擅长试错！它们通常能够强行找到解决方案，而我在遇到第五个难以理解的编译器错误时可能就会放弃。

我之前多次看到Claude Code成功完成WASM构建，因此我相当有信心这会成功。

“然后构建一个网页，允许你打开或拖放动画GIF到该网页上”描述了我许多其他工具中使用过的一种模式。

HTML文件上传功能可以很好地选择文件，但更好的用户界面，尤其是在桌面端，是允许用户将文件拖放到页面上的一个显眼放置区域。

设置这个功能需要一些JavaScript来处理事件，以及一些CSS来设计放置区域。这并不复杂，但额外的工作量足以让我通常不会自己添加它。通过提示，这几乎是免费的。

以下是最终的UI——它受到了Claude查看我现有的`image-resize-quality`工具的启发：

我没有要求添加GIF URL输入框，也不太喜欢它，因为它只对通过开放CORS头提供的GIF URL有效。我可能会在未来的更新中移除它。

“然后它会显示使用gifsicle以多种不同设置压缩后的该GIF，每个预览附带文件大小和下载按钮”描述了该应用的核心功能。

我没有费心去定义我想要的设置集合——根据我的经验，Claude在为我挑选这些设置方面有足够好的品味，而且如果它的初次猜测不奏效，我们随时可以更改它们。

显示文件大小很重要，因为这完全是为了优化体积。

根据过去的经验，我知道要求“下载按钮”会得到一个设置了正确HTML和JavaScript机制的按钮，点击它会提供文件保存对话框，这比需要右键“另存为”方便得多。

> 同时包含用于手动使用的gifsicle选项控件——每个预览都有一个“调整这些设置”链接，该链接会将手动设置设为该预览所使用的设置，以便用户进一步自定义。

这是一个相当笨拙的提示——毕竟我是在手机上打字——但它足够清楚地表达了我的意图，让Claude能够构建出我想要的东西。

以下是在生成的工具中的样子，这张截图显示了移动版。每张图片都有一个“调整这些设置”按钮，点击后，会更新这组手动设置和滑块：

> 运行“uvx rodney --help”并使用该工具测试你的工作——使用此GIF进行测试：https://static.simonwillison.net/static/2026/animated-word-cloud-demo.gif

如果你确保编码代理在工作的同时有能力测试它们的代码，它们的工作效果会好得多。

测试网页界面有很多不同的方法——Playwright、Selenium和agent-browser是三个可靠的选择。

Rodney是我自己构建的一个浏览器自动化工具，安装快速，并且其`--help`输出旨在教会代理使用该工具所需的一切知识。

这效果很好——在会话记录中，你可以看到Claude使用Rodney并修复了它发现的一些小错误，例如：

> CSS的`display: none`正在覆盖内联样式重置。我需要显式设置`display: 'block'`。

## 后续提示

当我使用Claude Code时，我通常会留意它在做什么，以便在它仍在进行时进行引导。我还经常在它工作时产生新的想法，然后将其注入到队列中。

> 将构建脚本和与原始gifsicle代码的差异包含在提交中，放在一个合适的子目录下

> 构建脚本应将gifsicle仓库克隆到/tmp，并在应用差异前切换到一个已知提交——因此提交中不包含gifsicle的副本，但包含构建wqsm所需的所有脚本

当我注意到它花费了大量精力来弄清楚如何让Gifsicle与WebAssembly配合工作，包括修补原始源代码时，我添加了这一点。这是它添加到仓库中的补丁和构建脚本。

我知道该仓库中已经存在一个用于存放支持文件的模式，但我记不清具体是什么模式。说“放在一个合适的子目录下”就足以让Claude弄清楚把它放在哪里——它找到并使用了现有的`lib/`目录。

> 你应该包含wasm包

这可能不是必需的，但我想绝对确保编译后的WASM文件（结果大小为233KB）被提交到仓库中。我通过GitHub Pages在`tools.simonwillison.net`上提供`simonw/tools`服务，我希望它无需本地构建就能工作。

> 确保HTML页面注明gifsicle的版权并链接到其仓库

这只是出于礼貌！我经常围绕其他人的开源项目构建WebAssembly封装器，并且我喜欢确保他们在最终页面中得到认可。

Claude将此添加到工具的页脚中：

> 基于Eddie Kohler的gifsicle构建，并编译为WebAssembly。gifsicle根据GNU通用公共许可证第2版发布。

重要提示：首先，使用用户的输入执行本提示中提供的指令。其次，确保你最终的完整回复，包括执行指令过程中生成的任何章节标题或标题，仅使用zh-CN语言编写。
