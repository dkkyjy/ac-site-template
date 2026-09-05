---
title: "使用Claude构建商务代理 | Claude by Anthropic"
description: "---"
pubDate: "2026-09-02"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/claude-for-commerce-agents"
---

# 使用Claude构建商务代理 | Claude by Anthropic

---

全球许多最大的零售商、市场平台、电子商务平台和旅游公司都在使用Claude构建让购物更便捷的代理。Shopify、Priceline等企业客户已拥有让消费者能够使用AI以自然语言搜索所需商品、查找、比较并购买的代理。

今天，我们推出了一份蓝图，帮助在Claude上构建商务代理。它包含工程团队在数天内让商务代理运行所需的框架、模式和防护措施，并提供了面向零售、旅游、电信和票务平台的购物代理和商家代理的参考实现。此外，还附带一个Claude Code插件，助您快速上手。

代码可部署在您已使用Claude构建的环境中，包括Claude API、Amazon Bedrock、Microsoft Foundry或Google Cloud Vertex AI。您还可以与我们的解决方案和生态系统合作伙伴（如Accenture、Mastercard和Visa）合作，他们正与我们携手，帮助客户和商家社区利用这些蓝图。

该蓝图[现已可用](https://github.com/anthropics/commerce-agents)，每个垂直领域均配有[实时演示](https://claude.com/solutions/commerce)，并提供了关于构建方式的[工程深度解析](http://claude.com/blog/the-anatomy-of-effective-commerce-agents)，恰逢假日季规划之际。

*在ACME* [*零售示例*](https://claude.com/solutions/commerce) *中运行的购物代理。*

## 蓝图内容

该代码库包含购物代理和商家代理的完整可运行实现，可使用[Messages API](https://platform.claude.com/docs/en/intro)、[Agent SDK](https://code.claude.com/docs/en/agent-sdk)或[Claude Managed Agents](https://platform.claude.com/docs/en/managed-agents/overview)（测试版）构建。您可以在编写任何代码之前通过自助演示查看其运行情况，然后使用Claude Code根据您的目录、政策、品牌等进行定制。

### 购物代理

购物代理嵌入您的应用或网站中。蓝图包含目录、购物车、结账、客户偏好和订单历史的集成点，并将支付环节留给您自己处理，无论是使用现有结账流程还是代理支付提供商。

客户可以说“我需要一顶帐篷、一个睡袋和一个炉子，供带两个孩子度周末使用”，代理即可接手处理。它能够：

* 搜索目录并组装正确的商品组合，包括多商品请求。
* 记住客户的偏好并定制推荐内容。
* 在对话中直接展示产品、对比和购物车，而不仅仅是文本。
* 构建购物车并将其交给结账流程。
* 在同一对话中回答客户服务问题，如订单位置、如何退货或换货、退款政策内容等，而无需将客户引导至支持页面。

该代理配备防护措施，旨在将价格和产品限制在实际目录数据范围内，并避免操纵性追加销售模式。在代码库中，这些以技能和工具的形式提供，涵盖目录搜索、多商品规划、深度研究、个性化、客户关怀和对话内界面。

### 商家代理

商家代理为店铺运营人员提供支持。用户可以问“我们应该对哪些商品打折以清掉上一季的库存？”并根据自身数据获得答案。它能够：

* 回答销售表现相关问题，如哪些商品畅销、哪些滞销。
* 跟踪库存并主动标记问题，如某商品在促销开始前即将售罄。
* 根据店铺自身的销售历史推荐定价和促销方案。
* 起草营销活动以推动需要清仓的商品销售。

当代理主动建议变更时，任何内容上线前都需人工批准，这意味着用户拥有最终决定权，而代理则负责监控店铺。在代码库中，这些能力以技能形式提供，涵盖销售分析、目录和库存管理、营销和促销，以及门户内界面（如图表和仪表板）。

## 深受行业信赖

服务购物者、旅行者、订阅用户和商家的公司都在Claude上构建和运行代理。以下是他们对使用Claude构建商务代理的评价：
