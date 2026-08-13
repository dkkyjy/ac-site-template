---
title: "我用免费谷歌API取代了每月500美元的SEO数据——以下是配置方法"
description: "---"
pubDate: "2026-03-28"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/google-api-seo-automation-claude-code"
---

# 我用免费谷歌API取代了每月500美元的SEO数据——以下是配置方法

---

Ahrefs 每月收费 499 美元获取 SEO 数据。Semrush 每月收费 449 美元。两者加起来，每年就是 11,376 美元，而这些数据 **谷歌通过自己的 API 免费提供。**
我刚刚发布了 [claude-seo](https://github.com/AgriciDaniel/claude-seo) 和 [claude-blog](https://github.com/AgriciDaniel/claude-blog) 自上线以来最大的更新。这两个工具现在直接连接 9 个谷歌 API，获取的是驱动谷歌自身排名算法的相同数据。成本？每次查询约 0.0006 美元。每天 1,000 次查询，每月 18 美元。这不是笔误。
以下是变更内容、为何重要，以及如何在 5 分钟内完成配置。
## v1.7.0 和 v1.6.5 的变更
两个工具在同一天发布了新的谷歌 API 子技能：
* **[claude-seo v1.7.0](https://github.com/AgriciDaniel/claude-seo)** 新增了 `seo-google`——21 个命令、11 个 Python 脚本、PDF 报告生成、SSRF 防护，以及一个在检测到谷歌 API 凭据时自动激活的新子代理。附带 10 个参考文档文件。
* **[claude-blog v1.6.5](https://github.com/AgriciDaniel/claude-blog)** 新增了 `blog-google`——13 个命令、11 个 Python 脚本、YouTube 视频自动发现与嵌入，以及 VideoObject JSON-LD 模式生成。同时还清理了全部 22 个技能前置元数据，以符合 Claude Code 插件规范。
两者共享一个配置文件 `~/.config/claude-seo/google-api.json`，因此你只需设置一次凭据，两个工具都能使用。如果你读过[claude-seo 如何取代我每月 300 美元的 SEO 工具栈](/blog/claude-code-seo-stack)或[claude-blog 如何取代我的博客写手](/blog/claude-code-blog-writer)，那么这就是下一步进化：**将谷歌第一方数据直接注入审计和内容流水线。**
## 9 个 API（以及它们为何全部免费）
谷歌提供这些 API 并配有慷慨的免费配额，因为他们希望开发者在其平台上构建。以下是我们接入的内容：
1. **PageSpeed Insights API**——单次调用即可获取 Lighthouse 实验室评分及 CrUX 现场数据。每天 25,000 次查询免费。
2. **Chrome UX Report (CrUX) API**——真实的 Chrome 用户指标：LCP、INP、CLS、TTFB、FCP。这是谷歌用于核心网页指标排名信号的实际数据。包含 25 周历史趋势。
3. **Search Console API**——查询、点击次数、展示次数、平均排名。每个站点每分钟 1,200 次查询。
4. **URL Inspection API**——真实的索引状态、抓取详情、移动端适用性、首选 Canonical。精确了解谷歌如何看待你的页面。
5. **Indexing API**——提交 URL 以立即索引，无需等待下次抓取周期。每天 200 个 URL。
6. **GA4 Data API**——自然流量、会话数、页面浏览量、跳出率、互动指标。
7. **Cloud Natural Language API**——NLP 实体抽取与情感分析。了解谷歌将哪些实体与你内容关联，并针对 E-E-A-T 进行优化。
8. **YouTube Data API**——视频搜索、元数据、观看次数、频道权威性。为 claude-blog 中的自动嵌入功能提供支持。
9. **Google Ads Keyword Planner API**——搜索量的黄金标准。真实的谷歌查询量，而非第三方估算。
上述每个 API 都有免费套餐，覆盖正常使用。前 8 个无需信用卡。Keyword Planner 需要一个 Google Ads 账户（免费创建），不过开发者令牌审批过程需要几天时间。
各 API 官方文档：[PageSpeed Insights API](https://developers.google.com/speed/docs/insights/v5/get-started)、[CrUX API](https://developer.chrome.com/docs/crux/api)、[Search Console API](https://developers.google.com/webmaster-tools/v1/api_reference_index)、[Indexing API](https://developers.google.com/search/apis/indexing-api/v3/quickstart)、[GA4 Data API](https://developers.google.com/analytics/devguides/reporting/data/v1)、[Cloud NLP API](https://cloud.google.com/natural-language/docs)、[YouTube Data API](https://developers.google.com/youtube/v3)、[Keyword Planner API](https://developers.google.com/google-ads/api/docs/keyword-planning/overview)。
PageSpeed Insights API——同样的 Lighthouse + CrUX 数据驱动着谷歌的排名信号，现在直接注入你的审计工作流。
## 成本计算：颠覆一切
让我具体说明。以下是主要 SEO 平台每年对谷歌自身 API 免费（或近乎免费）提供的数据所收取的费用：
年度 SEO 数据成本
Ahrefs：5,988 美元/年
Semrush：5,399 美元/年
Moz Pro：2,148 美元/年
谷歌 API：约 216 美元/年
使用免费谷歌 API 节省 5,772 美元/年
基于每天 1,000 次查询，平均每次 0.0006 美元
年度成本对比——付费 SEO 平台 vs 直接访问谷歌 API
但没人谈论的是这一点：**谷歌 API 的数据实际上更好。** 当你通过 Ahrefs 或 Semrush 检查核心网页指标时，它们是从自己的服务器运行 Lighthouse 测试。那是合成的实验室数据。而当你使用 CrUX API 时，你得到的是真实的 Chrome 用户指标，也就是谷歌用来评估你站点排名的同一数据集。第一方源数据 vs 第三方近似值。免费选项才是权威数据源。
Search Console 数据也是如此。Ahrefs 基于关键词排名追踪估算你的搜索流量。Search Console 则给你来自谷歌自身日志的真实点击和展示次数。没有任何估算。你是在读取源头数据。
Search Console API 数据——来自谷歌自身日志的真实点击和展示次数，而非第三方估算。
## YouTube 嵌入与 AI 可见性
这是 claude-blog v1.6.5 中最让我兴奋的功能。YouTube 提及与 AI 搜索可见性之间存在 **0.737 的相关性**，基于一项 75,000 个品牌的研究。这是发现的最强单一信号。强于 FAQ 模式、强于答案优先格式、强于其他任何测试过的信号。
AI 搜索引用相关性因素
YouTube 嵌入：0.737
FAQ 模式：0.651
答案优先：0.589
引用统计数据：0.523
内部链接 5+：0.412
YouTube 嵌入 = 最强单一信号
来源：75,000 个品牌的 AI 可见性研究（Ahrefs）
AI 搜索引用相关性——YouTube 嵌入领先所有其他信号
更多支持数据：AI 概览中的视频引用同比增长 **414%**（BrightEdge 2025 年第一季度）。指南类视频引用增长 651%。YouTube 被 AI 系统引用的次数是其他任何视频平台的 200 倍以上。嵌入视频的页面排名首页的几率高出 **53 倍**（Forrester）。
因此，claude-blog v1.6.5 现在会在每篇博客文章中自动发现并嵌入相关的 YouTube 视频。工作原理如下：
* **自动发现**——在研究阶段，claude-blog 通过 Data API 在 YouTube 上搜索与你文章主题相关的视频。它根据相关性、观看次数、时效性、频道权威性和互动率为每个视频打分。只有得分超过 50/100 的视频才会被嵌入。
* **延迟加载**——使用 `srcdoc` 模式替代标准 YouTube 嵌入方式。初始负载约为 5KB，而标准 iframe 约为 500KB。每个嵌入的初始页面重量减少了 99%。
* **AI 爬虫回退**——`<noscript>` 标签以纯文本形式呈现视频标题、频道和描述。GPTBot、PerplexityBot、ClaudeBot 和 Google-Extended 即使没有 JavaScript 也能看到并引用视频内容。
* **VideoObject 模式**——为每个嵌入生成 JSON-LD VideoObject，使每篇博客页面的模式类型总数达到 7 种（之前为 6 种）。模式包含时长、观看次数、上传日期和缩略图 URL。
嵌入格式支持 MDX、HTML、Markdown 和 Hugo。每篇文章最多 2-3 个视频，嵌入之间至少间隔 500 字。
## 4 层凭据体系
并非每个用户都需要所有 API。因此，两个工具采用渐进式凭据体系——你从简单的开始，根据需要逐步增加：
凭据层级——配置时间 vs 解锁的 API
第 0 层：5 个 API（PSI、CrUX、YouTube、NLP），2 分钟配置
第 1 层：+3 个 API（GSC、Inspect、Index），10 分钟配置
第 2 层：+GA4 + 属性 ID
第 3 层：+关键词 + Ads 开发者令牌
80% 的价值在 2 分钟内解锁（第 0 层）
渐进式凭据体系——从简单开始，按需添加 API
配置是一个两个工具共享的 JSON 文件：
```
// ~/.config/claude-seo/google-api.json
{
  "api_key": "AIzaSy...",
  "oauth_client_path": "/path/to/client_secret.json",
  "default_property": "sc-domain:yoursite.com",
  "ga4_property_id": "properties/123456789"
}
```
在 claude-seo 中，`seo-google` 代理会在审计期间检测到凭据时自动启动。没有凭据？审计仍然基于爬取分析运行。有凭据？你会获得真实的 CrUX 现场数据、实际的索引状态和 Search Console 性能指标叠加其上。**零破坏性变更——凭据是附加的。**
## 面向客户交付件的 PDF 报告
claude-seo v1.7.0 现在可以根据谷歌 API 数据生成企业级 PDF 报告。这是 [AI 营销中心](https://www.skool.com/ai-marketing-hub) 的代理商用户最常要求的功能之一。
报告使用 WeasyPrint 进行 HTML 到 PDF 的渲染，并使用 matplotlib 以 200 DPI 生成图表。模板为 A4 格式，包含：
* 带评分摘要的封面页
* 带章节徽章的目录
* CrUX 趋势图（25 周时间线）
* 性能分布仪表盘
* 数据表格与热力图
* 按优先级排序的推荐部分
四种报告类型：CWV 审计、GSC 性能、索引状态或完整综合报告。运行 `/seo google report full`，然后将 PDF 交给客户。这是一个从 SEO 代理商那里可能要花费 500 美元以上的交付件。
在安全方面，所有用户 URL 都通过 `validate_url()` 函数进行检查，阻止私有 IP、回环地址和 GCP 元数据端点。SSRF 防护内置于每个脚本中，而非事后添加。OAuth 令牌不再存储客户端密钥，而是每次请求时从 client\_secret.json 文件中读取。**默认有 8 种凭据模式被 .gitignore 忽略**——包括 .env 文件、客户端密钥、OAuth 令牌、服务账户密钥。
## 如何配置（5 分钟）
由于两个工具共享凭据，配置方法相同。以下是快速进入第 0 层（5 个 API，2 分钟）的方法：
1. 前往 [Google Cloud 控制台](https://console.cloud.google.com) 创建一个项目（或使用现有项目）
2. 启用以下 API：PageSpeed Insights、Chrome UX Report、YouTube Data API v3、Cloud Natural Language
3. 在凭据中创建一个 API 密钥
4. 将其添加到你的配置中：
```
mkdir -p ~/.config/claude-seo
echo '{"api_key": "YOUR_API_KEY_HERE"}' > ~/.config/claude-seo/google-api.json
```
测试：
```
# 在安装了 claude-seo 的 Claude Code 中：
/seo google pagespeed https://yoursite.com
# 或使用 claude-blog：
/blog google crux-history https://yoursite.com
```
对于第 1 层（添加 Search Console、URL Inspection、Indexing API），你需要 OAuth 2.0 凭据。创建一个测试模式的 OAuth 同意屏幕（无需验证），创建 OAuth 客户端凭据（桌面应用类型），下载 client\_secret.json 文件，并将路径添加到配置中。首次运行第 1 层命令时，会打开浏览器窗口进行授权，回调地址为 localhost:8085。总共约需 10 分钟。
claude-seo 和 claude-blog 都附带配置指南。运行 `/seo google setup` 或 `/blog google setup`，它会逐步引导你完成所有步骤，检查哪些 API 已启用、哪些凭据缺失。
## 下一步计划
本次更新使 claude-blog 的技能总数达到 **22 个子技能**，claude-seo 达到 **18 个技能**（15 个核心 + 2 个扩展 + 新的 seo-google）。两者共同覆盖从关键词研究、内容创作、技术审计到性能监控的完整生命周期，全部在终端中完成。
路线图上的下一步：
* **Google Merchant Center API**——电商 SEO 的产品模式丰富化
* **自动化周报**——通过 n8n 集成定时生成 PDF
* **多属性聚合**——将多个域名的 Search Console 数据汇总到单个看板视图中
如果你已经在使用 claude-seo 或 claude-blog，请更新到最新版本并运行 `/seo google setup` 开始使用。如果你是新手，可以查看[免费 SEO 审计工具对比](/blog/free-seo-audit-tools)或了解这两个工具如何融入[我日常使用的完整 AI 营销自动化栈](/blog/ai-marketing-automation-stack)。这两个工具都入选了[2026 年最佳 Claude Code 技能](/blog/best-claude-code-skills-2026)。
发布说明在 GitHub 上：[claude-blog v1.6.5](https://github.com/AgriciDaniel/claude-blog/releases/tag/v1.6.5) 和 [claude-seo v1.7.0](https://github.com/AgriciDaniel/claude-seo/releases/tag/v1.7.0)。
## 常见问题
### 谷歌 API 真的免费吗？
是的。全部 9 个 API 都有慷慨的免费套餐。PageSpeed Insights 每天允许 25,000 次查询。CrUX 无限制。Search Console 每个站点每分钟允许 1,200 次查询。唯一需要付费账户的 API 是 Keyword Planner，它需要一个 Google Ads 账户（免费创建，但需要批准的开发者令牌）。对于单个站点或小型代理商的典型使用量，你永远不会超过免费配额。
### 这与现有的 claude-seo 审计有何不同？
现有审计基于爬取分析（抓取你的页面并解析 HTML）。Google API 集成在其之上增加了第一方数据：来自 CrUX 的真实 Chrome 用户指标、来自 URL Inspection 的实际索引状态、来自 GSC 的搜索性能、以及来自 GA4 的自然流量。无论是否使用 API 凭据，审计都可以运行，但使用凭据时，你将获得权威的谷歌数据而非估算。
### 我需要为 claude-seo 和 claude-blog 分别设置凭据吗？
不需要。两个工具共享同一个配置文件 `~/.config/claude-seo/google-api.json`。一次设置凭据，两个工具自动使用。4 层体系在两者中完全相同。
### 我可以用服务账户代替 OAuth 吗？
可以。对于 Search Console、Indexing API 和 GA4，你可以使用 Google Cloud 服务账户代替 OAuth。这更适合自动化工作流，因为没有基于浏览器的登录。创建一个服务账户，授予其对 Search Console 属性的访问权限，然后将 JSON 密钥路径添加到配置中。脚本会检测并使用可用的凭据类型。
### API 速率限制如何？
速率限制对于正常使用来说很慷慨。PageSpeed：25,000/天。CrUX：150/分钟（与历史记录共享）。GSC：1,200/分钟/站点。Indexing：200 个 URL/天。GA4：10 个并发请求。YouTube：10,000 单位/天。所有脚本都包含针对速率限制错误的指数退避。对于批量操作，批处理命令会自动处理分页和限流。
## 相关文章
* [Claude Code 刚刚取代了整个 SEO 工具栈](/blog/claude-code-seo-stack)——如何用一个终端命令取代每月 300 美元的 SEO 工具
* [Claude Code 刚刚取代了你的博客写手](/blog/claude-code-blog-writer)——针对谷歌排名和 AI 引用的双重优化内容
* [真正免费且好用的 SEO 审计工具](/blog/free-seo-audit-tools)——取代付费订阅的真正免费 SEO 审计工具

加入 4,500+ AI 营销构建者
获取工作流模板、自动化蓝图，并与 SEO、代理商所有者和创作者交流分享。
[免费加入 →](https://www.skool.com/ai-marketing-hub)
