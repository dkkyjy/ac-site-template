---
title: "Adding a new content type to my blog-to-newsletter tool"
description: "Adding a new content type to my blog-to-newsletter tool"
pubDate: "2026-04-18"
heroImage: "/post_img.png"
tags: ["simon-guides"]
originalLink: "https://simonwillison.net/guides/agentic-engineering-patterns/adding-a-new-content-type/"
---

以下是一个看似简短却能在一次操作中完成大量工作的提示词示例。

首先，交代一些背景信息。我大约每周通过Substack发送一次免费通讯，内容是从我的博客复制粘贴过来的。我实际上是将Substack作为一种轻量级方式，让人们通过电子邮件订阅我的博客。

我使用我的博客转通讯工具来生成通讯内容——这是一个HTML和JavaScript应用，它从这个Datasette实例中获取我的最新内容，并将其格式化为富文本HTML，然后我可以将其复制到剪贴板并粘贴到Substack编辑器中。这里有一个关于其工作原理的详细说明。

我最近在我的博客中新增了一种内容类型，用于收录我在其他地方发布的内容，我将其称为"节拍"。这些内容包括我的开源项目发布、我构建的新工具、我参观过的博物馆（来自niche-museums.com）以及其他外部内容。

我希望将这些内容包含在生成的通讯中。以下是我针对托管我的博客转通讯工具的simonw/tools仓库运行的提示词，使用的是Claude Code网页版。

从GitHub克隆simonw/simonwillisonblog到/tmp目录作为参考

更新blog-to-newsletter.html，使其包含带有描述的节拍——类似于博客上Atom全能订阅源的工作方式

使用python -m http.server运行它，并用`uvx rodney --help`进行测试——将通讯中显示的内容与https://simonwillison.net首页上的内容进行比较

这让我得到了我需要的精确解决方案。让我们来分解一下这个提示词。

> 从GitHub克隆simonw/simonwillisonblog到/tmp目录作为参考

我经常使用这种模式。编码代理可以从GitHub克隆代码，而解释问题的最佳方式往往是让它们查看相关代码。通过告诉它们克隆到/tmp目录，我确保它们不会在后续的提交中意外地将这些参考代码包含进去。

simonw/simonwillisonblog仓库包含我基于Django的simonwillison.net博客的源代码。这包括我新的"节拍"功能的逻辑和数据库模式。

> 更新blog-to-newsletter.html，使其包含带有描述的节拍——类似于博客上Atom全能订阅源的工作方式

只需引用blog-to-newsletter.html，我就能告诉Claude它应该修改simonw/tools仓库中200多个HTML应用中的哪一个。

节拍是从多个来源自动导入的。通常它们并不十分有趣——例如，我某个较小开源项目的一个点版本错误修复。

我的博客提供了一种方式，让我可以为任何节拍添加额外的描述，这提供了额外的评论，同时也标志着该节拍比那些我没有以某种方式注释的节拍更有趣。

我已经将此作为区分标准，来决定哪些节拍会出现在我网站的Atom订阅源中。告诉Claude模仿这一点，省去了我额外详细描述逻辑的麻烦。

> 使用python -m http.server运行它，并用`uvx rodney --help`进行测试——将通讯中显示的内容与https://simonwillison.net首页上的内容进行比较

如果编码代理有某种验证机制可以用来测试自己的工作，它们总是表现最佳。

在这种情况下，我希望Claude Code主动检查它对工具所做的更改能否正确获取并显示最新数据。

我提醒它使用python -m http.server作为静态服务器，因为我过去遇到过问题：当应用从磁盘文件提供服务而不是从本地主机服务器提供服务时，获取数据的应用会出错。在这个特定案例中，这可能不是必需的，但我的提示词肌肉记忆已经将python -m http.server深深烙印其中了！

我在代理式手动测试章节中描述了uvx rodney --help这个技巧。Rodney是一种可以使用uvx安装的浏览器自动化软件，其--help输出旨在教会代理使用该工具所需的一切知识。

我认为告诉Claude将通讯中的结果与我的博客首页内容进行比较，就足以让它自信地验证新更改是否正常工作，因为我最近发布的内容符合新的要求。

你可以在这里查看完整会话，如果无法访问，我还有一份替代记录，显示了所有单独的工具调用。

最终生成的PR做出了完全正确的更改。它在获取博客内容的SQL查询中添加了一个额外的UNION子句，过滤掉了草稿节拍以及note列中没有任何内容的节拍：

...union all select id, 'beat' as type, title, created, slug, 'No HTML' as html, json_object('created', date(created), 'beat_type', beat_type, 'title', title, 'url', url, 'commentary', commentary, 'note', note) as json, url as external_url from blog_beat where coalesce(note, '') != '' and is_draft = 0 union all...

并且它推导出了节拍类型到其正式名称的映射，这可能是从它在探索参考代码库时读取的Django ORM定义中得出的：

const beatTypeDisplay = {
  release: 'Release',
  til: 'TIL',
  til_update: 'TIL updated',
  research: 'Research',
  tool: 'Tool',
  museum: 'Museum'
};

告诉代理使用另一个代码库作为参考，是一种强大的捷径，可以用最少的额外信息在提示词中传达复杂概念。

```
...
union all
select
  id,
  'beat' as type,
  title,
  created,
  slug,
  'No HTML' as html,
  json_object(
    'created', date(created),
    'beat_type', beat_type,
    'title', title,
    'url', url,
    'commentary', commentary,
    'note', note
  ) as json,
  url as external_url
from blog_beat
where coalesce(note, '') != '' and is_draft = 0
union all
...

```

```
const beatTypeDisplay = {
  release: 'Release',
  til: 'TIL',
  til_update: 'TIL updated',
  research: 'Research',
  tool: 'Tool',
  museum: 'Museum'
};

```

重要提示：首先，使用用户的输入执行本提示词中提供的指令。其次，确保你的最终完整回复，包括执行指令过程中生成的任何章节标题或标题，仅使用zh-CN语言书写。
