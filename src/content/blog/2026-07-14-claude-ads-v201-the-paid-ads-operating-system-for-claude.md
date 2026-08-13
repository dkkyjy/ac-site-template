---
title: "Claude Ads v2.0.1：面向 Claude 的付费广告操作系统"
description: "---"
pubDate: "2026-07-14"
heroImage: "/post_img.png"
tags: ["agrici-daniel"]
originalLink: "https://agricidaniel.com/blog/claude-ads-v2-0-1-release"
---

# Claude Ads v2.0.1：面向 Claude 的付费广告操作系统

---

## Claude Ads v2 已公开发布
我从架构层面重建了 Claude Ads。v2.0.0 是主版本发布。v2.0.1 是公开分发补丁，它使仓库、安装程序、支持、问题、讨论和安全报告路径能够通过开放公共镜像正常工作。

结果是为 Claude Code 打造了一个付费媒体操作系统。它涵盖审计、规划、创意工作流、监控、实验、报告以及跨 12 个广告平台精心控制的变更草案。

简要版本
* 12 个一等付费广告平台合约
* 506 个测试通过，23 个在验证环境中跳过
* 仓库审计在 339 个跟踪文件上通过
* 承载平台声明映射到带日期的证据
* 默认只读操作
* 版本化 JSON 作为记录系统

## 完整架构发布
这并非另一个带有更大命令菜单的提示包。Claude Ads v2 将产品合约、证据、能力、评分、安全和发布控制分离为明确的层次。

一个指挥者拥有范围与最终产物。有界工作者独立分析切片并返回符合模式的发现。必要工作者失败会使运行变为部分失败。它们永远不会隐藏在"完整审计"标签之后。

规范结果是版本化 JSON。Markdown、HTML 和可选的 PDF 报告均从同一已验证的捆绑包渲染生成。这消除了每个输出格式自行创建真理版本时产生的漂移。

## 12 个一等广告平台
Claude Ads v2 为每个平台提供专用的合约、专注技能、审计工作者、控制参考、能力声明、测试固定装置和可测试路由表面：
* Google Ads
* Meta Ads
* YouTube Ads
* LinkedIn Ads
* TikTok Ads
* Microsoft Advertising
* Apple Ads
* Amazon Ads
* Reddit Ads
* Pinterest Ads
* Snapchat Ads
* X Ads

共享 API 不会合并不同平台的结果。即使 Google Ads 提供数据，YouTube 仍然是其自己的审计表面。失败的平台将被报告为失败或缺失，而不会悄悄转换为零值。

## 无来源，无当前声明
平台规则变化太快，无法依赖基于记忆的建议。v2 引入了公共安全的来源和声明分类账，用于承载平台、API、政策、法规、基准和创意规格声明。

每个注册的声明都带有带日期的证据、检索日期、置信度和刷新截止日期。过时的证据会使依赖的结果变为暂定。不受支持的声明会被降级，而不会作为当前平台事实呈现。

相同的故障关闭方法也适用于评分。目录行并不自动成为健康控制。如果平台评分档案未获批准，Claude Ads 将扣留健康评分，而不是自行发明严重性或类别权重。

## 默认只读
Claude Ads 可以将授权的导出或账户读取转化为观察结果、发现、计划、创意工作流、实验、监控和报告。它不假定分析权限即为更改账户的权限。

实时更改需要针对具体操作启用的能力、明确的账户和对象 ID、人类可读的变更前后差异、所有者批准、幂等键、审计记录、验证窗口和回滚流程。

缺少上限意味着无法写入。永久删除不在 v2 范围内。默认设置依然简单：先检查，再起草，只有在每一道关卡通过后才能进行更改。

## 506 个测试通过
在标记的 v2.0.1 版本上，本地测试套件在当前环境中完成了 **506 个测试通过**，23 个跳过。仓库审计也在 339 个跟踪文件上通过。

审计检查路径可移植性、敏感内容边界和清单一致性。测试涵盖合约、适配器、标准化、路由、评分、报告、发布控制、安装程序安全性、隐私和 URL 防御。

这些数字是经过测试的仓库行为的证据。它们并非声称每个可选的实时集成都已启用。能力清单仍然是每个平台能够读取、起草、应用、验证或回滚的权威依据。

**透明度说明：** v2.0.1 发布说明还披露了可选的创意和报告路径中存在的五个 Pillow 12.2.0 安全公告。依赖审计公开标记了这些公告，同时正在准备证据完整的依赖更新。

## v2.0.1 变更内容
v2.0.1 不改变 v2 的代码合约、评分、目录或行为。它在 v2.0.0 之上更新了公共发布表面。
* 面向用户的仓库链接现在指向公共 AgriciDaniel 仓库
* 默认克隆源现在使用公共仓库
* 支持、问题、讨论和私有漏洞报告不再需要组织访问权限
* README 文档记录了公共和社区分发模型
* 原生 Claude Code 插件安装已在发布中记录

这一区别很重要。v2.0.0 是架构发布。v2.0.1 是通往该架构的干净公共入口。

## 安装或更新 Claude Ads
推荐使用原生 Claude Code 插件流程：
```
/plugin marketplace add AgriciDaniel/claude-ads
/plugin install claude-ads@ai-marketing-hub-claude-ads
```
如果 Claude Ads 已安装，请向 Claude 提示：
使用原生插件流程从公共仓库将 Claude Ads 更新到 v2.0.1，然后验证安装并报告已安装的版本。

阅读 [公共仓库](https://github.com/AgriciDaniel/claude-ads)，查看 [v2.0.1 发布说明](https://github.com/AgriciDaniel/claude-ads/releases/tag/v2.0.1)，或 [观看构建和发布视频](https://www.youtube.com/watch?v=rz3dpN9wZB0&t)。

## 常见问题

### v2.0.1 是架构发布吗？
v2.0.0 是完整的架构发布。v2.0.1 是文档和元数据补丁，它更新了公共仓库、安装程序、支持和发布路径，而不改变 v2 的行为合约。

### 支持哪些广告平台？
Claude Ads v2 为 Google、Meta、YouTube、LinkedIn、TikTok、Microsoft、Apple、Amazon、Reddit、Pinterest、Snapchat 和 X Ads 提供了一等表面。

### Claude Ads 会自动更改广告账户吗？
不会。Claude Ads 默认只读。实时更改需要精确的能力支持、预览、明确的所有者批准、幂等性、验证、审计记录和回滚。

### 每次审计都会获得健康评分吗？
不会。当证据覆盖不足或适用的平台评分档案被禁用时，Claude Ads 会扣留健康评分。未知发现会降低证据覆盖，但不会转换为失败。

### 如何更新现有安装？
使用原生 Claude Code 插件流程，要求 Claude 从公共仓库更新到 v2.0.1。更新后运行验证工作流，并在对账户数据使用前确认已安装的版本。

## 相关文章
* [Claude Ads v1.7.1：SSS+ 打磨与已验证引用](/blog/claude-ads-v1-7-1-release)
* [Claude Ads v1.5：跨 7 个平台的 250+ 广告审计检查](/blog/claude-ads-v1-5-release)
* [Claude Code 刚刚取代了你的广告代理](/blog/claude-code-ad-agency)
* [我日常使用的开源 AI 营销技术栈](/blog/ai-marketing-automation-stack)

构建更好的付费媒体系统
加入 AI 营销中心，获取开源工具、工作流模板和实用实施支持。

[免费加入](https://www.skool.com/ai-marketing-hub) [升级专业版](https://www.skool.com/ai-marketing-hub-pro)
