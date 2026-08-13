---
title: "Claude Code 安全：AI 安全审计在我的代码中发现了 23 个漏洞"
description: "---"
pubDate: "2026-04-14"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-cybersecurity-ai-security-audit"
---

# Claude Code 安全：AI 安全审计在我的代码中发现了 23 个漏洞

---

claude-cybersecurity 是一个免费、开源的 Claude Code 技能，它能把你的终端变成一个完整的安全审计平台。一条命令。八个专业代理并行运行。零配置。它覆盖了 OWASP Top 10:2025、CWE Top 25、MITRE ATT&CK 技术、11 种编程语言和 5 个合规框架。**如果你在 2026 年仍在发布代码，特别是 vibe-coded 代码，你的工作流中需要像这样的工具。**

我在意识到我用 AI 辅助发布的代码积累安全债务的速度远超我的追踪能力之后，构建了这个工具。第一次运行时，它就在我自己的 [claude-ads](/blog/claude-ads-v1-5-release) 项目中发现了一个我完全遗漏的 SSRF 漏洞，这个工具当场就回本了。

## 关键要点

* claude-cybersecurity 通过单个 `/cybersecurity` 命令运行 8 个并行安全代理
* 覆盖 OWASP Top 10:2025、CWE Top 25 以及 11 种语言的 7 种 MITRE ATT&CK 技术
* AI 生成的代码比人类编写的代码多出 2.74 倍的漏洞（Veracode 2025）
* 使用加权评分系统（0-100），严重发现会自动触发 CRITICAL 门控
* 免费开源的 GitHub Advanced Security（GHAS）替代方案
* 单条 curl 命令 30 秒即可安装

## 什么是 Claude Cybersecurity？

claude-cybersecurity 是一个 Claude Code 技能，它编排八个专业安全代理来审计你的代码库。你在终端输入 `/cybersecurity`，它就能处理一切：检测你的技术栈、映射信任边界、运行并行分析，并生成带有修复建议的优先级报告。不需要 API 密钥。不需要配置文件。不需要 SaaS 订阅。它适用于任何 Claude Code 可以读取的项目，基本上就是你机器上的所有东西。

该技能遵循我在所有 [Claude Code 技能](/blog/best-claude-code-skills-2026) 中使用的相同架构模式：一个协调器，负责收集上下文、调度专业代理，并将它们的输出综合成可操作的结果。区别在于深度。八个代理中的每一个都携带自己的参考数据、检测启发式规则以及针对特定框架调优的误报抑制规则。

## 为什么 vibe-coded 应用需要进行安全审计

Vibe 编码的安全性已不再是理论上的担忧。2025 年的数据清楚地表明，AI 辅助的代码比手工编写的代码承担着更高的风险。如果你正在使用 Claude、Copilot 或任何其他 AI 编码工具来发布生产代码，你很可能会引入你现有的审查流程无法捕捉到的漏洞。

根据 Veracode 的 2025 年软件安全状况报告，AI 生成的代码包含的**安全缺陷比人类编写的代码多 2.74 倍**。该研究分析了超过 220 万个应用程序。另外，45% 的 AI 生成的代码片段引入了至少一个 OWASP Top 10 漏洞。佐治亚理工学院的研究人员追踪了 2024-2025 年间直接源自 AI 生成的代码并被合并到开源项目中的 74 个 CVE。

这个模式仔细想想就明白了。AI 模型是在整个互联网上训练的，包括数百万个为了简单而使用不安全模式的 Stack Overflow 答案和教程。模型优化的是“能工作的代码”而不是“安全的代码”。它会很高兴地生成带有字符串拼接的 SQL 查询、以明文形式存储秘密、或者跳过输入验证——除非你明确要求它们做这些事情。

传统的 SAST 工具能捕获一部分，但它们是为人类开发者编写的模式而设计的。AI 引入了新的风险类别：幻觉依赖（不存在的包，攻击者会将其注册为恶意软件）、看起来正确但有细微缺陷的过度自信的安全实现，以及一种特别倾向于从训练数据中复制不安全模式而不理解其危险性的倾向。

## 8 个专业代理

claude-cybersecurity 不是运行单个整体扫描，而是派发八个专注的代理并行运行。每个代理都有一个特定的安全领域、一个加权的整体分数贡献以及自己的一套检测规则。这种并行架构使得完整审计比顺序扫描更快完成，同时捕捉单用途工具遗漏的跨领域问题。

**8 个专业代理 — 并行执行**

| 漏洞扫描器 | 20% — OWASP Top 10 + CWE Top 25，污点分析 |
| --- | --- |
| 认证审查器 | 15% — IDOR、权限提升、会话管理 |
| 威胁情报 | 15% — 恶意软件、后门、C2 通信、MITRE ATT&CK |
| 秘密检测 | 10% — 语义检测、混淆凭据 |
| 依赖审计器 | 10% — 供应链、slopsquatting、typosquatting |
| IaC 扫描器 | 10% — Terraform、Docker、K8s、GitHub Actions |
| AI 代码审查器 | 10% — AI 生成模式、幻觉依赖 |
| 逻辑分析器 | 10% — 业务逻辑、竞态条件、TOCTOU |

**漏洞扫描器（20%）** 是最重的代理。它执行污点分析，追踪从用户输入到危险接收器的数据流，并将发现结果映射到 OWASP Top 10:2025 和 CWE Top 25:2024 目录。该代理捕获注入缺陷、XSS、反序列化问题和路径遍历漏洞。

**认证审查器（15%）** 专门关注认证和授权。它寻找 IDOR（不安全的直接对象引用）模式、权限提升路径、损坏的会话管理和缺失的访问控制。这个代理特别擅长捕捉 AI 生成的代码中常见的“忘记检查权限”的错误。

**威胁情报（15%）** 扫描入侵指标：恶意软件签名、后门模式、命令与控制通信通道以及映射到 MITRE ATT&CK 框架的已知攻击技术。如果某个依赖或代码片段包含混淆的恶意负载，这个代理就会标记出来。

**秘密检测（10%）** 超越了基于正则表达式的秘密扫描。它使用语义分析来发现混淆的凭据、伪装成配置值的硬编码令牌以及经过 base64 编码或分散在多个变量中的秘密。传统的秘密扫描器经常遗漏这些模式。

**依赖审计器（10%）** 处理供应链安全。它检查已知的易受攻击依赖、typosquatting（名称与流行包相似的包）和 slopsquatting（AI 模型幻觉出的包，然后攻击者在 npm/PyPI 上注册）。这是一个日益增长的攻击向量，大多数团队都没有监控。

**IaC 扫描器（10%）** 审计你的基础设施即代码：Terraform 配置、Dockerfile、Kubernetes 清单和 GitHub Actions 工作流。配置不当的基础设施是导致数据泄露的主要原因之一，这个代理可以捕获过于宽松的 IAM 策略、未固定的操作版本、暴露的端口和不安全的容器配置。

**AI 代码审查器（10%）** 专门针对 AI 生成的代码中常见的模式：幻觉依赖、从训练数据中复制粘贴的不安全模式、过度自信的加密实现以及 LLM 产生的“看起来正确但略有缺陷”的代码。这个代理之所以存在，是因为 AI 代码与人类代码有不同的失败模式。

**业务逻辑分析器（10%）** 寻找竞态条件、TOCTOU（检查时间到使用时间）错误、不正确的状态机转换以及仅靠模式匹配无法检测到的逻辑缺陷。这些是最难用传统 SAST 工具发现的漏洞，因为它们需要理解应用程序的预期行为。

## 它是如何工作的：GARE 架构

该技能遵循一个四阶段架构，称为 GARE：收集（Gather）、分析（Analyze）、建议（Recommend）、执行（Execute）。这种编排模式与企业安全工具中使用的相同，但经过调整以适应 Claude Code 的执行环境。整个管道在本地运行，数据不会离开你的机器。

**第一阶段：收集**  
检测技术栈 | 枚举入口点 | 映射信任边界 | STRIDE 分析 | 构建上下文  
🔍

**第二阶段：分析 — 8 个代理并行**  
漏洞 20% | 认证 15% | 威胁 15% | 秘密 10% | 依赖 10% | IaC 10% | AI 代码 10% | 逻辑 10%  
每个代理加载自己的参考文件 | 返回 VULN-XXX 发现 + 类别分数（0-100）+ 置信水平

**第三阶段：建议**  
分数聚合 | 攻击路径链 | 合规映射

**第四阶段：执行**  
结构化报告 | 优先级修复队列

```
/cybersecurity [path] [--scope full|quick|diff] [--compliance pci|hipaa|soc2|gdpr]
零配置 | 自动检测语言和框架 | 框架感知的误报抑制
```

**第一阶段：收集。** 协调器扫描你的项目以检测语言、框架和基础设施。它枚举入口点（API 路由、表单处理器、CLI 接口）、映射信任边界（用户输入进入系统的地方），并执行 STRIDE 威胁模型。这个上下文被传递给每个代理，以便它们知道自己在看什么。

**第二阶段：分析。** 所有八个代理并行运行。每个代理接收收集到的上下文以及自己的领域特定参考文件。每个代理返回一系列发现（标记为 VULN-001、VULN-002 等），附带严重性分数、置信水平、受影响的文件和修复建议。并行执行意味着对中等规模的代码库进行完整审计只需要几分钟，而不是几小时。

**第三阶段：建议。** 协调器聚合所有代理的发现结果，去重重叠问题，将相关漏洞链接成攻击路径，并将所有内容映射到你选择的合规框架（PCI DSS、HIPAA、SOC 2、GDPR 或 NIST 800-53）。输出是一个按风险排序的优先级修复队列。

**第四阶段：执行。** 最终报告包括整体安全分数、字母等级（A 到 F）、每个发现的严重性和置信水平，以及特定的代码级修复建议。你可以要求 Claude Code 直接应用修复，或者导出报告供团队审查。

## 真实结果：审计 Claude Ads（从 62/100 到 90/100）

展示这个工具有多好的最佳方式是分享我对自己代码运行它时发生的事情。我将 claude-cybersecurity 指向了 [claude-ads v1.5](/blog/claude-ads-v1-5-release) 代码库，期望得到一个完全健康的报告。我错了。初始得分是 62/100，一个 D 级。该工具在 5 个类别中发现了 23 个漏洞。

最严重的发现是 API 集成层中的一个 SSRF（服务器端请求伪造）漏洞。代码接受了用户提供的用于 Webhook 回调的 URL，而没有验证目标。攻击者可以利用这一点让服务器向内部服务发送请求。该工具将其标记为 CRITICAL，置信度为 HIGH，并提供了精确的修复方法：使用允许的域名白名单进行 URL 验证。

IaC 代理发现几个 GitHub Actions 工作流使用了未固定的操作版本（例如，`uses: actions/checkout@v4` 而不是固定到特定的 SHA）。这是一个供应链风险，因为被攻破的操作可能会向 CI 管道注入恶意代码。修复很简单：将每个操作固定到其完整的提交 SHA。

该工具还标记了缺少 CI 安全门控。CI 管道中没有自动化的安全扫描，这意味着漏洞可以在没有任何自动检查的情况下被合并。我添加了 CodeQL 扫描和依赖审查作为必需的检查。在应用了所有 23 个修复并重新运行审计后，分数跃升至 90/100。这项工作已在 [v1.5.1 补丁发布](/blog/claude-ads-v1-5-release) 中发布。

## 评分系统

评分系统旨在既精确又实用。每个发现都会根据四个因素计算严重性分数：基础严重性（从 CVSS 映射）、置信水平、可利用性和上下文修饰符。整体项目分数是所有八个代理分数的加权聚合，权重与上面代理图表中显示的百分比一致。

**安全分数 — 0 到 100**

F → D → C → B → A → 0 → 25 → 50 → 75 → 90 → 100

分数 = 基础严重性（CVSS）× 置信水平（0.3-1.0）× 可利用性（0.5-1.0）± 上下文（-20 到 +20）

4 级置信度：高（90-100%）| 中（60-89%）| 低（30-59%）| 信息（<30%）

严重性等级：CRITICAL 90-100 | HIGH 70-89 | MEDIUM 40-69 | LOW 20-39 | INFO 0-19

有五个严重性等级：CRITICAL（90-100）、HIGH（70-89）、MEDIUM（40-69）、LOW（20-39）和 INFO（0-19）。有四个置信度等级：HIGH（90-100%）、MEDIUM（60-89%）、LOW（30-59%）和 INFO（低于 30%）。置信度等级直接影响一个发现对你分数的影响程度，因此 LOW 置信度的 CRITICAL 发现不会像 HIGH 置信度的那样拖垮你的分数。

自动 CRITICAL 门控是一个重要的功能。如果任何一个发现的分数达到 90 或以上且置信度为 HIGH，那么整体项目分数会自动上限为 69（C 级），无论其他所有内容得分如何。这可以防止项目在藏有已知关键漏洞的情况下获得 A 级。你必须先修复关键问题。

## 覆盖范围

覆盖范围涵盖了现代 Web 应用和 API 所涉及的主要安全标准和框架。claude-cybersecurity 并没有试图穷尽一切，而是专注于实际出现在真实世界漏洞中的漏洞类别。每条检测规则至少映射到一个标准，因此发现结果可以追溯到行业基准。

**概览**

- OWASP Top 10:2025（10/10），包括新增的 A03 供应链和 A10 异常条件
- CWE Top 25:2024 数据（25/25），每个 CWE 有专用检测部分
- MITRE ATT&CK 技术（7 种）：T1059、T1027、T1071、T1195、T1005、T1041、T1496
- 合规框架（5 种）：PCI DSS | HIPAA | SOC 2 | GDPR | NIST 800-53
- 支持 11 种语言：Python、JS/TS、Java、Go、Rust、C/C++、Ruby、PHP、C#、Swift/Kotlin、Shell
- 支持 4 种 IaC 平台：Terraform（AWS/GCP/Azure）、Dockerfile、Kubernetes、GitHub Actions
- 支持 10 个框架的误报抑制：Django、Flask、FastAPI、Express、React、Vue、Angular、Spring Boot、Rails、ASP.NET Core + 7 个 ORM

OWASP Top 10:2025 版本包含两个以前未涵盖的新类别：A03（软件和数据完整性/供应链）和 A10（异常条件）。依赖审计器和 IaC 扫描器处理 A03，而业务逻辑分析器覆盖 A10。所有 10 个类别都有专用的检测逻辑。

语言支持涵盖 11 种语言：Python、JavaScript/TypeScript、Java、Go、Rust、C/C++、Ruby、PHP、C#、Swift/Kotlin 和 Shell 脚本。该工具自动检测存在的语言并加载相应的检测规则。针对 10 个主流框架（包括 Django、FastAPI、Express、React、Spring Boot 和 Rails）以及 7 个 ORM 提供了框架感知的误报抑制。

## Claude Code 安全 vs GitHub Advanced Security

GitHub Advanced Security（GHAS）是最常见的比较对象，所以我直接说明。GHAS 是一个可靠的企业级产品。它与 GitHub 紧密集成，在 CI 中自动运行，并且具有出色的 CodeQL 分析。但它也有显著的局限性，这使得 claude-cybersecurity 成为许多团队中一个引人注目的 GitHub Advanced Security 替代方案。

| 能力 | GHAS | claude-cybersecurity |
| --- | --- | --- |
| 成本 | 每月每个提交者 49 美元 | 免费（开源） |
| 语言 | 9 种（CodeQL） | 11 种 |
| OWASP Top 10:2025 | 部分 | 全部 10/10 |
| 业务逻辑分析 | 否 | 有（专用代理） |
| AI 代码特定检查 | 否 | 有（幻觉依赖等） |
| IaC 扫描 | 有限 | Terraform、Docker、K8s、Actions |
| 合规映射 | 否 | PCI、HIPAA、SOC 2、GDPR、NIST |
| MITRE ATT&CK 映射 | 否 | 7 种技术 |
| CI 集成 | 原生 | 通过 Claude Code CLI |
| 数据留在本地 | 否（云端） | 是 |
| 修复建议 | 基础 | 代码级，可自动应用 |

GHAS 在 CI 集成方面表现出色，并且具有在每个拉取请求上自动运行的优势。claude-cybersecurity 更适合按需深度审计，支持更多语言，包含业务逻辑和 AI 特定检查，并且完全免费。对于大多数团队来说，理想的设置是同时使用两者：GHAS 用于持续的 CI 扫描，claude-cybersecurity 用于定期的深度审计和发布前的安全审查。

## 如何安装

安装大约需要 30 秒。一行安装脚本会克隆仓库并将技能文件复制到项目的 `.claude/skills/` 目录中。你也可以手动安装，如果你更喜欢先检查文件的话。

**一行安装：**

```
curl -fsSL https://raw.githubusercontent.com/AgriciDaniel/claude-cybersecurity/main/install.sh | bash
```

**手动安装：**

```
git clone https://github.com/AgriciDaniel/claude-cybersecurity.git
cp -r claude-cybersecurity/.claude/skills/cybersecurity your-project/.claude/skills/
```

**快速启动命令：**

```
# 完整安全审计
/cybersecurity

# 审计特定目录
/cybersecurity src/

# 快速扫描（更快，检查较少）
/cybersecurity --scope quick

# 带合规映射的审计
/cybersecurity --compliance soc2

# 仅扫描更改的文件（非常适合 PR）
/cybersecurity --scope diff
```

该技能适用于 Claude Code 可以读取的任何项目。没有外部依赖、没有 API 密钥、没有需要设置的配置文件。如果你想了解更多关于构建和定制 Claude Code 技能的信息，请查看 [skill-forge 指南](/blog/skill-forge-build-claude-code-skills) 或浏览完整的 [最佳 Claude Code 技能](/blog/best-claude-code-skills-2026) 列表。

## 常见问题

### 什么是 claude-cybersecurity？

claude-cybersecurity 是一个免费、开源的 Claude Code 技能，可以对你的代码库进行全面的安全审计。它编排八个专业代理并行运行，涵盖漏洞扫描、认证审查、威胁情报、秘密检测、依赖审计、基础设施即代码扫描、AI 代码审查和业务逻辑分析。你只需在终端输入 `/cybersecurity` 命令即可调用。

### 它与 GitHub Advanced Security 相比如何？

GHAS 每月每个提交者需要 49 美元，专注于基于 CodeQL 的 SAST 扫描，具有强大的 CI 集成能力。claude-cybersecurity 是免费的，支持更多语言（11 种 vs 9 种），包含 GHAS 所缺乏的业务逻辑和 AI 特定检查，并将发现结果映射到合规框架。GHAS 更适合自动化的 CI 门控。claude-cybersecurity 更适合按需深度审计。许多团队同时使用两者。

### 它支持哪些语言？

该工具支持 11 种编程语言：Python、JavaScript、TypeScript、Java、Go、Rust、C、C++、Ruby、PHP、C#、Swift、Kotlin 和 Shell 脚本。它会自动检测项目中存在的语言并加载相应的检测规则。针对 Django、Flask、FastAPI、Express、React、Vue、Angular、Spring Boot、Rails 和 ASP.NET Core 提供了框架感知的误报抑制。

### 它是免费的吗？

是的。claude-cybersecurity 使用 MIT 许可证，完全免费。该技能本身没有成本、没有 API 密钥、没有使用限制。你需要 Claude Code 来运行它，这需要 Anthropic API 密钥（按使用付费）或 Claude Pro/Team 订阅。但安全技能本身不会在你已经为 Claude Code 访问支付的费用上增加额外成本。

### 它能检测业务逻辑缺陷吗？

是的。业务逻辑分析器是八个专业代理之一。它寻找竞态条件、TOCTOU（检查时间到使用时间）漏洞、不正确的状态机转换以及模式匹配的 SAST 工具无法检测到的逻辑缺陷。这是可能的，因为 Claude Code 理解代码的语义含义，而不仅仅是语法。这是相对于传统静态分析工具的关键优势之一。

### 什么是 OWASP Top 10:2025？

OWASP Top 10:2025 是开放 Web 应用安全项目（OWASP）列出的十个最关键的 Web 应用安全风险的最新版本。它从 2021 版本更新以反映不断变化的威胁环境。值得注意的变化包括新增了 A03（软件和数据完整性/供应链）和 A10（异常条件）。claude-cybersecurity 覆盖了所有 10 个类别，每个类别都有专用的检测逻辑。

## 相关文章

* [claude-ads v1.5: 250+ 广告审计检查](/blog/claude-ads-v1-5-release) —— 这是 claude-cybersecurity 的首个实际测试项目
* [2026 年最佳 Claude Code 技能](/blog/best-claude-code-skills-2026) —— 顶级 Claude Code 技能完整指南
* [skill-forge：构建你自己的 Claude Code 技能](/blog/skill-forge-build-claude-code-skills) —— 在几分钟内构建并发布 Claude Code 技能
* [Claude Code 刚刚取代了你的整个 SEO 堆栈](/blog/claude-code-seo-stack) —— Claude Code 技能的另一个例子

## 加入 AI 营销中心

AI 工具、技能和营销自动化的免费社区

[立即加入](https://www.skool.com/ai-marketing-hub)
