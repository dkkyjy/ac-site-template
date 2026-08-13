---
title: "claude-music：在终端中使用 ACE-Step 1.5 生成完整歌曲"
description: "---"
pubDate: "2026-04-22"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-music-ai-production"
---

```markdown
# claude-music：在终端中使用 ACE-Step 1.5 生成完整歌曲

---

## 在所有基准测试中击败 Suno 和 Udio 的开源模型
2024 年，音乐领域的生成式 AI 市场规模达到 5.697 亿美元，并以 30.4% 的年增长率增长，到 2030 年预计将达到 28 亿美元（[Grand View Research](https://www.grandviewresearch.com/industry-analysis/generative-ai-in-music-market-report)，2024）。Suno、Udio 及其云端竞争对手正在乘着这波浪潮——每月收费 10-20 美元，让你访问无法审查的模型，将你的音频存储在自己的服务器上，并随时更新其服务条款。与此同时，[ACE-Step 1.5](https://github.com/ace-step/ACE-Step-1.5) —— 一个完全开源的音乐生成模型——在每一个已发布的基准测试中均优于这两者，它运行在你的 GPU 上，并且完全免费。
我将其封装成了一个 Claude Code 技能。[claude-music](https://github.com/AgriciDaniel/claude-music) 是一个包含 10 个命令的 AI 音乐制作系统，完全在你的终端中运行。根据文本描述生成完整歌曲。通过风格转换重新混音曲目。使用 LoRA 在你自己的音乐目录上进行微调。导出适用于 Spotify、YouTube 或 TikTok 的平台就绪文件。一切都保留在本地。无需账户，无需月费，你的数据不会离开你的机器。
关键要点
* ACE-Step 1.5-XL 在全部 4 个已发布的基准测试中均胜过 Suno v5 和 Udio v1.5（[ACE-Step](https://ace-step.github.io/ace-step-v1.5.github.io/)，2025 年 4 月）
* 在 A100 上生成完整歌曲不到 2 秒，在 RTX 3090 上不到 10 秒
* 86% 的全球创作者已经在内容工作中使用生成式 AI（[Adobe Creators' Toolkit](https://news.adobe.com/news/2025/10/adobe-max-2025-creators-survey)，2025 年 10 月）——音乐是最后的空白
* 10 个子技能：generate（生成）、cover（翻唱）、repaint（重绘）、compose（作曲）、export（导出）、analyze（分析）、enhance（增强）、random（随机）、library（库）、lora（LoRA）
* 在 RTX 3090 上对 3-10 首你自己的歌曲进行 LoRA 微调，大约需要 1 小时
## 什么是 ACE-Step 1.5？为什么它比付费工具更好？
ACE-Step 1.5-XL 在 AudioBox 上得分为 7.76，在 SongEval 上得分为 8.12，在风格对齐上得分为 6.62，在歌词对齐上得分为 8.42——全面击败 Suno v5（7.69、7.87、6.51、8.29）和 Udio v1.5（7.45、7.65、6.15、8.03）（[ACE-Step 官方](https://ace-step.github.io/ace-step-v1.5.github.io/)，2025 年 4 月）。它是一个基于扩散的模型——与引爆图像生成的架构相同——应用于音频生成并经过专门的音乐训练。
ACE-Step 1.5-XL vs Suno v5 vs Udio v1.5 – 基准测试分数
分组柱状图。AudioBox：7.76 vs 7.69 vs 7.45。SongEval：8.12 vs 7.87 vs 7.65。风格对齐：6.62 vs 6.51 vs 6.15。歌词对齐：8.42 vs 8.29 vs 8.03。ACE-Step 在所有四项上领先。
ACE-Step 1.5-XL vs Suno v5 vs Udio v1.5
基准测试分数（越高越好）——ACE-Step 官方，2025 年 4 月
ACE-Step 1.5-XL
Suno v5
Udio v1.5
5.5
6.0
6.5
7.0
7.5
8.0
8.5
7.76
7.69
7.45
8.12
7.87
7.65
6.62
6.51
6.15
8.42
8.29
8.03
AudioBox
SongEval
风格对齐
歌词对齐
来源：ACE-Step 官方基准测试，2025 年 4 月
ACE-Step 1.5-XL 在所有四个基准测试中均领先——同时击败了 Suno v5 和 Udio v1.5
技术架构分为两个层级。Turbo 模型有 2B 参数，在 8 个扩散步骤内完成，最低可在 4GB VRAM 上运行——即 RTX 3060 或更高。XL 模型有 4B 参数，bf16 权重约需 9GB，最高质量设置需要约 16GB VRAM，并且是上面产生基准领先结果的原因。两个层级都支持 10 秒到 10 分钟的音频，1000+ 种乐器和风格，同时最多批量生成 8 首歌曲，以及 50+ 种语言的人声。
据 ACE-Step 团队称，在 A100 上生成一首完整歌曲不到 2 秒，在 RTX 3090 上不到 10 秒（[ACE-Step GitHub](https://github.com/ace-step/ACE-Step-1.5)，2025 年 4 月）。多语言支持的重要性比听起来更大——大多数 AI 音乐工具在处理非英语歌词时表现不佳。如果你正在为韩语、日语、西班牙语或法语受众创作内容，ACE-Step 1.5 可以处理它，而不会出现你在其他地方遇到的尴尬语音伪影。
## 10 个子技能：你实际上能做什么？
84% 的开发人员正在使用或计划在其开发过程中使用 AI 工具，但 66% 的人将“几乎正确但不完全正确”的 AI 解决方案列为最大的挫折（[Stack Overflow 开发者调查](https://survey.stackoverflow.co/2025/ai)，2025）。claude-music 的 10 个子技能旨在为你提供精确的控制，而不是希望模型正确猜测你的意图。
| 命令 | 功能 |
| --- | --- |
| `/music generate` | 根据文本描述 + 可选歌词创作音乐。指定时长、语言、质量预设。 |
| `/music cover` | 风格迁移。用不同的流派或风格重制参考曲目。 |
| `/music repaint` | 编辑歌曲的特定部分。指定时间戳范围并描述要更改的内容。 |
| `/music compose` | 歌曲创作辅助：歌词、说明建议、BPM 和调性推荐。 |
| `/music export` | 针对平台的优化导出。处理 Spotify、YouTube、TikTok、播客、CD 的响度标准化。 |
| `/music analyze` | 检查任何音频文件的 BPM、调性、响度水平和频谱。 |
| `/music enhance` | 标准化电平、降噪、分离音轨（人声、鼓、贝斯、其他）。 |
| `/music random` | 随机流派和风格生成。在你不知道想要什么时很有用。 |
| `/music library` | 浏览和搜索你生成的音乐。按流派、BPM、日期或关键词过滤。 |
| `/music lora` | 在 3-10 首你自己的歌曲上微调 ACE-Step，以创建自定义风格检查点。 |
其中有三个值得特别关注。**repaint**（重绘）是 claude-music 与“生成后碰运气”的区别所在——如果副歌完美但前奏拖沓，你可以描述时间戳范围以及你希望更改的内容。这是段落级别的控制，而不是完全重新生成。**lora**（LoRA）为你提供了一个持久的风格指纹——训练一次，在以后的每次生成中使用它。**export**（导出）处理了大多数创作者会出错的烦人的响度标准化步骤：Spotify 目标 -14 LUFS，YouTube -13 LUFS，TikTok -14 LUFS。一个命令，正确输出。
**来自 claude-music 的测试：** 标准质量（默认，每次生成约 15 秒）产生的输出，对于 YouTube 背景音乐或播客开场来说，首次尝试就足够好的概率约为 80%。另外 20% 的情况需要对前奏或第一段副歌进行一次 `/music repaint` 操作。最高质量将首次尝试成功率提高到大约 92%——代价是每次生成需 3-5 分钟。对于大多数使用场景，标准质量是正确的起点。在最终定稿时使用最高质量。
## LoRA 微调是如何工作的？
ACE-Step 1.5 支持在 RTX 3090 上对 3-10 首歌曲进行 LoRA 微调，大约需要 1 小时（[ACE-Step GitHub](https://github.com/ace-step/ACE-Step-1.5)，2025 年 4 月）。这使得 claude-music 对于专业用途真正有用，而不仅仅是实验。工作流程：
```
# 步骤 1：指向你的训练歌曲
/music lora --train ./my-songs/ --name my-style
# 步骤 2：等待约 1 小时（RTX 3090）
# 训练器在本地运行，不会上传任何内容
# 步骤 3：在以后的每次生成中使用你的检查点
/music generate --caption "upbeat summer pop" --lora my-style --duration 60
```
3-10 首歌曲的要求并非随意。少于 3 首会导致模型记忆而非学习风格——每次生成听起来都与你特定的曲目过于相似。超过 10 首则会显著延长训练时间，且质量收益递减。对于大多数艺术家来说，最佳歌曲数量是 5-8 首，这些歌曲代表了他们预期输出风格的范围。
这在实际应用中能解锁什么？播客制作人可以训练现有的开场音乐，并自动生成特定剧集的变体，这些变体保持品牌一致。YouTube 创作者可以为频道建立标志性声音，并为每个视频生成匹配的背景曲目。游戏开发者可以训练环境音频调色板，并生成适合的新曲目，而无需手动进行 A/B 测试与现有混音进行比较。关键点：你的自定义风格保留在你的硬件上。没有其他人基于你的音乐进行训练，没有 API 依赖，意味着无需担心训练数据政策变化。
## 为什么要在本地运行音乐 AI 而不是使用 Suno 或 Udio？
86% 的全球创作者现在在其内容工作中使用生成式 AI，81% 的人表示这有助于他们制作原本无法制作的内容（[Adobe Creators' Toolkit 报告](https://news.adobe.com/news/2025/10/adobe-max-2025-creators-survey)，2025 年 10 月）。但云端工具除了月费外还有隐藏成本：你的输出、你的风格指纹以及你的训练数据都保存在别人的服务器上。音乐许可已经够复杂了——你不需要在法律责任范围内增加“我的云提供商可能对此有主张”这种问题。
使用 claude-music，音频文件保存在你的磁盘上。模型权重保存在你的磁盘上。你训练的 LoRA 检查点保存在你的磁盘上。生成或训练过程中不会上传任何内容。版权情况与你自行制作音频时完全一样清晰——对于 AI 生成的内容，这在大多数司法管辖区仍是一个未解决的问题，但至少这是你和相关法律之间的事，而不是你和服务条款更新之间的事。
以下是大多数关于 AI 音乐工具的报道忽略的一点：最有价值的用例不是取代专业音乐人。而是让开发者、营销人员和创作者无需订阅或数字音频工作站就能获得制作质量的背景音频。从 claude-music 中获益最多的人不是希望自动化工作流程的制作人——而是那个需要为应用演示准备三首不同变体的低保真节拍、却不想花 40 美元购买素材库授权的开发者，或者是那个需要为本地化视频广告准备 12 个地区性背景音乐变体的营销人员。
2024-2030 年音乐领域生成式 AI 市场增长
面积图显示音乐领域生成式 AI 市场从 2024 年的 5.697 亿美元增长到 2030 年预计的 28 亿美元，年复合增长率为 30.4%。
AI 音乐市场增长
5.697 亿美元（2024 年）到 28 亿美元（2030 年*），年复合增长率 30.4%——Grand View Research，2024
$0
$1B
$2B
$570M
$2.8B
2024
2025
2026
2027
2028
2029
2030\*
\* 按 30.4% 的年复合增长率预测。中间值为外推值。来源：Grand View Research，2024。
AI 音乐市场到 2030 年增长近 5 倍——目前大部分增长流向云平台
## 如何在 5 分钟内安装 claude-music
安装程序会处理一切：通过 uv 设置 Python 环境、FFmpeg 以及 ACE-Step 模型（约 5GB，下载时会有明确确认）。你需要 Claude Code 和至少 4GB VRAM 的 GPU。以下是完整设置：
```
# 步骤 1：克隆仓库
git clone https://github.com/AgriciDaniel/claude-music.git
cd claude-music
# 步骤 2：运行安装程序
bash install.sh           # Linux / macOS
# 或者
powershell -ExecutionPolicy Bypass -File .\install.ps1   # Windows
```
安装程序会检测你的 GPU 和 VRAM，告知你可用的质量预设，在你确认后下载 ACE-Step，并运行测试生成以验证设置是否正常工作。之后无需手动配置任何内容。
安装完成后，打开 Claude Code 并尝试：
```
/music generate "chill lo-fi beat, 60 seconds"
/music generate --caption "upbeat pop, female vocal" --duration 90 --quality high
/music random
```
30 多个内置流派配方为你处理了提示工程。你不需要知道一首低保真节拍需要“boom bap 鼓、黑胶噼啪声、罗德钢琴、闷音吉他”——输入“lo-fi beat”就会激活配方，自动提供最佳的标题结构、BPM 范围和乐器权重。
**来自构建此工具的经验：** 最难的工程问题不是模型集成——而是 VRAM 检测。不同的 GPU 代次报告可用内存的方式不同，在 4GB、8GB 和 16GB 边界处加载行为会发生显著变化。`detect_gpu.sh` 脚本经历了六次迭代，才在 RTX 3060、3090 和 4090 硬件上可靠地给出正确的质量预设建议。如果你遇到预设不匹配的情况，运行 `/music setup` 会重新运行检测并重置配置。
## 常见问题解答
### 我需要一块强大的 GPU 来运行 claude-music 吗？
最低要求是 4GB VRAM——即 RTX 3060 或同等产品。在 4GB 下，你可以使用标准质量的 Turbo 模型。8GB 可解锁 Turbo 的扩展思考功能，以获得更好的结构。16GB 以上可让你以完整质量运行 XL 模型——也就是上面基准测试分数所基于的模型。仅 CPU 模式也可以工作，但生成需要 5-10 分钟而不是几秒钟。
### 输出质量实际上与 Suno 或 Udio 相比如何？
在四个已发布的基准测试中，ACE-Step 1.5-XL 在 AudioBox、SongEval、风格对齐和歌词对齐上的得分均高于这两项服务（[ACE-Step 官方](https://ace-step.github.io/ace-step-v1.5.github.io/)，2025 年 4 月）。XL 模型的人声清晰度和歌词连贯性是相对于标准质量最显著的改进——特别是在英语以及具有清晰主歌和副歌的复杂歌曲结构中。
### 我可以将生成的音乐用于商业用途吗？
claude-music 技能和 ACE-Step 模型权重均以 MIT 许可证发布。AI 生成的音频用于商业用途的法律状态因司法管辖区而异，并且持续演变——这里的情况与任何其他 AI 内容工具相同。除 MIT 许可证和模型自身条款外，没有添加额外的限制。
### 导出命令生成哪些音频格式？
WAV（无损，用于制作工作流程）、MP3（标准分发）以及具有适当响度标准化的平台特定导出：Spotify（-14 LUFS）、YouTube（-13 LUFS）、TikTok（-14 LUFS）、播客（-16 LUFS）和 CD（-23 LUFS）。`/music analyze` 命令可以在上传前验证输出是否满足目标规格。
### 它在 Windows 上能工作吗？
可以——PowerShell 安装程序（`install.ps1`）在 Windows 上处理完整设置。该技能在 Claude Code 运行的任何地方都有效：CLI、桌面应用（Mac 和 Windows）以及 VS Code 扩展。Windows 安装程序需要开发人员模式或管理员权限才能创建必要的符号链接。
## 在终端中构建你的音频堆栈
AI 音乐市场正以 30.4% 的年增长率增长，并且大部分增长流向封闭的、云端锁定的平台。ACE-Step 1.5 是第一个在质量上达到——并且在基准测试中超越——它们的开源模型。claude-music 在其之上构建了一个完整的制作系统：生成、混音、微调、分析和导出，所有这些都通过一个终端命令完成。
10 个子技能。30 多个流派配方。LoRA 微调。平台导出。零订阅。
* 在 [GitHub](https://github.com/AgriciDaniel/claude-music) 上给仓库加星
* 在[关于页面](/about)上查看我构建的其他内容
* 将其与 [claude-canvas](/blog/claude-canvas-ai-visual-production) 配对，在同一个终端中进行视觉和音频制作
* 通过 [Skill Forge](/blog/skill-forge-build-claude-code-skills) 了解更多关于构建此类工具的信息
加入 4,500+ AI 营销建设者
工作流模板、自动化蓝图以及一个由 SEO、代理商所有者以及创作者组成的社区。
[立即免费加入 →](https://www.skool.com/ai-marketing-hub)
## 相关文章
* [Claude Code 刚刚将 Obsidian Canvas 变成了 AI 设计工作室](/blog/claude-canvas-ai-visual-production) —— claude-music 的视觉制作伴侣
* [2026 年最佳 Claude Code 技能](/blog/best-claude-code-skills-2026) —— 节省时间的技能的权威排名
* [使用 Skill Forge 构建你自己的 Claude Code 技能](/blog/skill-forge-build-claude-code-skills) —— 从想法到发布技能，一次会话完成
* [Obsidian AI 第二大脑](/blog/claude-obsidian-ai-second-brain) —— 与音频堆栈配合良好的知识引擎
```
