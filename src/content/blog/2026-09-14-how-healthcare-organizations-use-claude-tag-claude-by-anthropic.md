---
title: "医疗机构如何使用 Claude Tag | Anthropic 出品 Claude"
description: "医疗机构正在使用 [Claude Tag（测试版）](https://support.claude.com/en/articles/15594475-what-is-claude-tag)，它将 Claude 以 [团队成员](https://academy.claude.com/courses/building-effective-human-agent-teams) 的身份引入 Slack，帮"
pubDate: "2026-09-14"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/how-healthcare-organizations-use-claude-tag"
---

# 医疗机构如何使用 Claude Tag | Anthropic 出品 Claude

---

医疗机构正在使用 [Claude Tag（测试版）](https://support.claude.com/en/articles/15594475-what-is-claude-tag)，它将 Claude 以 [团队成员](https://academy.claude.com/courses/building-effective-human-agent-teams) 的身份引入 Slack，帮助它们对生产环境告警进行初步分类、维护内部工具，并解答有关支付方规则的问题。虽然 Claude Tag 尚未纳入 Anthropic 的 [商业伙伴协议](https://support.claude.com/en/articles/8114513-business-associate-agreements-baa-for-commercial-customers) 覆盖范围，但已有数家医疗机构在完全不涉及受保护健康信息（PHI）的频道和连接器中投入使用。

  在频道中提及 @Claude，它便会阅读整个对话线程，调用你已连接的各类工具，完成工作并汇报结果。它会记住每个频道中发生的一切，因此长期任务无需重复说明；你还可以让它监控某个频道，在需要时主动介入。

管理员决定 Claude Tag 的工作范围和可访问内容，因此医疗团队可以将 Claude Tag 排除在包含 PHI 的频道和系统之外：

* Claude Tag 默认处于关闭状态，**仅在经批准的频道中启用**，同时禁用私信功能，并**按频道限定连接器范围**。
* Claude Tag 不会读取 Slack 的全部内容——它只能看到工作区成员能看到的内容。虽然它可以阅读工作区的公开频道并通过关键词搜索，但无法访问未被邀请加入的私密频道。
* **访问权限包**允许团队在一个频道中集中连接代码库、问题追踪器等数据源，同时确保电子健康档案（EHR）、临床系统和患者沟通渠道不可访问。了解更多关于 Claude Tag [智能体身份访问模型](https://claude.com/blog/agent-identity-access-model) 及其完整架构的信息，请参阅 [安全与数据处理文档](https://claude.com/docs/claude-tag/concepts/security-and-data?open_in_browser=1)，医疗机构的最佳实践可参阅 [此处](https://claude.com/docs/claude-tag/admins/healthcare)。

激活 Claude Tag 并将其与 GitHub 关联的企业级机构可获得 25,000 美元的 Claude Tag 额度（拥有 10 个以上席位的团队版机构可获得 2,500 美元）；请注意，这些额度将于 2026 年 10 月 1 日到期。额度详情见 [此处](https://support.claude.com/en/articles/15575654-claude-tag-launch-promo-for-claude-team-and-enterprise)。

以下是 Insight Health、Tennr 和 Medallion 如何利用 Claude Tag 构建 [人机协作团队](https://claude.com/blog/building-effective-human-agent-teams) 的实际案例。

#### **Insight Health 的事件响应实践**

Insight Health 开发了 MagicDocs，一款面向专科医疗机构的 AI 转诊协调工具。MagicDocs 读取患者传入的文件（传真、转诊单、预授权申请、检验报告和病历），提取临床信息，将每份文件与正确的患者匹配，并将结构化数据写入电子健康档案（EHR）。该公司目前服务覆盖 56 个专科领域的 1,100 多家医疗机构。

随着 Insight Health 客户群的扩大，生产环境告警的初步分类占用了其精简工程团队大量本可用于更高价值工作的时间。因此，过去三个月来，该公司一直在不含 PHI 的工程与支持频道中运行 Claude Tag，由它调查生产环境告警、提交并去重工单、审查拉取请求（PR），并在不同对话线程之间传递上下文。

在告警量巨大的生产环境告警频道中，他们将 Claude Tag 与另一个智能体 Zeus（由 Insight Health 基于 Claude Agent SDK 构建）配对使用，以将一次事件从首次通知一路推进到经过测试的修复方案。两个智能体被刻意赋予不同的权限：Claude Tag 可以访问代码库和 Linear，因此了解代码模式和工单历史；Zeus 运行在该公司受 BAA 覆盖的 Claude API 组织上，因此可以查询生产数据，并在数据到达 Slack 之前对 PHI 进行脱敏处理。当告警到来时，两个智能体会协同调查——查询生产数据、对照代码和近期部署记录、与历史工单进行比对——同时在整个过程中于 Slack 线程中汇报进展。一旦确定根本原因，它们便会创建一份拉取请求草稿并监控测试结果，最终由工程师审查并合并。Claude Tag 的频道记忆在此处价值显著：它能识别出一条新投诉属于已知问题且修复方案正在等待中，回忆起数周前的调查记录，并无需提示即可遵循既定指令执行。

"频道中的每个人都能看到两个智能体的推理过程以及它们如何分工协作，"Insight Health 联合创始人兼 CTO Saran Siva 说道，"这种透明度建立了信任，也教会了团队如何从智能体中获取最大价值。"自 Claude Tag 上线以来，Insight Health 关键告警频道中 97% 的告警无需工程师介入便已关闭，使团队得以专注于核心产品工作。

除了事件响应，Insight Health 团队还将 Claude Tag 用于招聘、供应商谈判准备、合同审查（将条款与通话记录进行比对）以及日常业务运营。

#### **Tennr 的内部工具维护**

Tennr 是一个患者协调平台，通过自动化接诊、病历记录、授权和排程等原本会阻碍诊疗流程的工作，帮助医疗机构更快地将患者引导至合适的诊疗场景。Claude Tag 驻留在其内部工具中，帮助非技术团队搭建和维护自定义内部应用。

在 Tennr 这样快速增长的公司，内部工具的废弃速度往往与创建速度一样快，因为没有人有精力去维护它们。2026 年 7 月，Tennr 在构建内部工具时采取了不同的方式。它推出了 recruiting.tennr.com——一个使用 Claude Code 构建的内部录用通知展示门户，并通过专用 Slack 频道将 Claude Tag 设为主要维护者。如今，真正使用工具的人（招聘人员、人力资源团队成员、招聘经理和营收运营人员）可以用日常英语以 @ 提及的方式向 Claude 提出需求，Claude 便会完成代码修改、部署上线并汇报结果。他们可以直接迭代工具，无需将工程师从产品工作中抽离。

大约一个月内，团队以这种方式交付了 15 个以上的工单：一个从上传的 PDF 单页简报中构建的福利深度解读板块、一个链接到 Dropbox Sign 的"在此签署录用通知"横幅、目标奖金字段，以及自助式管理控制功能。当一个对外暴露的复制 API 被标记后，Claude 当天便将其锁定。当 Ashby 导入的缺陷不断重置已发出录用通知中的股权数据时，Claude 修复了该缺陷并同步传播了修正后的数值。当另一个缺陷短暂导致候选人的浮动薪酬显示错误时，Claude 未经请求便在频道内发布了包含受影响时间窗口和补救措施的全体通知。它还编写了入职文档，并负责该工具的权限管理。

团队在频道内向 Claude 传授了自身的运维规范，包括一套表情符号状态图例，以及一种包含编号工单、请求人、提交链接、截图和在线测试链接的工单格式。Claude 此后便沿用了这些规范。招聘人员以自然语言提交着实质上等同于工程工单的需求，有时甚至只是一张截图，便能在一个小时内收到生产环境的变更。"Claude Tag 是让内部工具维护变得可行的关键，"Tennr 业务运营与战略副总裁 Abe Griffiths 说道，"通过 Slack 频道让 Claude 担任工具的管家，真正使用工具的人可以直接对其进行迭代，而无需将工程师从产品工作中抽离。"

推广之所以迅速，是因为基础工作已经就绪。Tennr 仅允许在有限的一组私密频道中存放 PHI，因此广泛添加 Claude Tag 并未引入新的数据问题。让 Claude Tag 易于获批的关键在于按频道限定范围。

"在录用通知工具频道中，我们希望 Claude 积极主动：修复问题、修改代码、跳过拉取请求审查、直接上线，"Griffiths 说道，"这种姿态显然不适合产品或工程频道，在那里 Claude 的角色更接近信息收集或帮助我们保持有序。能够逐频道划定这些界限，意味着我们可以在低风险场景中赋予 Claude 真正的自主权，而无需在所有频道中开放。"

#### **Medallion 积累支付方专业知识**

Medallion 为医疗机构自动化处理医疗提供者的资质认证、执业许可和支付方注册。其中大量工作依赖于晦涩且缺乏文档记录的规则：哪些支付方要求一定数量的已认证提供者后团体才能注册、各州特定的法规，以及数十个类似的问题——工程师在将流程编码到产品中之前都需要得到解答。过去，这些答案掌握在一小批内部医疗领域专家手中，为核心产品开发决策造成了持续存在的人力瓶颈。

如今，Claude Tag 已嵌入这一流程，打破了知识孤岛。当工程师在 Slack 中提出一个支付方规则问题时，Claude Tag 会依据过往专家回复、历史数据和非结构化内部资源给出回答。专业知识在频道中不断积累，而不再仅存于某一个人脑中。

"当它不够确信时，会 @ 相关的专家，而该专家的回复将成为未来相关话题回答的基础，"CTO Armaan Sarkar 说道。

是什么让他们放心地推广 Claude Tag？Sarkar 指出关键在于选择了正确的不涉及 PHI 的工作流、专家审核以及自动化验证。Claude Tag 在政策层面运作：问题涉及的是支付方规则和流程，而非个别患者或提供者。它不会独立作答：专家提供监督与纠正，且每一次交互都发生在公司任何人都可以审计的 Slack 频道中。同时，输出结果会在下游接受检查，因为 Medallion 的系统会依据这些政策执行操作，而这些操作本身就会经过独立的审计和验证。

#### **快速上手**

上述所有团队都从同一个起点出发：工程、产品、运营或招聘频道，搭配一两个连接器，在公开对话线程中工作而非使用私信，以便整个团队都能审阅并承接上下文。

参阅 [此处](https://claude.com/docs/claude-tag/admins/healthcare) 的医疗机构 Claude Tag 最佳实践，然后从一个频道开始。在一个告警或支持工程频道中开启 Claude Tag，连接 GitHub，让团队使用两周后再扩大允许列表。

***开始使用*** [***Claude Tag***](https://support.claude.com/en/articles/15594475-what-is-claude-tag)***。***

---

**Note on translation choices:** I preserved all product names (Claude Tag, Slack, GitHub, Linear, Ashby, Dropbox Sign, MagicDocs, Zeus, Claude Code, Claude Agent SDK), company names (Insight Health, Tennr, Medallion), and technical acronyms (PHI, BAA, EHR, PR, RevOps) in their original form as they are proper nouns/brand names. The tone remains professional and informative, matching the original blog post style. All markdown formatting (headers, bold, links, bullet points, blockquotes) has been kept identical to the source.
