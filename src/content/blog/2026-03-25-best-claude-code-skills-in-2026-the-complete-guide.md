---
title: "2026 年最佳 Claude Code 技能 —— 完全指南"
description: "---"
pubDate: "2026-03-25"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/best-claude-code-skills-2026"
---

# 2026 年最佳 Claude Code 技能 —— 完全指南

---

Claude Code 在 2025 年末推出了技能支持，此后几个月内，围绕它爆发出了一个完整的生态系统。现在已有数百个社区构建的技能，涵盖从 SEO 审计到视频编辑再到广告账户分析的方方面面。**问题在于，弄清楚哪些技能真正值得安装。** 我测试了其中大部分（我自己构建了几个顶级技能），这篇指南正是我当初开始时所希望拥有的。

## 什么是 Claude Code 技能？

如果你是新手：Claude Code 是 Anthropic 为 Claude 推出的官方 CLI。你安装它，在终端中运行，它可以读取你的代码库、执行命令、编辑文件并与 API 交互。技能是赋予 Claude Code 专业能力的扩展。**可以把它们想象成插件，将通用型 AI 助手转变为领域专家。**

一个技能就是一个 markdown 文件（或一组文件），存放在项目的 `.claude/skills/` 目录中。当 Claude Code 加载你的项目时，它会读取这些技能文件，从而获得其中定义的指令、工作流程和工具调用模式。无需编译。无需构建步骤。你只需把一个文件放入文件夹，Claude Code 就会变得更聪明。

技术实现非常优雅。每个技能定义一个触发方式（通常是像 `/seo-audit` 这样的斜杠命令）、一组 Claude 遵循的指令，以及一些可选配置。当你调用该技能时，Claude Code 会利用其现有能力执行这些指令：读取文件、运行 shell 命令、调用 API 以及写入输出。技能只是告诉它做什么以及按什么顺序做。

## 如何安装 Claude Code 技能

安装很简单。大多数技能以 GitHub 仓库的形式分发。以下是通用流程：

```
# 克隆技能仓库（或仅克隆技能文件）
git clone https://github.com/AgriciDaniel/claude-seo.git
# 将技能文件复制到你的项目中
cp -r claude-seo/.claude/skills/seo your-project/.claude/skills/
# 就这样。在你的项目中启动 Claude Code
claude
```

**一旦技能文件位于你的 `.claude/skills/` 目录中，Claude Code 会自动识别它们。** 如果你在 Claude Code 运行时添加技能，无需重启（它会监视文件更改）。某些技能还包含一个 `CLAUDE.md` 文件，其中包含项目级指令，你可以将其合并到自己的项目中。

少数技能需要外部依赖（API 密钥、CLI 工具等）。这些总是在技能的 README 中记录。例如，claude-seo 需要 Node.js 来运行其部分分析工具，claude-ads 需要对你广告平台的 API 访问权限。

## 按 GitHub 星标数排名的顶级 Claude Code 技能

我按 GitHub 星标数排名，因为它是最接近社区投票的指标。星标数并非完美指标（很多优秀的工具星标数偏低），但对于一份“最佳”列表而言，这是最客观的起点。所有星标数截至 2026 年 3 月。

### 1. claude-seo – 2,974 颗星

*深入阅读：[Claude Code 刚刚取代了你的整个 SEO 工具栈](/blog/claude-code-seo-stack)*

**现存星标数最高的 Claude Code 技能，没错，是我构建的。**（偏见已披露。继续。）

claude-seo 将 Claude Code 转变为一个全面的 SEO 审计工具。你指向一个 URL 或本地项目，它就会运行完整的技术 SEO 审计：可爬取性、索引问题、元标签分析、标题层级、结构化标记验证、核心网页指标评估、内部链接分析以及内容优化评分。

主要功能：
* 完整技术 SEO 审计，包含 50 多项检查点
* 内容优化评分，附带具体修复建议
* 结构化标记生成与验证（Article、FAQ、HowTo、Product 等）
* 关键词密度与布局分析
* 竞争对手内容缺口分析
* 内部链接建议
* 核心网页指标诊断
* 批量 URL 处理，用于全站审计

它与付费工具的区别不在于某一项功能——而在于一切都在你的终端本地运行，没有数据限制，没有 API 密钥墙（核心功能），也没有月度订阅。它采用 MIT 许可证，永久免费。

```
# 安装
git clone https://github.com/AgriciDaniel/claude-seo.git
# 在任何项目中使用
/seo-audit https://example.com
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/claude-seo)

### 2. claude-ads – 1,171 颗星

*深入阅读：[Claude Code 刚刚取代了你的广告代理公司](/blog/claude-code-ad-agency)*

claude-ads 对广告的作用正如 claude-seo 对搜索的作用。**它审计你在 Google Ads、Meta Ads 和 TikTok Ads 上的广告账户，然后给出具体、可操作的建议。** 不是模糊的“优化你的定向”这类建议。而是具体的，比如：“活动 X 在移动端的点击率为 4.2%，但在桌面端仅为 1.1%——暂停桌面端或创建针对不同设备的素材。”

主要功能：
* 多平台支持：Google Ads、Meta Ads、TikTok Ads
* 预算浪费检测（识别那些烧钱却无转化的活动）
* 受众重叠分析
* 创意效果评分
* 出价策略优化建议
* ROAS 和 CPA 基准对比行业平均水平
* 导出审计报告为结构化文档

本技能需要你的广告平台的 API 凭据。如果你已经有 API 访问权限，设置大约需要 10 分钟；如果需要创建开发者账户，则需更长时间。

```
# 安装
git clone https://github.com/AgriciDaniel/claude-ads.git
# 审计一个 Google Ads 账户
/ads-audit  - platform google  - account-id 123-456-7890
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/claude-ads)

### 3. claude-blog – 300 颗星

*深入阅读：[Claude Code 刚刚取代了你的博客写手](/blog/claude-code-blog-writer)*

claude-blog 是一个内容创作技能，能生成从一开始就针对 SEO 优化的博客文章。你给它一个主题和目标关键词，它就会生成一篇完整的文章，包含恰当的标题层级、关键词布局、元标签和结构化标记。**它本质上是从 Rankenstein 中提取出来的写作引擎，封装成了独立的 Claude Code 技能。**

主要功能：
* 根据单个关键词生成 SEO 优化的文章
* 可配置的语气和声音档案
* 自动标题层级（H1 到 H4）
* 元标题和描述生成
* 结构化标记输出（Article、BlogPosting）
* 基于你现有内容的内部链接建议
* 可读性评分与调整

声音档案系统是这款技能的特别之处。你可以定义自己的写作风格（句子长度、词汇水平、语气以及要避免的特定词语），claude-blog 会一致地匹配它。我用它来写初稿，然后编辑以加入个人特色。大约节省 60% 的写作时间。

```
# 安装
git clone https://github.com/AgriciDaniel/claude-blog.git
# 生成一篇博客文章
/blog-write  - keyword "n8n 自动化"  - tone professional
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/claude-blog)

### 4. banana-claude – 41 颗星

*深入阅读：[banana-claude：在 Logo 上表现出奇好的 AI 图像生成](/blog/banana-claude-ai-image-generation)*

banana-claude 为 Claude Code 添加了图像生成能力。它集成了多个图像生成 API（Stable Diffusion、DALL-E、Midjourney 的 API），让你可以直接从终端生成、编辑和批量处理图像。**它特别适合在不离开工作流的情况下生成博客特色图片和社交媒体素材。**

主要功能：
* 多提供商支持（Stable Diffusion、DALL-E、Flux）
* 带可变提示的批量生成
* 图像到图像编辑
* 针对不同平台（博客、Twitter、LinkedIn、Instagram）的自动调整大小
* 提示优化（重写你的提示以获得更好结果）

```
# 安装
git clone https://github.com/AgriciDaniel/banana-claude.git
# 生成一张图片
/banana  - prompt "极简科技博客头部，深色背景"  - size 1200x630
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/banana-claude)

### 5. claude-shorts – 27 颗星

claude-shorts 可实现短视频自动编辑。你输入一段长视频，它能识别出最吸引人的片段，进行剪辑，添加字幕，并以针对 YouTube Shorts、TikTok 和 Instagram Reels 优化的格式导出。**如果你要将长内容重新利用为短视频，这款技能可将编辑时间从几小时缩短到几分钟。**

主要功能：
* 长视频中的自动精彩片段检测
* AI 驱动的字幕生成与样式设置
* 宽高比转换（16:9 转 9:16）
* 多平台导出预设
* 多个视频的批量处理

需要本地安装 FFmpeg（如果你做任何视频工作，很可能已经安装了）。

```
# 安装
git clone https://github.com/AgriciDaniel/claude-shorts.git
# 处理一个视频
/shorts  - input video.mp4  - max-clips 5  - captions true
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/claude-shorts)

### 6. skill-forge – 23 颗星

*深入阅读：[skill-forge：构建你自己的 Claude Code 技能](/blog/skill-forge-build-claude-code-skills)*

skill-forge 是元工具。它是一个用于构建 Claude Code 技能的 Claude Code 技能。你描述你希望技能做什么，skill-forge 就会生成技能文件、目录结构、斜杠命令配置和 README。**这是从“我希望 Claude Code 能做 X”到项目中存在一个可用技能的最快途径。**

主要功能：
* 交互式技能脚手架
* 自动斜杠命令注册
* 最佳实践文件结构生成
* 技能测试与验证
* README 与文档生成
* GitHub 仓库模板设置

```
# 安装
git clone https://github.com/AgriciDaniel/skill-forge.git
# 创建一个新技能
/forge  - name "my-skill"  - description "做某件很酷的事"
```

[在 GitHub 上查看](https://github.com/AgriciDaniel/skill-forge)

## 如何构建你自己的 Claude Code 技能

如果现有技能都无法满足你的需求（或者你想定制一个），自己构建其实出奇地简单。以下是最小可行技能：

```
# 1. 创建技能目录
mkdir -p your-project/.claude/skills/my-skill
# 2. 创建技能文件
cat > your-project/.claude/skills/my-skill/skill.md << 'EOF'
---
name: My Skill
command: /my-skill
description: 做我需要它做的事
---
## 指令
当用户调用 /my-skill 时，请执行以下操作：
1. 读取当前目录结构
2. [在此处输入你的具体指令]
3. 以 [格式] 输出结果
## 规则
- 始终 [约束条件]
- 从不 [反模式]
EOF
```

**这就是一个可用的技能。** markdown 前言定义了命令触发器和元数据。主体部分包含 Claude Code 在技能被调用时遵循的指令。你可以让这些指令根据需要简单或复杂。

对于更高级的技能，你可以：
* 将指令拆分到多个 markdown 文件中
* 包含示例输入和输出用于少样本学习
* 定义用户可自定义的配置选项
* 引用 Claude Code 应调用的外部工具和 API
* 将多个技能链接成一个工作流

或者直接使用 skill-forge，跳过手动设置。它可以根据你想要的描述生成所有这些脚手架。

## 生态系统正在快速增长

当我在 2025 年 8 月发布 claude-seo 时，总共大约只有十几个 Claude Code 技能。截至 2026 年 3 月，我仅在 GitHub 上就统计到超过 300 个。**增长速度正在加快，因为构建技能几乎零摩擦——如果你能写出清晰的 markdown 文档，你就能构建一个技能。**

目前最活跃的发展领域是：
* **营销与 SEO**（显然是老本行）——审计工具、内容生成器、分析集成
* **DevOps 与基础设施**——部署自动化、监控、事件响应
* **数据分析**——CSV 处理、数据库查询、可视化生成
* **内容创作**——写作、图像生成、视频编辑
* **代码质量**——代码审查自动化、测试、文档生成

我预计这份清单在 6 个月内会完全不同。工具仍然年轻，社区正在积极探索。

## 常见问题解答

### Claude Code 技能需要付费吗？

技能本身是免费的（大多数是开源的，采用 MIT 许可证）。但是，Claude Code 本身需要 Anthropic API 密钥或 Claude 订阅，部分技能会调用外部 API，可能产生各自费用。例如，claude-seo 的核心功能完全免费，但如果你需要 DataForSEO 关键词数据集成，则需要一个 DataForSEO API 密钥。**技能总会预先告知你需要哪些外部依赖项。**

### 我能在同一个项目中使用多个技能吗？

是的。技能设计为可以共存。你可以在同一个项目中安装 claude-seo、claude-blog 和 banana-claude，并独立使用它们。唯一潜在的冲突是两个技能定义了相同的斜杠命令，这种情况很少见，通过重命名其中一个即可轻松解决。

### 技能与 MCP（模型上下文协议）服务器相比如何？

它们解决不同的问题。MCP 服务器为 Claude Code 提供对外部数据源和 API（数据库、SaaS 工具、文件系统）的访问。技能为 Claude Code 提供专业化的工作流程和指令。你经常会同时使用两者。例如，claude-ads 使用 MCP 连接从你的广告平台拉取数据，然后技能指令告诉 Claude 如何分析这些数据并生成建议。**MCP 是管道。技能是上层的智能层。**

### 了解新技能的最佳方式是什么？

[Skool 上的 AI 营销中心](https://www.skool.com/ai-marketing-hub) 是我首发新技能的地方。GitHub 上“claude-code”主题的探索页面是另一个好来源。如果你自己正在构建技能，[skill-forge 仓库](https://github.com/AgriciDaniel/skill-forge) 有一个社区部分，人们会在那里分享他们正在做的东西。

## 下一步计划

我目前正在开发两个新技能，将在未来几周内公布。一个专注于邮件营销自动化，另一个是竞争情报工具，其深度远超 claude-seo 目前提供的功能。如果你想抢先体验，请加入 [AI 营销中心](https://www.skool.com/ai-marketing-hub)——高级会员可以优先获得我发布的所有内容。

**Claude Code 技能生态系统是当前 AI 工具领域最令人兴奋的事情。** 不是因为某个单一技能具有革命性，而是因为构建和分享专业化 AI 工作流的门槛已经降到几乎为零。如果你能用简单的英语描述你想要的东西，你就能构建一个实现它的工具。这意义重大。

## 相关文章

* [Claude Code 刚刚取代了你的整个 SEO 工具栈](/blog/claude-code-seo-stack) —— 如何用一个终端命令取代每月 300 美元的 SEO 工具
* [skill-forge：构建你自己的 Claude Code 技能](/blog/skill-forge-build-claude-code-skills) —— 在几分钟内构建并发布 Claude Code 技能，而不是几小时
* [AI 营销自动化：我每日使用的开源工具栈](/blog/ai-marketing-automation-stack) —— 完整开源 AI 营销工具栈，每月仅需 50 美元

加入 4,500+ AI 营销构建者
获取工作流模板、自动化蓝图，与 SEO、代理机构拥有者和创作者交流。
[免费加入 →](https://www.skool.com/ai-marketing-hub)
