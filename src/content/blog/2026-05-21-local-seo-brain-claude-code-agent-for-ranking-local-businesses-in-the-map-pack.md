---
title: "Local SEO 大脑：用于在本地地图包中提升本地企业排名的 Claude Code 智能体"
description: "---"
pubDate: "2026-05-21"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-code-local-seo-brain"
---

# Local SEO 大脑：用于在本地地图包中提升本地企业排名的 Claude Code 智能体

---

## 什么是 Local SEO 大脑？
Local SEO 大脑是一个带来源引用的 Obsidian 操作系统大脑 + AI 智能体技能包，用于在 Google 地图包、本地自然搜索结果和 AI 搜索中提升本地企业排名。它包含 59 个知识库章节、37 个可复用的 AI 提示词、31 张参考图片和 1,271 个维基链接，外加一个通过 Claude Code、Codex 或 Gemini 运行的审批优先智能体脚本层。每个客户设置约需 30 分钟。之后智能体将你的 GBP 导出数据、引文抓取数据、评论反馈和地理网格扫描数据整合到一个 Obsidian 记忆中，并综合生成一份优先级排序的审计报告——每条建议都链接回其来源的原始导出文件。
观看完整的 6 分钟操作演示：
Local SEO 大脑在 YouTube 上——由 Daniel Agrici 制作的 6 分 18 秒演示。
## 问题：每个客户需要 60 多个小时的电子表格审计
大多数本地 SEO 机构每次客户审计收费 60 小时或更多。工作量是真实的——来自 Google Business Profile 的 GBP 导出数据、来自 Whitespark 或 BrightLocal 的引文抓取数据、来自 Local Falcon 的地理网格扫描数据、评论反馈、页面审计、外链映射——而其中大部分最终散落在 Google 表格和截图中。而“综合”工作则是在周二电话会议上发生在某个人的头脑中。
通用 AI 工具无法解决这个问题。ChatGPT 会幻觉出 GBP 指标。Claude 没有在你的另一个标签页中打开你的 Local Falcon 导出文件。Gemini 会愉快地发明出不存在的反向链接。缺失的是一个结构化的操作系统大脑，它能吸收真实的本地 SEO 数据，并给 AI 智能体提供编写有用审计所需的上下文。
碎片化的电子表格和截图变成了一个带来源引用的 Obsidian 记忆。
## 盒子里有什么
Local SEO 大脑在一个捆绑包中提供两件产物。第一个是面向买家的 Obsidian 保管库，其中预装了完整的本地 SEO 知识库——涵盖 3 个阶段的 59 个章节、Core 30 内容策略、GBP 操作手册、审计、引文、反向链接、E-E-A-T 和 LLM SEO。第二个是面向智能体的操作层：一个 SKILL.md 契约加一个 scripts/ 文件夹，用于搭建每个客户的保管库、吸收原始数据并综合生成带来源引用的审计报告。
* **assets/template-brain/** — 遵循 Hot/Index/Wiki 模式的 Obsidian 保管库，预装了 3 个阶段、Core 30 内容策略、GBP 优化手册、涵盖属性/内容/技术/点击率的审计、本地引文、反向链接、E-E-A-T 和 LLM SEO。包含一个 2026 年新鲜度覆盖层，因此过时的章节会带有警告标注和当前状态链接。
* **SKILL.md + scripts/** — 面向智能体的操作层。搭建每个客户的保管库、幂等地重新播种知识库、吸收原始来源（GBP、引文、评论、地理网格）、综合生成带来源引用的审计 + 行动路线图、检查图结构并生成客户就绪的可交付成果。V1 版本零 Python 依赖——运行在标准库上。
预播种的保管库——200 多个节点、1,271 个维基链接，按文件夹颜色编码。
## 工作原理：Karpathy 的 Hot/Index/Wiki 记忆模式
Local SEO 大脑搭建的每个保管库都遵循 Andrej Karpathy 的三层上下文模式，该模式改编自他的智能体记忆演讲。这种模式保持智能体的工作集紧凑，同时允许底层保管库增长而不至于撑爆上下文窗口。
* **wiki/hot.md** — 小型工作记忆文件。发生了什么变化、什么在阻塞、下一步做什么。
* **wiki/index.md** — 完整导航地图。按分区分组的保管库维基链接。
* **wiki/** — 通过维基链接和枢纽按需加载的深度笔记。
对于任何跨智能体会话，你可以将智能体指向保管库并运行：*"Read CODEX.md, then wiki/hot.md, then wiki/index.md, then the relevant note."* 大脑始终保持紧凑的工作集，并按需拉取深层上下文。
Karpathy 的三层记忆模式，应用于本地 SEO。
## 本地 SEO 排名的 3 个阶段
该大脑在 frameworks/、audits/ 和 gbp/ 文件夹中教授并实施一个三阶段战略框架。属性（Property），然后是 GBP，再是权威（Authority）——按此顺序，因为跳过某个阶段会浪费所有基于其上的努力。
### 阶段 1 — 属性（网站基础）
你的网站是基础。如果没有经过适当审计的网站（E-E-A-T、Core 30 页面、技术健康、点击率优化的元数据、行业特定的模式子类型），其他一切都无法放大。该大脑提供技术、页面、内容和点击率的属性审计，以及 Core 30 内容地图，用于构建主题权威并获取 2025 年 12 月核心更新所奖励的行为信号。
### 阶段 2 — GBP（店面）
你的 Google Business Profile 就是店面。60-70% 的本地点击来自地图包。GBP 文件夹涵盖从验证到帖子、地理标记、服务、类别、营业时间、NAP 一致性以及消息与问答界面。Claude Code 智能体吸收你的 GBP 洞察导出数据，并编写一份带来源的优先级 GBP 路线图。
地图包就是店面——60-70% 的本地点击发生在这里。
### 阶段 3 — 权威（引文、反向链接、评论、AI 引文份额）
来自第三方网站的信任信号。本地引文、竞争性反向链接研究、社交信号、评论速度，以及跨越 ChatGPT、Perplexity、Google AI Overviews 和 Bing Copilot 的 AI 引文份额。Claude Code 吸收 Local Falcon / Whitespark / BrightLocal / Moz Local 导出数据，并综合生成一份优先级权威路线图。
## 面向本地的 LLM SEO：2026 年新鲜度层
Local SEO 大脑附带一个专用的 llm-seo/ 文件夹，涵盖在 ChatGPT、Perplexity、Google AI Overviews 和 Bing Copilot 中显示所需的内容——这四个信号两年前还不存在作为排名因素，但现在驱动着可衡量的本地发现份额：
* **Reddit 引文引擎。** Reddit 约占 LLM 引文份额的 40%。该大脑附带一个操作手册，用于在不违反子版块规则的情况下，在 r/localBusiness 子版块中获取 Reddit 引文。
* **人物 schema。** 2025 年 12 月后对 E-E-A-T 归属至关重要。该大脑为企业主、关键员工和贡献作者模板化 Person schema。
* **行业特定的 schema 子类型。** 通用 LocalBusiness 已不再足够。该大脑为水管工、餐厅、牙医、家居与建筑企业、汽车维修等提供子类型。
* **AI Overviews 地图包压力。** 引文模式已稳定为每条 AI Overview 3 到 5 个来源。该大脑优化以加入这个紧凑集合。
2026 年新鲜度层将原始知识库与当前状态笔记叠加，以便过时的引用——已停用的 GBP Chat、已淘汰的 LLM 模型名称、2025 年 12 月核心更新、Reddit 引文份额——在原地用警告标注标记出来。
## 审计输出是什么样的
针对预播种的示例保管库运行 `synthesize_brain.py` 后，Claude Code 智能体会生成一份健康评分卡，包含阶段评分、前 5 个优先级操作和一个审批队列。每条建议都带有 `source:` 行——如果支持的原始文件缺失，该建议就不会发布。
来自大脑在演示保管库上一次运行的真实健康评分卡。
```
# acme-plumbing · 健康评分卡
> 生成于 2026-05-20 · 来源已验证
## 阶段评分
| 阶段     | 评分 | 最严重问题                                           |
|-----------|-------|-------------------------------------------------------|
| 属性      | C+   | 首页缺少 LocalBusiness > Plumber schema 子类型        |
| GBP       | B-   | 主类别中缺少 3 个服务                                 |
| 反向链接  | D    | 与前 3 名竞争对手相比存在 14 个引文差距               |
| LLM SEO   | C    | 无 Reddit 存在；关于页面缺少 Person schema            |
## 前 5 个优先级（有来源）
1. 向首页添加 Plumber schema 子类型
   来源: .raw/sources/property-audit.csv:42 · sha256: 9a3f...
2. 在主 GBP 类别中补充 3 个缺失的服务
   来源: .raw/sources/gbp-export.csv:18 · sha256: 4c12...
3. 缩小 14 个引文差距（BBB、Yelp、Angi、HomeAdvisor + 10）
   来源: .raw/sources/whitespark.csv:1-203 · sha256: e7b9...
4. 每周在 r/Plumbing 中构建 1 条 Reddit 回答，引用所有者照片
   来源: wiki/llm-seo/Reddit-LLM-Citation-Engine.md
5. 为排名前 4 的服务区域郊区发布位置页面
   来源: wiki/audits/Topical-Geo-Relevance.md
## 审批队列
- [ ] Schema 标记更改（回滚：撤销部署）
- [ ] GBP 服务添加（回滚：从所有者界面上重新删除）
```
## 大脑开箱即用的六个用例
Claude Code Local SEO 大脑专为机构和运营人员实际执行的工作而构建，而非抽象的“AI SEO”演示。六个场景附带操作手册：
| 场景 | 运行的命令 | 获得的结果 |
| --- | --- | --- |
| **新客户启动**（单地点） | `scaffold_vault` → 吸收 4 个来源 → `synthesize_brain` | 第一天健康评分卡、优先级路线图、引文差距 |
| **多地点审计**（12 家门店连锁） | 每个地点一个搭建、并行吸收、连锁级综合 | 每地点评分矩阵、共享 NAP 问题、连锁范围引文差距 |
| **AI 搜索可见性检查** | 吸收 GBP + 针对 GPT-5 / Claude / Gemini 运行 `llm-seo/` 提示 | 品牌提及差异、Reddit 差距、schema 现代化检查清单 |
| **季度回顾**（现有客户） | 重新运行所有吸收器、检查图结构、生成报告 | 速度报告：已完成、已推迟、下一步 |
| **竞争对手拆解**（一个对手） | 通过 `competitors/` 模板吸收竞争对手反向链接 + GBP 资料 | 带来源机会的差距分析 |
| **属性交接**（机构到内部团队） | 将完整保管库渲染为 HTML | 静态、可搜索、带来源引用的交接包 |
## 拒绝规则：大脑不会做什么
大多数本地 SEO 自动化会滑向那些可能导致账户被封的灰色手段。Local SEO 大脑将拒绝规则内置到提示层中——智能体拒绝建议或生成任何违反 Google 垃圾政策或可能导致 GBP 暂停的模式。
大脑拒绝假评论、临近位置作弊、GBP 类别作弊和“保证排名第一”的文案。
## Local SEO 大脑适合谁
| 买家 | 获得什么 |
| --- | --- |
| **本地 SEO 机构与自由职业者**（5 到 50 个客户地点） | 一个可重复、带来源引用的操作层，取代电子表格审计和截图堆。 |
| **多地点运营者**（10+ 个地点） | 每个地点的 NAP、引文、评论、地理网格扫描和审计历史记忆层。 |
| **自行做 SEO 的服务型企业**（水管工、暖通空调、律师、牙医、家居服务） | 一个集操作手册与记忆于一体的产品。大脑知道该做什么，并且记得已经做了什么。 |
## 如何获取 Local SEO 大脑
两种获取 Local SEO 大脑的方式——选择适合你工作方式的路径。
* **捆绑包**——包含在 [AI Marketing Hub PRO](https://skool.com/ai-marketing-hub-pro) 中，连同其余的大脑库、社区支持以及每次大脑版本发布时的访问权限。
* **单买**——在 [Gumroad](https://erniseth.gumroad.com/l/localseobrain) 上以 69 美元购买保管库。同样的保管库，无社区。随时升级到 PRO 以获取下一个大脑版本。
其他工具和开放构建位于 [github.com/AI-Marketing-Hub](https://github.com/AI-Marketing-Hub)。如需完整参观 Local SEO 大脑的实际运行，[观看 YouTube 上的 6 分钟操作演示](https://youtu.be/4v7116sEbrQ)。
## 大脑系列后续
Local SEO 大脑是系列中的第一个。相同的 Hot/Index/Wiki 记忆模式 + 带来源引用的综合器可应用于其他营销领域——每个领域都有自己的操作手册库和吸收器集。路线图：
* **营销大脑** — 全漏斗 ICP / 消息 / 内容 / 渠道编排
* **邮件列表大脑** — 列表增长 + 细分 + 发送策略
* **电商大脑** — 产品目录 + PMax + 评论挖掘 + 留存循环
* **B2B SaaS 大脑** — 内容驱动 + ABM + 生命周期编排
在 [YouTube 视频](https://youtu.be/4v7116sEbrQ) 下回复你想要下一个构建哪个大脑——你的意见将塑造路线图。
