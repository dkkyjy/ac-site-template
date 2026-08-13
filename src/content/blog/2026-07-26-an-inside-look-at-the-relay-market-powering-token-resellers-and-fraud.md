---
title: "揭秘支撑代币转售与欺诈的中继市场"
description: "揭秘支撑代币转售与欺诈的中继市场"
pubDate: "2026-07-26"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/26/relay-market"
---

> *提要：揭秘支撑代币转售与欺诈的中继市场
Matt Lenhard 对围绕通过汇集 API 密钥来折扣转售 LLM 代币而兴起的市场进行了一项引人入胜的调查。*

2026 年 7 月 26 日 - 链接博客

**[揭秘支撑代币转售与欺诈的中继市场](https://vectoral.com/blog/token-relay-market)**（[来源](https://news.ycombinator.com/item?id=49058993 "黑客新闻")）Matt Lenhard 对围绕通过汇集各方 API 密钥来折扣转售 LLM 代币而兴起的市场进行了一项引人入胜的调查。

这看来主要发生在中国。转售商出售访问 LLM 代理的权限，该代理提供比常规 API 定价大幅折扣的价格，他们通过滥用免费试用、通过未受保护的支持机器人进行代理，或有时通过盗刷信用卡或退款攻击来实现这一点。

他们用于这些代理的软件是开源的——主要是 [one-api](https://github.com/songquanpeng/one-api) 及其开发更活跃的分支 [new-api](https://github.com/QuantumNous/new-api)，这些都是合法的 API 代理产品，可用于在一组 API 凭证之间进行负载均衡请求。

买家则寻求低价代币、规避地域限制，在某些情况下还为了模型蒸馏而收集数据。

我一直谨慎避免将我的 LLM 驱动应用公开暴露，担心遭到滥用而导致巨额代币账单。这个市场的存在让我更加谨慎：现在有一个完整的生态系统可以从发现新的未受保护端点并进行利用中获利。

LLM 供应商**真的**需要更好地为其 API 密钥提供严格的限额。我希望我的 LLM 应用在达到我设定的某个时间段内的一美元阈值时立即停止工作。

这里是 [（中文）论坛帖子](https://www.v2ex.com/t/1196011)，它是 Matt 文章的主要信息来源。

发布于 [2026 年 7 月 26 日](/2026/Jul/26/) 晚上 7:30
