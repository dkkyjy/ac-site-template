---
title: "claude-ads v1.7.1：SSS+ 精炼、动画横幅、10 个已验证引用"
description: "---"
pubDate: "2026-05-19"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-ads-v1-7-1-release"
---

# claude-ads v1.7.1：SSS+ 精炼、动画横幅、10 个已验证引用

---

## claude-ads v1.7.1 的更新内容
claude-ads v1.7.1 于 2026 年 5 月 18 日发布。claude-ads 是一个开源广告审计工具，运行于 Claude Code 内部，涵盖 Google Ads、Meta Ads、Amazon Ads、YouTube、LinkedIn、TikTok、Microsoft Ads 和 Apple Search Ads。v1.7.1 是一个精炼版本。没有新增检查项、没有新技能、也没有新平台。本次工作旨在将技能提升至 SSS+ 级别，涉及 README、品牌标识、横幅、双仓库定位以及引用规范。八个平台、22 项子技能、209 项加权检查、41 项测试仍然通过。这一部分没有变化。变化的是围绕它的所有内容。
v1.7.0 → v1.7.1 差异：文档精炼，零行为变化。
以下是本次发布的内容：
* **动画 SVG 横幅：** 13.9 KB 优化文件，22 个 SMIL 动画，`role="img"` 以及用于屏幕阅读器的描述性标记
* **品牌标识套件：** 参数化模板位于 `branding/banner-template.html`，外加一份 `AGENT-PROMPT.md`，以便终端风格设计可复制到兄弟项目中
* **README 全面翻新：** 用户画像、JSON 示例输出、对比矩阵、可展开的 FAQ、双仓库文档
* **引用审查：** 跨越六个广告平台模块的 10 个量化声明，已针对主要供应商文档进行验证
* **项目信息部分：** 从 README 链接的 CHANGELOG、CONTRIBUTING、CODE OF CONDUCT、SECURITY、SUPPORT
* **双远程规范迁移：** 源仓库已迁移至 `AI-Marketing-Hub/claude-ads`，此仓库现为开源镜像
* **样式清理：** 删除文档中的 54 个长破折号，平台特性表更新为具体功能名称和日期，脚本权限标准化为 755
主要要点
* v1.7.1 是文档、品牌标识和信任信号。零功能变更。
* 跨越六个广告平台模块的 10 个量化声明现在引用了主要供应商文档。
* 动画 SVG 横幅大小为 13.9 KB，包含 22 个 SMIL 动画和屏幕阅读器标记。
* 删除了 54 个长破折号。样式指南现在由机器在 README 中强制执行。
* 规范远程仓库现为 AI-Marketing-Hub/claude-ads。AgriciDaniel 镜像保持同步。
## 为什么是精炼而非新功能
精炼，而非新功能。v1.7.1 没有新增任何功能性内容，每一项变更都是信任信号。
v1.7.0 是一个重大版本。八个平台、22 项子技能、209 项检查、41 项测试、六个 AI 助手宿主、完整的 Amazon 覆盖（包括 Sponsored Products、Brands 和 Display）。YouTube 上的发布演示视频展示了所有这些内容。
功能发布后，下一个瓶颈是信任。大多数付费广告工具是一个黑盒。你安装它，得到一个数字，然后只能凭信任接受。我希望恰恰相反。每一项检查都应该是可审查的。每一个基准都应该引用供应商来源。README 应该回答代理商在将工具交给客户账户之前实际会问的问题。
这就是 v1.7.1 的任务：锁定引用规范，发布一个与其他系列匹配的横幅，重写 README，使对比矩阵、用户画像和示例输出在首次滚动时就能呈现。
## 动画横幅
新横幅是视觉核心。13.9 KB。22 个 SMIL 动画。终端风格框架与 claude-seo、claude-blog 和 claude-canvas 一致。品牌套件位于 `~/Documents/Obsidian Vault/concepts/brand-design-system.md`，要求使用操作系统窗口边框、JetBrains Mono 标签、品牌橙色 `#D97757` 作为焦点元素，以及 4.2 秒呼吸渐变。v1.7.0 使用静态 PNG。v1.7.1 使用带有适当无障碍标记的动画 SVG。
参数化模板随 `branding/banner-template.html` 提供。搭配 `AGENT-PROMPT.md`，你可以通过一个提示和一个 JSON 参数文件为任何兄弟仓库重新生成相同的横幅。这就是关键：设计系统现在是可克隆的，而非定制的。
平台覆盖：Google、Meta、YouTube、LinkedIn、TikTok、Microsoft、Apple、Amazon。共计 209 项检查。
## 引用审查
十个量化声明，十个主要供应商来源。v1.7.1 中的每一个基准都追溯到出版商、标题、URL 和检索日期。
十个量化声明接受了凭据审查。任务很简单。检查描述中的每一个数字、每一个阈值、每一个基准、每一个“+18% 查询”或“560 亿零售媒体”都必须追溯到主要供应商来源。不是引用博客文章的博客文章。而是供应商文档本身，附带检索日期，放在参考文件中。
涉及的六个模块：ads-google、ads-meta、ads-apple、ads-microsoft、ads-budget、ads-creative。引用的供应商来源包括 Meta Engineering、Apple Developer 会话 221、Google Ads 博客、Microsoft Advertising 博客和 AppTweak。审计生命周期与之前相同。变化在于，当检查触发并显示“Meta 在规模化之前将近似重复项捆绑到拍卖层”时，底层阈值现在与 Meta 发布的文档相关联，而非猜测。
这就是 FLOW 证据三元组的具体实践：散文中年份锚点、带出版商和标题的行内引用、带检索日期的 URL。完整框架见 [github.com/AgriciDaniel/flow](https://github.com/AgriciDaniel/flow)。应用于广告审计，意味着客户可以问“这个数字从哪里来”并获得真实答案。v1.7.1 审查中引用的示例供应商来源包括 [Meta Engineering](https://engineering.fb.com/)、[Apple Developer WWDC25 会议](https://developer.apple.com/videos/play/wwdc2025/)、[Google Ads 博客](https://blog.google/products/ads-commerce/) 和 [Microsoft Advertising 博客](https://about.ads.microsoft.com/en/blog)。
引用审查结果：10 个主要来源，41 项测试通过，零功能漂移。
## 演示演练
v1.7 的讲解视频是了解产品最快的方式。流程如下：
* **首先阐述三大转变：** Amazon 零售媒体达到 560 亿美元，Meta 通过 Andromeda 重构了创意排名，Google 正在将账户迁移至 AI Max。大多数工具都忽略了这三者。claude-ads 覆盖了它们。
* **22 项子技能，一个编排器：** 说“审计我的 Meta 账户”，Meta 子技能就会加载。说“计划一个促销活动”，计划子技能就会加载。路由器负责调度。
* **一个脚本，六个宿主：** Claude Code、Codex CLI、Cursor、Windsurf、Gemini CLI、Goose。相同的技能，任何宿主。
* **41 项测试强制执行 209 项检查：** 运行相同的审计两次，获得相同的评分。这是大多数竞争对手缺失的信任信号。
* **预检 AI Max 审计：** Google 正在将搜索账户切换到 AI Max。历史广告活动在切换之前就会被捕获，而不是之后。
* **Andromeda 创意相似度：** Meta 在规模化之前限制近似重复项。审计在发布前根据视觉、标题、正文和钩子格式进行评分。
* **一次完成 Amazon 审计：** Sponsored Products、Sponsored Brands、Sponsored Display，一次审计运行全部覆盖。
架构本身保持不变。claude-ads 仍然提供 22 项子技能，跨越 8 个广告平台宿主（Google、Meta、YouTube、LinkedIn、TikTok、Microsoft、Apple、Amazon），由一个编排器连接起来。v1.7.1 围绕这个表面区域对文档和设计系统进行了精炼。它没有改变表面区域。
22 项子技能，跨越 8 个广告平台宿主，一个编排器。v1.7.1 中产品的形态。
## 适用人群
三个受众群体直接受益于 v1.7.1：
* **向客户提交审计报告的代理商：** 引用审查意味着 PDF 交付物中的每个基准现在都追溯到供应商文档。减少“这个数字从哪里来”的邮件。
* **正在争取新账户的独立顾问：** README 的全面翻新就是新的销售页面。用户画像、样本输出、对比矩阵、FAQ。发送 GitHub 链接，潜在客户自行阅读。
* **分支该技能的开发者：** 品牌套件意味着任何为私有代理商变体分支 claude-ads 的人都可以通过一个提示重新生成横幅。设计系统不再锁定在我的桌面上。
## 安装
与 v1.7.0 相同的安装命令。选择一个：
```
# Claude Code 插件
/plugin marketplace add AI-Marketing-Hub/claude-ads
# Unix 单行命令
curl -fsSL https://raw.githubusercontent.com/AI-Marketing-Hub/claude-ads/main/install.sh | bash
# Windows 单行命令
irm https://raw.githubusercontent.com/AI-Marketing-Hub/claude-ads/main/install.ps1 | iex
```
该技能适用于 Claude Code、Codex CLI、Cursor、Windsurf、Gemini CLI 和 Goose。所有地方使用相同的子技能。
## 下一步计划
v1.7.1 结束了 v1.7 周期。下一个队列：
* **第三波平台覆盖：** Walmart Connect 和 Connected TV。Walmart 是美国第二大零售媒体网络。CTV 是预算正在流向的地方。
* **v2 测量栈：** 营销组合建模、增量测试、PMax Feed 健康审计。检查目录保持不变。新增的是其上层，将检查与收入影响联系起来。
* **更多引用审查：** v1.7.1 审查击中了 10 个声明。还有更多。从今往后每次发布在打标签之前都会进行一次引用审查。
## 常见问题解答
### v1.7.1 是破坏性变更吗？
不是。零功能变更。相同的 22 项子技能、相同的 209 项检查、相同的 41 项测试、相同的评分算法。如果 v1.7.0 对你有效，v1.7.1 的运行方式完全相同。本次发布是文档、品牌标识和引用规范。
### 双仓库是什么情况？
规范源已迁移至 `AI-Marketing-Hub/claude-ads`。这是我用来向 Skool 社区分发 Pro 级工作的组织。`AgriciDaniel/claude-ads` 仓库作为开源镜像继续存在。两个仓库指向相同的代码。安装说明引用 AI-Marketing-Hub，因为这是首先发布版本的地方。
### 10 个已验证引用在哪里显示？
在六个广告平台模块（Google、Meta、Amazon、TikTok、LinkedIn、Microsoft）的检查描述中内联显示。每个量化声明现在都有出版商、标题、URL 和检索日期。完整参考列表位于技能的 `references/` 目录中。
### 动画横幅有多大？
13.9 KB。22 个 SMIL 动画。总文件大小低于品牌设计系统强制要求的 100 KB 图片预算。横幅使用 `role="img"` 和描述性元素以实现屏幕阅读器兼容性。
### 我可以将品牌套件用于自己的项目吗？
可以。`branding/banner-template.html` 是参数化的。搭配 `branding/AGENT-PROMPT.md`，你可以为任何兄弟仓库重新生成匹配的横幅。该套件与技能的其他部分一样采用 MIT 许可。
### 该工具仍然免费吗？
是的。MIT 许可，无使用限制，无高级版本。你需要 Claude Code 订阅，因为技能运行在 Claude Code 内部，但技能本身是免费且开源的。没有托管版本。
## 相关文章
* [claude-ads v1.5：跨 7 个平台的 250+ 广告审计检查](/blog/claude-ads-v1-5-release) - v1.5 版本，包含首个 PDF 报告生成器
* [Claude Code 刚刚取代了你的广告代理商](/blog/claude-code-ad-agency) - 关于定位和范围的最初产品文章
* [AI 营销自动化：我日常使用的开源栈](/blog/ai-marketing-automation-stack) - claude-ads 在完整栈中的位置
* [claude-seo v1.9.6：FLOW 框架安全加固](/blog/claude-seo-v196-flow-security-hardening) - 推动 v1.7.1 引用审查的 FLOW 证据引用框架
* [2026 年最佳 Claude Code 技能](/blog/best-claude-code-skills-2026) - claude-ads 与其余 Claude Code 技能生态系统的比较
在 GitHub 上阅读完整的 [v1.7.1 发布说明](https://github.com/AgriciDaniel/claude-ads/releases/tag/v1.7.1)，如果对你有所帮助请给仓库加星，如果有任何问题请提交 issue。了解更多[关于我](/about)以及我构建的其他开源 AI 营销栈的信息。
加入 4,500+ AI 营销构建者
工作流模板、审计手册以及一个由 SEO、代理商所有者和 PPC 顾问组成的社区，他们不断发布产品。
[免费加入](https://www.skool.com/ai-marketing-hub)[升级专业版](https://www.skool.com/ai-marketing-hub-pro)
