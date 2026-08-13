---
title: "让您的智能体用 shot-scraper video 录制作品视频演示"
description: "让您的智能体用 shot-scraper video 录制作品视频演示"
pubDate: "2026-06-30"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jun/30/shot-scraper-video"
---

## 让您的智能体用 shot-scraper video 录制作品视频演示

2026年6月30日

[shot-scraper video](https://shot-scraper.datasette.io/en/stable/video.html) 是今天发布的 [shot-scraper 1.10](https://github.com/simonw/shot-scraper/releases/tag/1.10) 中引入的一个新命令，它接受一个 `storyboard.yml` 文件，定义对 Web 应用程序执行的一系列操作，并使用 Playwright 录制该操作过程的视频。我以前写过关于[让编码智能体生成其作品的演示](https://simonwillison.net/2026/Feb/10/showboat-and-rodney/#proving-code-actually-works)的重要性；这是我实现这一目标的最新尝试。

以下是一个使用 `shot-scraper video` 创建的示例视频，演示了 Datasette 中一个[仍在开发中](https://github.com/simonw/datasette/pull/2813)的功能——通过粘贴 CSV、TSV 或 JSON 数据来创建新表：

[![

](https://static.simonwillison.net/static/2026/datasette-bulk-insert-demo.jpg)](https://static.simonwillison.net/static/2026/datasette-bulk-insert-demo.mp4)

该视频是通过运行以下命令创建的：

```
shot-scraper video datasette-bulk-insert-storyboard.yml \
  --auth datasette-demo-auth.json --mp4
```

（那个 `--auth` JSON 文件[包含一个 cookie](https://gist.github.com/simonw/287b26aff53fcb72942b19f5b69d7e5c)，如文档中[所述](https://shot-scraper.datasette.io/en/stable/authentication.html)。）

以下是 `datasette-bulk-insert-storyboard.yml` 文件：

```
output: /tmp/datasette-bulk-insert-demo.webm
server:
  - uv
  - --directory
  - /Users/simon/Dropbox/dev/datasette
  - run
  - datasette
  - -p
  - 6419
  - --root
  - --secret
  - "1"
  - /tmp/demo.db
url: http://127.0.0.1:6419/demo/tasks
viewport:
  width: 1280
  height: 720
cursor: true
wait_for: 'button[data-table-action="insert-row"]'
javascript: |
  (() => {
    let clipboardText = "";
    Object.defineProperty(navigator, "clipboard", {
      configurable: true,
      get: () => ({
        writeText: async (text) => {
          clipboardText = String(text);
        },
        readText: async () => clipboardText,
      }),
    });
  })();
scenes:
  - name: 批量插入现有表格行
    do:
      - pause: 0.8
      - click: 'button[data-table-action="insert-row"]'
      - wait_for: "#row-edit-dialog[open]"
      - pause: 0.5
      - click: ".row-edit-bulk-insert"
      - wait_for: ".row-edit-bulk-textarea"
      - pause: 0.5
      - click: ".row-edit-copy-template"
      - wait_for: "text=Copied"
      - pause: 0.8
      - fill:
          into: ".row-edit-bulk-textarea"
          text: |
            title,owner,status,priority,notes
            Prepare release video,Ana,doing,1,Recorded with shot-scraper
            Check pasted CSV import,Ben,review,3,Previewed before inserting
            Share the branch demo,Chen,queued,2,Bulk insert creates three rows
      - pause: 0.8
      - click: ".row-edit-save"
      - wait_for: "text=Previewing 3 rows."
      - pause: 1.2
      - click: ".row-edit-save"
      - wait_for: "text=3 rows inserted."
      - pause: 1.0
      - click: ".row-edit-cancel"
      - wait_for: "text=Prepare release video"
      - pause: 1.0
  - name: 从粘贴的 CSV 创建表格
    open: http://127.0.0.1:6419/demo
    wait_for: 'details.actions-menu-links summary'
    do:
      - pause: 0.8
      - click: 'details.actions-menu-links summary'
      - click: 'button[data-database-action="create-table"]'
      - wait_for: "#table-create-dialog[open]"
      - pause: 0.5
      - fill:
          into: ".table-create-table-name"
          text: "launch_metrics"
      - click: ".table-create-from-data"
      - wait_for: ".table-create-data-textarea"
      - pause: 0.5
      - fill:
          into: ".table-create-data-textarea"
          text: |
            metric_id,name,score,recorded_on
            m001,Activation rate,87.5,2026-06-29
            m002,Retention check,72.25,2026-06-30
            m003,CSV import health,95,2026-07-01
      - pause: 0.8
      - click: ".table-create-save"
      - wait_for: "text=Previewing 3 rows."
      - pause: 1.2
      - click: ".table-create-save"
      - wait_for_url: "**/demo/launch_metrics"
      - wait_for: "text=Activation rate"
      - pause: 1.2
```

[video 命令文档](https://shot-scraper.datasette.io/en/stable/video.html)中包含更简单的示例，但为了这篇博文的目的，我选择了一个更全面的示例。

那个演示 YAML 故事板完全由运行在 Codex Desktop 中的 GPT-5.5 xhigh 构建，使用的提示词在我的 `~/dev/datasette` 仓库（[该分支](https://github.com/simonw/datasette/commits/b759ea548606bc9bf9a4bf0e33e2d57ead7e0ab8/)的检出）中运行如下：

> `Review the changes on this branch.`
>
> `cd to ~/dev/shot-scraper and run the command "uv run shot-scraper video --help"`
>
> `Now use that new video command to record a video demo of the new features from this branch, including running a "uv run datasette -p 6419 --root --secret 1 /tmp/demo.db" development server so you can record the video against a demo DB that you first create.`

现在我已经发布了这个功能，提示词可以改成 "`run uvx shot-scraper video --help`" 也能达到同样的效果。

我非常喜欢这种模式：命令的 `--help` 输出提供了足够的细节，让编码智能体能够使用它——这有点像把 `SKILL.md` 文件直接打包在工具里。我在 [showboat 和 rodney](https://simonwillison.net/2026/Feb/10/showboat-and-rodney/) 中也使用了同样的模式。

#### 我是如何构建它的

`shot-scraper video` 最初是一个实验性原型。`shot-scraper` 基于 [Playwright](https://playwright.dev/) 构建，它需要的核心功能是让 Playwright 能够录制浏览器会话的视频，并具有足够的控制力来创建所需的演示。

我几年前第一次尝试这个时，发现 Playwright 生成的视频包含额外的界面元素，这些元素对调试测试失败很有用，但对于产品演示来说却不理想。

他们在一段时间前修复了这个问题，但仍有一些小障碍。特别是我在视频开头得到了[一些白色帧](https://github.com/simonw/shot-scraper/pull/194/changes/c2f3b3a52ba84f2adcf3ad6da4d39c2570328584#issuecomment-4724459369)，因为录制机制在浏览器加载第一个 URL 之前就启动了。

Playwright 1.59 添加了一个新的[屏幕录制机制](https://playwright.dev/python/docs/api/class-screencast)，提供了更精细的视频录制控制。这几乎就是我需要的，但生成的视频固定为 800 像素宽。

我发现了一个[已经合并的修复该问题的 PR](https://github.com/microsoft/playwright/pull/41183)，但它还没有发布。然后昨天他们在 [playwright-python 1.61.0](https://github.com/microsoft/playwright-python/releases/tag/v1.61.0) 中发布了它，我终于可以不受阻碍地完成这个功能了！

代码本身全部由 Codex Desktop 中的 GPT-5.5 xhigh 编写。我还让它编写了文档，这为我审查设计提供了一个非常有用的框架——该功能的大部分迭代都来自于审查这些文档，发现冗余、不一致或令人困惑的地方，然后请求（或指示）进行更好的设计。

YAML 格式本身大部分是由编码智能体定义的。我让它[使用 Pydantic](https://github.com/simonw/shot-scraper/blob/1.10/shot_scraper/video.py#L24) 来定义和验证格式，部分原因是为了让设计更容易审查。

这是一个很好的例子，说明如果没有编码智能体的支持，我几乎肯定不会承担这种功能。我在 2024 年 2 月提交了[原始问题](https://github.com/simonw/shot-scraper/issues/142)，并且很难在众多其他项目中找到必要的时间来解决这个问题。

发布于 [2026年6月30日](/2026/Jun/30/) 下午4:54 · 在 [Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw) 上关注我，或[订阅我的新闻通讯](https://simonwillison.net/about/#subscribe)
