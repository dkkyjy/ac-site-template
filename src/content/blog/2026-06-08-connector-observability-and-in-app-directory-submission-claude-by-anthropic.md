---
title: "连接器可观测性与应用内目录提交 | Anthropic Claude"
description: "---"
pubDate: "2026-06-08"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/observability-for-developers-building-connectors"
---

# 连接器可观测性与应用内目录提交 | Anthropic Claude

---

## 监控、调试和优化连接器

已在[目录](https://claude.ai/directory/connectors)中发布的连接器现在拥有一个仪表板，展示其在各 Claude 产品界面中的运行表现。连接器所有者可以借此：

* **跟踪采用情况。** 监控活跃用户数、总工具调用量以及随时间变化的目录排名。
* **诊断错误和延迟。** 一览健康评分、错误率和延迟情况，并查看按工具分类的错误细分，以精准定位故障原因。**
* **按产品分析使用情况。** 对比 Claude、Claude Code、Cowork 等产品中的工具调用量，了解用户的活跃场景。

*连接器可观测性示意图。数据仅供参考。*

现已以公开测试版形式上线。在 [组织设置](https://claude.ai/admin-settings/organization) 中的 [目录](https://claude.ai/admin-settings/directory/submissions) 下可以找到该功能。需要团队版或企业版的管理员或所有者权限。在企业版中，所有者还可以通过具有目录管理或库权限的[自定义角色](https://support.claude.com/en/articles/13930452-manage-custom-roles-on-enterprise-plans)来委派访问权限。

## 加入目录

连接器基于[模型上下文协议（MCP）](https://modelcontextprotocol.io/docs/getting-started/intro)构建。目前[目录](https://claude.ai/directory/connectors)中已有超过 300 个第三方连接器，每天被数百万用户使用。如果您希望将自己的 MCP 服务器提交至目录，现在可以直接在 Claude 中完成操作。[了解更多](https://claude.com/docs/connectors/building/submission)。
