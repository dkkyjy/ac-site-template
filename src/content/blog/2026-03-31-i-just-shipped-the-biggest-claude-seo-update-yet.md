---
title: "我刚刚发布了有史以来最大的 Claude SEO 更新"
description: "---"
pubDate: "2026-03-31"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-seo-172-firecrawl-backlink-analysis"
---

# 我刚刚发布了有史以来最大的 Claude SEO 更新

---

## 3500 颗星，还在增长
一个月前我发布了 [claude-seo](https://github.com/AgriciDaniel/claude-seo)，它达到 3500 颗 GitHub 星标的速度比我之前做的任何项目都快。560 个 fork。每个 issue 和 PR 我都亲自审核过。人们真的在用——用它审计自己的网站，遇到问题时提交 bug，提出我没想到的功能建议。
从那以后，我一直埋头推版本。不做营销，不写推文，不发发布帖。只是构建。今天 **v1.7.2 已上线**，这个项目与我最初发布时已经完全不同。最初只是 [每月 300 美元的 SEO 工具替代品](/blog/claude-code-seo-stack)，现在变得更大——一个可扩展的平台，连接到实时数据源，并生成实际上可以交给客户的报告。
让我带你看看改变了什么以及为什么。

## 从 12 个技能到 19 个子技能
我发布的 v1.0 有 12 个技能和 9 个并行代理。它可以审计你网站的技术 SEO、检查你的结构化数据、分析内容质量，并给你一份按优先级排序的修复清单。这确实有用——人们用它替代付费工具。
但反馈是一致的：“审计很好，但我需要实时数据。”这个工具是在分析静态 HTML，而不是查询真实的 API。它无法告诉你来自现场的真正核心网页指标、真实的 Search Console 展示次数或你的实时反向链接配置文件。它只是在做开发者通过阅读源代码能做的事——只是更快。
v1.7.2 解决了这个问题。该项目现在有 **19 个子技能**（16 个核心 + 3 个扩展）、**12 个并行子代理**和 **3 个扩展模块**，它们连接到实时数据。下面是增长的内容及原因。

## Google API 集成——真实数据，不是猜测
这是转折点。我将 [9 个 Google API](/blog/google-api-seo-automation-claude-code) 直接集成到 claude-seo 中：PageSpeed Insights、Chrome 用户体验报告（CrUX）、Search Console、Google Analytics 4、YouTube Data API、Cloud Natural Language API、Indexing API、Google Trends 和 Custom Search。在 Google 的免费层级上，每个查询只需 **0.0006 美元**。
凭证系统有 4 个层级，因此你可以从免费开始，然后扩展：
1. **仅 API 密钥**——PageSpeed、CrUX、YouTube、NLP、Trends、Custom Search。无需 OAuth。只需粘贴一个密钥。
2. **OAuth（只读）**——新增 Search Console 性能数据和 GA4 报告。
3. **OAuth（写入）**——新增 Indexing API，用于直接将 URL 提交给 Google。
4. **服务账号**——用于自动化管道和 CI/CD 集成。
代理商老板最关心的部分：**PDF 和 Excel 报告**。运行一次审计，将 Search Console 数据通过报告生成器处理，就能得到一份带有摘要指标、每页表现、查询排名和索引状态的样式化文档。Excel 版本有海军蓝标题、自动列宽、冻结行和筛选器。交给客户时，看起来就像你在 Google 表格里花了一小时。

## DataForSEO 扩展——实时 SERP 数据和关键词研究
Google 的 API 给你你自己的数据。[DataForSEO](https://dataforseo.com/) 则给你其他人的数据。DataForSEO 扩展通过 MCP 服务器集成为 claude-seo 增加了 **22 条命令**：
* **实时 SERP 分析**——提取任何关键词的实际前 10 名结果，包括标题、描述、精选摘要和“人们也问”框
* **关键词研究**——任何词的搜索量、关键词难度、CPC、竞争程度和搜索意图分类
* **反向链接配置文件**——引用域、锚文本分布、新增/丢失链接以及域名权威度指标
* **页面内分析**——内容解析、可读性评分以及来自他们基础设施的 Lighthouse 数据
* **内容分析**——大规模的情感、实体和主题提取
我为这个构建的扩展模式让我很自豪。一个安装脚本，一个 MCP 服务器配置，搞定。无需手动编辑 JSON，无需调试路径。运行 `./extensions/dataforseo/install.sh`，输入你的 API 凭证，下次运行 `/seo` 时扩展就自动可用。我对 Firecrawl 和 Banana（AI 图像生成）也使用了相同的模式。

## Firecrawl 扩展——最被要求的功能
“它能爬取我的整个网站吗？”这是我收到最多的问题。诚实的回答是：不太行。技术审计代理解析你的 sitemap 并检查单个 URL，但如果你的网站是用 React、Next.js、Vue 或任何 SPA 框架构建的，爬虫只会看到空的 `<div id="root"></div>`，其他什么都没有。
[Firecrawl](https://www.firecrawl.dev/) 彻底改变了这一点。它是一个带有完整 JavaScript 执行的爬虫服务，claude-seo 集成暴露了四条命令：
* **`/seo firecrawl crawl <url>`**——全站爬取，支持 JS 渲染。每个页面在内容提取前都会在真实浏览器中执行。
* **`/seo firecrawl map <url>`**——快速发现整个网站的 URL。对于 sitemap 审计来说积分利用效率高。
* **`/seo firecrawl scrape <url>`**——单页面深度抓取，获取完全渲染的 DOM。
* **`/seo firecrawl search <query> <url>`**——在网站已爬取的内容中搜索。
这种集成不仅仅是独立的命令。当安装了 Firecrawl 后，审计工作流会自动升级——URL 发现从仅 sitemap 切换到 `map`，断链检测使用 `crawl` 来处理 JavaScript 渲染的页面。免费层：**每月 500 积分**（每页 1 积分）。

## 反向链接分析
新的 `/seo backlinks <url>` 命令进行 **7 个部分的**分析：
1. **概况概览**——总反向链接、引用域、权威度趋势
2. **锚文本分布**——过度优化检测（当 >70% 是精确匹配时发出警告）
3. **引用域质量**——权威度、相关性和地理分布
4. **毒性链接检测**——**30 种毒性模式**，包括 PBN 足迹、链接农场和非自然链接速度。自动生成拒绝建议。
5. **反向链接最多的页面**——你最常被链接的页面及其来源
6. **竞争对手差距**——`/seo backlinks gap <你> <竞争对手>`：找到谁链接了他们但没有链接你
7. **新增/丢失跟踪**——最近的链接获取和丢失，附有上下文
它能比得上 Ahrefs 吗？不能。Ahrefs 拥有一个庞大的专有反向链接索引，建立已超过十年。但对于大多数中小型网站来说，这提供了可操作的数据——要拒绝的毒性链接、要瞄准的竞争对手差距、要修复的锚文本分布——而无需每月 99 美元的费用。

## 观看完整演示
我录制了一个完整的演示，展示了设置、审计工作流、Google API 报告以及扩展系统的实际操作：

## Anthropic 合规与市场
有一件事我由衷自豪：claude-seo **通过了 Anthropic 的官方插件验证**。这不是自我评估——而是实际的质量、安全和最佳实践合规检查。该插件已提交给 Anthropic 市场，并列入 [awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) 仓库（49K+ 星）。
这对信任很重要。当有人安装一个 Claude Code 技能时，他们赋予了它访问其终端和文件的权限。获得 Anthropic 验证意味着代码已经过审核，符合其安全和功能性标准。这也是新用户发现该工具的方式——市场现在成了大多数首次安装的来源。

## 社区
**3500+ 星。560 fork。** 我审核了每一个拉取请求，回复了每一个 issue。一些最好的功能来自社区反馈——Excel 导出功能是由 [AI Marketing Hub Pro](https://www.skool.com/ai-marketing-hub-pro) 社区的代理商老板要求的。Firecrawl 集成是因为十几个人独立要求支持 SPA 爬取。
该项目现在还有一个配套工具：[AI Marketing Claude](https://github.com/zubair-trabzada/ai-marketing-claude)，由 Zubair Trabzada 开发，负责审计后的营销操作。这正是我所期望的生态系统增长——其他构建者将平台扩展用于他们自己的用例。
如果你正在使用 claude-seo 并在此基础上构建东西，我想听听。在 [AI Marketing Hub](https://www.skool.com/ai-marketing-hub)（免费，4500+ 成员）留言，或者在 GitHub 上发起讨论。

## 下一步计划
v1.8 已经在进行中。路线图上有三件事：
* **内容策略技能**——基于你现有内容和关键词数据的主题集群规划、内容差距分析和编辑日历生成
* **更深入的 Firecrawl 集成**——定时爬取、变更检测和视觉回归测试
* **自动监控**——基于 cron 的审计，包含差异报告，并在关键指标下降时发出警报
目标是让 claude-seo 不仅仅是一个审计工具，而是一个监控系统，能在问题影响排名之前捕获回归。如果你想塑造构建的内容，路线图讨论在社区中进行。

## 试试看
一条命令安装：
```
curl -sL https://raw.githubusercontent.com/AgriciDaniel/claude-seo/v1.7.2/install.sh | bash
```
Windows：
```
irm https://raw.githubusercontent.com/AgriciDaniel/claude-seo/v1.7.2/install.ps1 | iex
```
* **给仓库加星** - [github.com/AgriciDaniel/claude-seo](https://github.com/AgriciDaniel/claude-seo)
* **阅读文档** - [claude-seo.md](https://claude-seo.md)
* **加入社区** - [AI Marketing Hub](https://www.skool.com/ai-marketing-hub)（免费）或 [Pro](https://www.skool.com/ai-marketing-hub-pro)（每月 88 美元）
* **查看 Rankenstein** - [rankenstein.pro](https://rankenstein.pro) 了解完整的 AI 内容引擎
19 个子技能。12 个子代理。3 个扩展。MIT 许可。永远免费。去搞点破坏吧。

## 常见问题
### 问：如何从旧版本升级？
运行相同的安装命令——它会用最新版本覆盖现有的技能文件。你的扩展配置（DataForSEO API 密钥、Firecrawl 令牌）会保存在单独的配置文件中，安装程序不会触碰它们。无需迁移。
### 问：我需要所有三个扩展吗？
不需要。核心的 16 个子技能无需任何扩展即可工作。DataForSEO 增加实时 SERP 和关键词数据。Firecrawl 增加 JavaScript 渲染的爬取。Banana 增加用于报告的 AI 图像生成。只安装你需要的——每个扩展都是独立的。
### 问：claude-seo 真的免费吗？
工具本身是 MIT 许可的，永远免费。扩展连接到第三方 API，这些 API 有自己的定价：Google API 有慷慨的免费层（每次查询 0.0006 美元），DataForSEO 起步价每月 50 美元，Firecrawl 有每月 500 积分的免费层。你可以完全不花一分钱使用完整的核心工具。
### 问：DataForSEO 和 Firecrawl 有什么区别？
DataForSEO 提供市场数据——其他网站排名的词、关键词量、反向链接配置文件、SERP 功能。Firecrawl 提供爬取基础设施——它使用真实浏览器访问你网站的页面并提取内容。DataForSEO 告诉你市场情况。Firecrawl 告诉你你网站的情况。它们互补。
### 问：我可以将 claude-seo 用于客户工作吗？
可以。MIT 许可意味着你可以商业使用、修改和再分发。Excel 和 PDF 报告专门为交付给客户而设计。社区中的许多代理商老板将其作为主要审计工具，并对报告进行白标处理。

## 相关文章
* [Claude Code 刚刚取代了你整个 SEO 技术栈](/blog/claude-code-seo-stack) - 关于 claude-seo 工作原理的原始深度解析
* [使用 Claude Code 实现免费 Google API SEO 自动化](/blog/google-api-seo-automation-claude-code) - 9 个 Google API 集成的详细教程
* [真正有效的免费 SEO 审计工具](/blog/free-seo-audit-tools) - claude-seo 在更广泛免费工具生态中的位置
* [AI 营销自动化：我日常使用的开源技术栈](/blog/ai-marketing-automation-stack) - 包含 claude-seo、claude-blog 和 Rankenstein 的完整工具集

加入 4500+ AI 营销构建者
获取工作流模板、自动化蓝图，并与出货的 SEO 专家、代理商老板和创作者交流。
[免费加入 →](https://www.skool.com/ai-marketing-hub)
