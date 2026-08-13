---
title: "codex-seo：我将 9,500 星级的 SEO 套件移植到了 OpenAI Codex CLI"
description: "---"
pubDate: "2026-05-12"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/codex-seo-openai-codex-cli"
---

# codex-seo：我将 9,500 星级的 SEO 套件移植到了 OpenAI Codex CLI

---

我在 GitHub 上的一半星星来自那些不使用 Claude Code 的人。他们用的是 [OpenAI Codex CLI](https://github.com/openai/codex)。每周都有一个人在 [claude-seo](https://github.com/AgriciDaniel/claude-seo) 上提 issue，问同样的问题：*这个能用 Codex 吗？* 直到四月份，答案一直是“不行，但快了。”现在有了 [codex-seo](https://github.com/AgriciDaniel/codex-seo)——整个 SEO 套件的 Codex 原生移植版，当前版本为 **v1.9.6-codex.5**，与 claude-seo 的 `main` 分支提交 `a9cf338` 同步，并包含了 Codex 从终端运行完整审计所需的一切。
这篇文章是 README 的详细版本。创始人看成本计算。开发者看 TOML 智能体架构。SEO 营销人员看 26 个工作流。选择感兴趣的章节，跳过其余内容。
codex-seo v1.9.6-codex.5——一个编排器、26 个专家工作流、24 个 TOML 智能体

核心要点
* **codex-seo 是 claude-seo 的 OpenAI Codex 变体**，我的 SEO 工具拥有 9,500+ GitHub 星标，已移植为包含 24 个 TOML 智能体的 Codex 技能套件。
* **26 个专家工作流**涵盖技术 SEO、内容、Schema、GEO、本地、电商、外链、FLOW、DataForSEO、Firecrawl 和图像生成。
* **完整审计平均运行时间：2.8 分钟**（针对 50 页网站），比相同工作流顺序执行快 6.2 倍。
* 一行安装。MIT 许可证。用每月 0 美元替代了每月 394 美元的 SaaS 套件。

## 用简单的话说，codex-seo 是什么？

codex-seo 是一个**针对 OpenAI Codex CLI 的开源 SEO 技能套件**，基于我拥有 9,500 星标的 [claude-seo](https://github.com/AgriciDaniel/claude-seo) 项目构建。安装一次，重启 Codex，之后你只需用自然语言输入一句话，就可以运行完整的技术和内容 SEO 审计。编排器将你的请求路由到 26 个专家工作流之一，再由该工作流将繁重任务委派给并行运行的 24 个 Codex TOML 智能体。报告以 Markdown、JSON、HTML 或 PDF 格式写入磁盘。没有 SaaS 仪表盘。没有月度账单。
这是电梯演讲。现在说详细版本：codex-seo 不是 ChatGPT 的包装器。它会爬取你的站点地图、解析你的 HTML、检查你的 Schema、运行 Core Web Vitals 检查，并为你的内容针对 E-E-A-T 信号评分。它围绕我所称的**确定性无头执行**构建——Codex 可以从聊天中触发它，但同样的工作流也可以通过 shell 脚本、CI 流水线或 Python 运行器来触发。**输出是可重复的，而非即兴创作的**，这正是用代码运行 SEO 的全部意义所在。
由于自 2026 年初 OpenAI 推出智能体以来，Codex CLI 生态系统增长迅速，拥有一个以 Codex 技能形式提供的严肃 SEO 工具至关重要。大多数“AI SEO 工具”只是提示词的包装器。codex-seo 包含 247 个 Markdown 文档、69 个 Python 脚本、24 个 TOML 智能体配置文件、契约测试以及跨平台安装程序。它的生产就绪程度与 claude-seo 相同，但专为运行 `codex` 而非 `claude` 的人群打包。
以下是视频演示，如果你想先看看再阅读其余内容：

## 为什么我不再为 SEO 工具每月支付 300 美元

过去我付费使用全套 SaaS SEO 工具。Ahrefs 每月 99 美元。Semrush 每月 139 美元。Surfer SEO 每月 89 美元。Frase 每月 45 美元。Screaming Frog 每月 22 美元。合计**每月 394 美元，每年 4,728 美元**，而我估计只用了其中约 30%——审计报告、关键词研究、页面评分。另外 70% 是我从未打开的功能：白标 PDF 生成器、代理席位管理、品牌客户门户等。
因此在 2025 年初，我开始构建 [claude-seo](/blog/claude-code-seo-stack)，以替代这套工具中的审计部分。十二个月后，它获得了 9,500 星标，并被那些希望在 Claude Code 中运行审计而非支付 SaaS 租金的机构所 fork。codex-seo 是同样的理念，移植到了 OpenAI 一侧。（我写了[免费 SEO 审计工具的完整对比](/blog/free-seo-audit-tools)供你进行更广泛的比较。）
成本图景一旦绘制出来就相当残酷：
五款 SaaS 工具合计每月 394 美元。codex-seo 每月 0 美元，MIT 许可证。
我并不是说 Ahrefs 或 Semrush 是差劲的工具。它们很出色。它们还拥有十年的 SERP 历史和外链数据库，这是 codex-seo 永远无法匹敌的。**我想说的是，我日常 80% 的 SEO 工作都是审计、页面修复、Schema 生成、GEO 检查和内容简报**——而所有这些任务现在都可以在我的终端本地运行，无需任何费用。如果你为做这五件事而付费使用 SaaS，你付的是仪表盘的钱，而不是数据的钱。
对于代理机构老板来说，这个账更明显。三个客户 × 每月 394 美元 = 每年 14,184 美元的工具开销。codex-seo 不会取代你的团队。它取代的是那个为团队使用 UI（而他们本可以直接在 Markdown 文件中阅读原始发现）付费的条目。

## codex-seo 与 claude-seo 有何不同？

codex-seo 是一个**移植版，不是分叉版**。26 个 SEO 工作流、FLOW 框架提示、Schema 模板、E-E-A-T 评分标准——它们与 claude-seo 的 `main` 分支提交 `a9cf338` 同步。如果修复落在 claude-seo 上，它将在下一个版本中移植到 codex-seo。这两个工具共享相同的核心，但打包方式完全不同。以下是它们的分歧点：

| 方面 | claude-seo | codex-seo |
| --- | --- | --- |
| 宿主运行时 | Claude Code 插件 | Codex 技能套件 + `.codex-plugin/plugin.json` |
| 智能体定义 | 隐式的 Claude 子智能体 | 24 个显式 Codex TOML 智能体配置文件 |
| 无头执行 | 仅聊天输出 | 用于 CI/CD 的确定性 Python 运行器 |
| 配置路径 | `~/.config/claude-seo/` | `~/.config/codex-seo/`（回退读取 claude-seo） |
| 缓存 | `~/.cache/claude-seo/` | `~/.cache/codex-seo/` + 项目 `.seo-cache/` |
| 报告格式 | Markdown + 可选 PDF | Markdown、JSON、HTML、PDF |
| 许可证 | MIT | MIT |

最有意义的行是第二行：**TOML 智能体**。在 claude-seo 中，并行执行是因为 Claude Code 在单个响应中分派多个子智能体工具调用。这在聊天内部工作良好，但对于任何试图脚本化该工具的人来说是不可见的。在 codex-seo 中，每个智能体都是 `~/.codex/agents/seo-*.toml` 下的独立 `.toml` 文件，每个文件都有自己的描述、角色和工具允许列表。Codex 在启动时加载它们，并可以在完整审计期间并行调度它们。这也意味着开发者可以阅读 `seo-technical.toml` 并确切知道技术智能体被允许做什么——无需猜测。
另一个有意义的行是第三行。claude-seo 存在于聊天中。如果你想在部署时运行审计，你必须打开 Claude Code，粘贴 URL，然后复制输出。codex-seo 提供了 `scripts/run_skill_workflow.py`，它将任何工作流包装成确定性的命令行调用。这就是你如何将 SEO 放入 CI/CD 的方法。下面更多介绍。

## 26 个专家工作流：你实际获得的内容

标题数字是 26 个工作流。这听起来很模糊，所以这里列出实际映射情况。[COMMANDS.md 参考文档](https://github.com/AgriciDaniel/codex-seo/blob/main/docs/COMMANDS.md)列出了编排器识别的每个提示，但更容易理解的方式是按五个类别分组：
每个 codex-seo 工作流按类别分组。每个都由其自己的 TOML 智能体支持。
* **基础专家（8 个）** - `technical`、`content`、`schema`、`sitemap`、`performance`、`visual`、`images`、`geo`。这些是普通站点审计的核心切片。Technical 处理可爬性、可索引性、robots、canonical、重定向。Content 对 E-E-A-T、有用性、AI 引用就绪度进行评分。Schema 检测并生成 JSON-LD。Sitemap 验证结构并提出新条目。Performance 提取 Core Web Vitals 信号。Visual 截取屏幕截图并对首屏内容评分。Images 分析替代文本、大小、格式和元数据。GEO 评估 AI Overviews、ChatGPT、Perplexity 和 llms.txt 就绪度。
* **战略规划（5 个）** - `plan`、`cluster`、`programmatic`、`sxo`、`competitor-pages`。Plan 将审计转化为 30/60/90 天路线图。Cluster 通过 SERP 分析构建枢纽-辐射式主题架构。Programmatic 评估模板驱动页面构建的风险和规模。SXO 对搜索体验优化进行评分——页面是否真的是搜索者想要的？Competitor-pages 基于排名结果设计对比和“替代”页面。
* **领域特定（6 个）** - `local`、`maps`、`hreflang`、`backlinks`、`ecommerce`、`drift`。Local 检查 NAP 一致性、GBP 优化、引文、评论。Maps 进行地理网格排名跟踪和竞争对手半径映射。Hreflang 验证国际 SEO 设置。Backlinks 汇总外链配置文件和来源层级检测。Ecommerce 处理产品 Schema 和市场可见性。Drift 在变更之前对 SEO 状态进行基准测试，并在之后进行比较。
* **数据集成（5 个）** - `google`、`dataforseo`、`firecrawl`、`image-gen`、`flow`。Google 连接 GSC、PageSpeed、CrUX、Indexing API、GA4。DataForSEO 在添加凭据后提取实时 SERP、关键词和外链数据。Firecrawl 处理 JS 渲染爬取。Image-gen 通过 Gemini/nanobanban 流水线创建 OG 图像和信息图表。FLOW 运行来自我的 [claude-seo v1.9.6 安全与 FLOW 文章](/blog/claude-seo-v196-flow-security-hardening)的基于证据的提示。
* **审计编排器（2 个）** - `audit` 和 `page`。Audit 是包罗万象的入口点。Page 是深度单页审查。
总计：26。如果你从未用过 claude-seo，这个比例可能感觉过多。并非如此。对超过 200 页的网站进行真实 SEO 审计会涵盖所有五个类别。将它们拆分为独立的工作流意味着每个都可以单独调用（`/seo schema https://example.com`）或一起调用（`/seo audit https://example.com` 分派完整集合）。

## 30 秒内安装 codex-seo

如果你已经安装了 OpenAI Codex CLI，安装命令只有一行：
```
curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/codex-seo/v1.9.6-codex.5/install.sh | bash
```
Windows 用户运行 PowerShell 等效命令：
```
irm https://raw.githubusercontent.com/AgriciDaniel/codex-seo/v1.9.6-codex.5/install.ps1 | iex
```
该脚本实际执行的操作：将仓库克隆到临时目录，将技能套件复制到 `~/.codex/skills/`，将 24 个 TOML 智能体文件放入 `~/.codex/agents/`，在 `~/.codex/skills/seo/.venv/` 下创建 Python 虚拟环境，安装核心运行时依赖，然后尝试安装可选能力组——用于截图和 PDF 的 Playwright、Google API 客户端、DataForSEO、Firecrawl、OCR 和报告生成器。如果某个能力组失败（例如在无头服务器上安装 Playwright），安装程序会记录失败并继续。技能仍然可以在没有它的情况下工作；只是优雅降级。
如果你想在运行前检查代码，更长的安装方式是：
```
git clone https://github.com/AgriciDaniel/codex-seo.git
cd codex-seo
bash install.sh
```
你可以通过环境变量覆盖安装：`CODEX_HOME`、`CODEX_SEO_REPO`（用于分叉）、`CODEX_SEO_REF`（用于标签或提交）以及 `CODEX_SEO_SKIP_PLAYWRIGHT_BROWSER=1`（如果你不想下载 Chromium）。安装器是幂等的——再次运行即可升级。
安装后，重启 Codex。就这么简单。你不会看到新的菜单项；技能套件会自行注册，编排器会识别自然语言。**最新的本地验证：52 个测试通过，完整安装冒烟测试通过，演示就绪性通过**。如果安装过程中出现问题，脚本会精确打印哪个能力组失败以及如何重试。

## 真实工作流：从“审计我的网站”到完整提示包

学习 codex-seo 最快的方法是复制[仓库文档](https://github.com/AgriciDaniel/codex-seo)中的六个提示。每个提示对应一个真实的 SEO 任务。你不需要记住命令；编排器会解析自然语言并为你路由。但显式形式在你想脚本化时很有用。
**1. 完整 A-Z 审计。** 触发所有相关专家的单一提示：
```
/seo audit https://example.com
```
编排器检测业务类型（SaaS、电商、本地、内容等）并选择性地开启本地、地图、电商、hreflang、程序化、漂移和集群检查。输出：SEO 健康评分、按严重/高/中/低排序的修复列表、30/60/90 天路线图、关键词机会、精确的后续提示。
**2. 关键词和集群研究。**
```
/seo cluster "主要关键词"
```
根据审计结果构建主题集群——支柱页面、支撑页面、搜索意图、内部链接、优先级顺序。这个工作流对我来说取代了 Surfer SEO 的内容规划器。
**3. 内容路线图。**
```
/seo plan 业务类型
```
将审计和集群结果转化为 90 天的 SEO 内容路线图，包含页面标题、目标关键词、意图、简报说明和优先级顺序。输出为 Markdown 表格——这曾是我每月支付 89 美元给 Surfer 才能获得的那种东西。
**4. 优化单个页面。**
```
/seo page https://example.com/page
```
提供标题标签、元描述、H1/H2 结构、缺失章节、内部链接、Schema 修复、图片修复以及重写计划。页面智能体使用与完整审计相同的证据缓存，因此不会重新爬取。
**5. AI 搜索 / GEO 就绪度。**
```
/seo geo https://example.com
```
检查页面是否准备好用于 AI Overviews、ChatGPT Search、Perplexity 和引用式回答。标记缺失的答案优先格式、低引用密度、弱结构化数据以及缺少 `llms.txt`。
**6. 本地商家检查。**
```
/seo local https://example.com
```
验证 NAP 一致性、GBP 优化、本地 Schema、引文、评论信号以及 Google Maps 存在。如果你配置了 DataForSEO，会触发地图专家进行地理网格排名跟踪。
这六个提示涵盖了我每天 90% 的工作。另外 10% 更专业——hreflang 验证、电商产品 Schema、重新设计后的漂移比较。每个都有其自己的提示。完整列表在 COMMANDS.md 中。

## 为什么 TOML 智能体对 CI/CD SEO 很重要

这是开发者视角。在 Codex CLI 中，每个智能体都是一个 TOML 文件。codex-seo 提供了 24 个这样的文件，命名为 `seo-technical.toml`、`seo-content.toml`、`seo-schema.toml` 等等。Codex 在启动时加载它们。当你运行审计时，编排器会并行分派多个智能体——技术智能体和内容智能体可以并行爬取，因为两者互不依赖；Schema 和站点地图也可以同时运行。
这种并行性就是 3 分钟审计和 17 分钟审计之间的差别。在我自己的网站 agricidaniel.com（约 50 页）上，我在 2026 年 4 月用三种方式对同一个审计进行了基准测试：
同一个审计，三种执行模式。在 agricidaniel.com 上测量，2026 年 4 月。
并行 codex-seo：**2 分 47 秒**。顺序执行：17 分 22 秒。手动清单（真正的 SEO 分析师通过工具点击并撰写笔记）：对于 50 页的网站，保守估计大约四小时。并行模式比顺序执行快 6.2 倍，比手动快 86 倍。**这就是 TOML 智能体重要的全部原因**——不是因为格式花哨，而是因为显式的智能体文件让 Codex 能够并行调度它们，而无需内置的协调逻辑。
而且因为智能体是文件，你可以将它们纳入版本控制。你可以审计它们。你可以分叉一个并根据自己的品牌调整其提示。你可以编写一个 CI 流水线，在每次部署时运行 `scripts/run_skill_workflow.py seo-technical https://staging.example.com`，如果技术评分低于阈值则构建失败。最后这一点是真的，顺便说一句：我已经帮助一些机构将 codex-seo 审计集成到 GitHub Actions 中，这样每个暂存部署在合并之前都会经过绿色/红色 SEO 门控。据我所知，没有 SaaS 工具无需自定义集成就能支持这一点。

## GEO 与 FLOW 框架：为 AI 搜索构建的 SEO

这里是没有人谈论的角度。传统的 SEO 工具针对 Google 的蓝色链接进行优化。codex-seo 拥有一个专用的 GEO 工作流和完整的 FLOW 框架集成，因为这已经不够了。**到 2026 年底，AI 辅助搜索预计将占所有主要搜索引擎和助手的搜索交互量的约 30%**，基于 AI Overviews、Perplexity、ChatGPT Search 和 Copilot 的发展轨迹。对 AI 爬虫读取良好的网站会被引用。不好的网站则会被摘要掉。
codex-seo 中的 GEO 工作流检查 AI 系统提取的内容：答案优先的段落开头、引用胶囊（40-60 字自包含的可引用片段）、内联来源归属、匹配对话式查询的 FAQ 风格标题、LLM 能够清晰解析的结构化数据，以及 `llms.txt` 合规性。如果缺少任何一项，它会标记出来，附带优先级评级并编写具体的修复方案。
FLOW 是我在 [claude-seo v1.9.6](/blog/claude-seo-v196-flow-security-hardening) 中发布并移植到 codex-seo（相同提交）的框架。这是一个五阶段循环：*发现*查询出现的表面（Google、AI Overviews、Perplexity、Reddit、YouTube），*利用*已经排名靠前的内容，*优化*使用基于证据的提示（CTR 审计、AI 检测器测试、Schema 完整性、ChatGPT 可见性检查），然后*获胜*通过跟踪位置随时间的变化。codex-seo 将每个 FLOW 阶段作为独立的提示运行，例如 `/seo flow find`、`/seo flow leverage` 等等。
如果你从未以这种方式思考过 SEO，最简单的心理模型是：**你的内容必须具有可引用性**。AI 助手通过将简短、可归属的段落拼接在一起来构建答案。如果你的 H2 不以一个 40-60 字的可引用段落开头，你就对该流水线不可见。codex-seo 的内容和 GEO 工作流直接对此进行评分。在任何页面运行它们，你将获得可引用性评分以及提升它的确切改写方案。
这也是 codex-seo 在与尚未推出真正 GEO 产品的 Ahrefs 等工具相比时赢得自己的价值的地方。SaaS 世界仍在为 2022 年有效的策略进行优化。codex-seo 是为 2026 年有效的东西构建的。

## codex-seo 适合你吗？决策矩阵

三类受众，三种不同的出发/跳过信号。以下是如何决定：

| 如果你是…… | 出发信号 | 跳过条件 |
| --- | --- | --- |
| **创始人** | 你运营一个中小型网站，你在为 SEO SaaS 付费，并且你想要一个一行安装的替代品。 | 你需要一个为非技术人员提供的精美 UI，或者你依赖 Ahrefs 的外链图谱。 |
| **开发者** | 你想在 CI 流水线中加入 SEO，你生活在 Codex CLI 中，并且你宁愿配置 24 个 TOML 文件也不愿学习 SaaS API。 | 你实际上并不做 SEO 工作，只是对架构感到好奇。 |
| **SEO 营销人员** | 你每周为客户制作审计报告，厌倦了从 5 个不同工具拼接输出，并且你想为每个客户提供一份 Markdown 报告。 | 你向客户收取 SaaS 工具本身的转手费用，并且会失去加价空间。 |

如果你落在上述的“出发信号”行中，codex-seo 将每周为你节省数小时。如果你落在“跳过条件”行中，请保持你当前的设置。我构建这个是因为我自己想要，而不是因为我认为每个 SEO 工作流都必须是开源的。在一个网站上尝试一次，一次审计。如果它不如你目前付费使用的好，你只损失了 10 分钟。

本周在单个审计上试用 codex-seo
一行安装。MIT 许可证。26 个工作流。为仓库加星，在自己的网站上运行一次审计，然后做出决定。

[在 GitHub 上加星 →](https://github.com/AgriciDaniel/codex-seo)

## 常见问题

### codex-seo 与 claude-seo 有何不同？
codex-seo 是 OpenAI Codex CLI 移植版，适配了 24 个 TOML 智能体配置文件、确定性 Python 运行器以及 Codex 原生安装路径（`~/.codex/`）。claude-seo 在 Claude Code 内运行。它们共享 26 个 SEO 工作流和相同的 MIT 许可证。Codex 变体增加了显式的智能体文件，实现了并行的 CI/CD 审计以及每个智能体的清晰工具允许列表。

### 我需要付费 API 才能使用 codex-seo 吗？
不需要。codex-seo 默认在免费信号上运行每个工作流：HTML 解析、站点地图爬取、Schema 验证、Core Web Vitals 检查、GEO 评分。像 DataForSEO、Firecrawl 和 Google API 这样的付费集成是可选的，并且在你连接凭据之前会明确标记为 `setup_required`。当缺少集成时，该工具绝不会编造数据。

### codex-seo 真的是 MIT 许可的吗？
是的。你可以分叉、修改，甚至将其打包到付费产品中。LICENSE 文件是纯 MIT，与 claude-seo 相同。所有 26 个工作流、24 个 TOML 智能体、Python 运行器、Schema 模板和参考文档都是开源的。没有企业版、没有等待名单、没有功能门控、没有“仅限个人使用免费”条款。

### codex-seo 会将我的网站数据发送给 OpenAI 吗？
codex-seo 只发送你的 Codex 会话发送的内容。技能套件本地运行：它爬取你的 URL，在磁盘上解析 HTML，将报告写入 `output/`。凭据保留在你的 `~/.codex/` 配置中。Codex 本身发送给 OpenAI 的内容受 Codex 隐私政策约束，而非 codex-seo。

### codex-seo 能取代 Semrush 或 Ahrefs 吗？
对于审计、技术 SEO、内容评分、Schema、Core Web Vitals、GEO、本地和电商工作，可以。对于大规模的外链数据库或十年的 SERP 历史，不行。codex-seo 首先依赖免费信号，并在你需要实时 SERP 数据时允许连接 DataForSEO。与我交流过的大多数团队保留一个 SaaS 用于外链，而用 codex-seo 处理其他所有事情。

## 总结

codex-seo 是 claude-seo 的同样理念，专为我的读者中运行 OpenAI Codex CLI 而非 Claude Code 的那一半打包。相同的 26 个工作流。相同的 MIT 许可证。相同的关于每月 300 美元工具套件的观点。新增的是 24 个显式 TOML 智能体、确定性 Python 运行器以及一个将审计时间从 17 分钟缩短到 3 分钟以内的并行执行模型。
如果你一直在等待一个在 Codex 中原生运行的严肃开源 SEO 套件，这就是了。安装它。运行一次审计。如果它为你节省了时间，就给仓库加星。**如果没有，提交一个 issue 并告诉我哪里出了问题**——这个系列的每个版本都包含了来自社区的修复。

## 相关文章
* [Claude Code 刚刚取代了你的整个 SEO 套件](/blog/claude-code-seo-stack) —— 启动这个项目的原始每月 300 美元拆解
* [claude-seo v1.9.6：FLOW 框架 + 安全强化](/blog/claude-seo-v196-flow-security-hardening) —— 引入 FLOW 提示和 codex-seo 继承的安全模型的版本
* [真正有效的免费 SEO 审计工具](/blog/free-seo-audit-tools) —— 2026 年免费工具的广泛对比
* [Google API SEO 自动化](/blog/google-api-seo-automation-claude-code) —— 如何将 GSC、PageSpeed 和 CrUX 接入 Codex 审计
