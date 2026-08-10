---
title: "every_skills — 外部技能库索引"
description: "外部技能库(superpowers等)的插件/技能目录索引。"
pubDate: "2026-08-10"
badge: "reference"
tags: ["skills", "index", "reference"]
---

# every_skills_sop — 外部技能库索引

路径: `memory/every_skills/` → `/Users/dkk/storage/github/every`
格式: Claude Code/Codex SKILL.md + scripts

使用方式: read `memory/every_skills/<plugin>/<skill-name>/SKILL.md` 获取完整技能

---

## superpowers (15个技能)
- brainstorming: 创意工作前的需求/设计探索
- dispatching-parallel-agents: 拆分独立并行任务
- executing-plans: 执行实现计划
- finishing-a-development-branch: 完成开发分支收尾
- receiving-code-review: 处理代码审查反馈
- requesting-code-review: 提交代码审查
- subagent-driven-development: 子代理驱动开发
- systematic-debugging: 系统化调试流程
- test-driven-development: TDD开发
- using-git-worktrees: Git工作树隔离
- using-superpowers: 新会话入口导航
- verification-before-completion: 完成前验证
- writing-plans: 多步骤任务计划编写
- writing-skills: 技能编写与测试

## compound-engineering (40个技能)
- ce-agent-native-architecture: 原生Agent架构设计
- ce-agent-native-audit: Agent架构审计
- ce-brainstorm: 需求探索与协作型脑暴
- ce-clean-gone-branches: 清理已删除远程分支的本地分支
- ce-code-review: 结构化代码审查（多层角色+置信度门控）
- ce-commit-push-pr: 一键提交推送并创建PR
- ce-commit: 高质量Git提交
- ce-compound-refresh: 刷新技能索引
- ce-compound: 复合工作流
- ce-debug: 调试支持
- ce-demo-reel: 演示录制
- ce-dhh-rails-style: DHH式Rails风格
- ce-doc-review: 文档审查
- ce-frontend-design: 前端设计
- ce-gemini-imagegen: Gemini图片生成
- ce-ideate: 方案构思（含问题溯源）
- ce-optimize: 代码优化
- ce-plan: 计划制定
- ce-polish-beta: 打磨优化(Beta)
- ce-product-pulse: 产品脉搏
- ce-proof: Proof编辑器协作
- ce-release-notes: 发布说明
- ce-report-bug: 报告Bug
- ce-resolve-pr-feedback: 处理PR审查反馈
- ce-riffrec-feedback-analysis: 产品反馈分析
- ce-session-extract: 会话文件深度提取
- ce-session-inventory: 会话文件扫描发现
- ce-sessions: 会话历史搜索与问答
- ce-setup: 环境诊断与配置
- ce-simplify-code: 代码简化与重构
- ce-slack-research: Slack组织知识检索
- ce-strategy: 策略文档编写维护
- ce-test-browser: 浏览器测试
- ce-test-xcode: iOS/Xcode测试
- ce-update: 更新
- ce-work-beta: [Beta]外部委托工作模式
- ce-work: 高效工作执行
- ce-worktree: Git工作树创建
- lfg: 全自动化工程流水线(plan→work→review→test→commit→push→PR→CI→fix)

## compound-knowledge (6个技能)
- kw-brainstorm: 知识工作前的脑暴/知识整理
- kw-compound: 知识工作后提取保存学习成果
- kw-confidence: 自信度检查，识别知识盲区
- kw-plan: 研究过往+结构化知识工作计划
- kw-review: 多审稿人质量检查
- kw-work: 执行知识工作计划

## coding-tutor (1个技能)
- coding-tutor: 个性化编程辅导（基于现有知识和代码库）

## open-slide (8个技能)
- frontend-design: 前端UI设计（Vercel风格）
- vercel-composition-patterns: Vercel组合模式
- vercel-react-best-practices: React/Next.js性能优化
- web-design-guidelines: UI审核/可访问性审计
- apply-comments: 应用@slide-comment标记
- create-slide: 创建幻灯片
- create-theme: 创建幻灯主题
- slide-authoring: 幻灯片编辑技术参考

## llm-wiki (1个技能)
- wei-llm-wiki: 知识库摄入/查询/lint/进化(GitHub/微信/本地/网页) | 触发: wiki init/ingest/query/lint/refresh/log/evolve

## understand-anything (8个技能)
- understand: 代码库知识图谱分析
- understand-chat: 基于知识图谱的代码问答
- understand-dashboard: 知识图谱可视化仪表盘
- understand-diff: Git差异分析
- understand-domain: 业务领域知识提取
- understand-explain: 文件/函数/模块深度解释
- understand-knowledge: LLM Wiki知识库图谱分析
- understand-onboard: 新成员上手指南生成

---

## 快速检索
- 开发流程: superpowers/* | ce-brainstorm | ce-plan | ce-work | ce-code-review
- 代码质量: ce-simplify-code | ce-optimize | ce-polish-beta | systematic-debugging
- PR/Git: ce-commit | ce-commit-push-pr | ce-resolve-pr-feedback | ce-worktree | using-git-worktrees
- 知识管理: wei-llm-wiki(文章摄入/查询/进化) | kw-* | understand-* | ce-slack-research
- 知识图谱: understand | understand-knowledge | understand-domain
- 前端设计: frontend-design | web-design-guidelines | vercel-react-best-practices
- 测试: test-driven-development | ce-test-browser | ce-test-xcode
- 全自动化: lfg