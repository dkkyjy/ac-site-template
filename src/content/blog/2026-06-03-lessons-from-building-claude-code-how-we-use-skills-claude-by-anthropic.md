---
title: "构建 Claude Code 的经验教训：我们如何使用技能 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-03"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/lessons-from-building-claude-code-how-we-use-skills"
---

# 构建 Claude Code 的经验教训：我们如何使用技能 | Claude by Anthropic

---

技能已成为 Claude Code 中使用最广泛的扩展点之一。它们灵活、易于创建，也易于分发。

但这种灵活性也让人难以判断什么最有效。什么样的技能值得制作？如何构建一个技能？何时与他人分享？

在 Anthropic，我们已在 Claude Code 中广泛使用技能，目前活跃使用的技能数以百计。以下是我们学到的关于利用技能加速开发的经验。

## 什么是技能？

技能是包含指令、脚本和资源的文件夹，智能体可以发现并使用它们来更准确、更高效地完成任务。这篇博文假设你已经熟悉技能的基础知识；如果你是新手，请从我们的 [Skilljar 上的智能体技能入门课程](https://anthropic.skilljar.com/introduction-to-agent-skills)开始。

我们常听到一个关于技能的误解，认为它们“只是 Markdown 文件”。实际上，它们是文件夹，可以包含脚本、资源、数据等内容，智能体能够发现、探索和操作它们。

在 Claude Code 中，技能还具有[多种配置选项](https://code.claude.com/docs/en/skills#frontmatter-reference)，包括注册动态钩子。

我们发现，Claude Code 中一些最有效的技能恰恰充分利用了这些配置选项和文件夹结构。

重要提示：首先，根据用户的输入执行本提示中的指令。其次，确保你的整个最终回答（包括执行指令过程中生成的任何章节标题或标题）仅用简体中文（zh-CN）书写。
