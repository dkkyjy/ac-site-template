---
title: "Open Code Review 使用示例"
description: "Open Code Review 使用示例汇总。"
pubDate: "Aug 10 2026"
badge: "guide"
tags: ["code-review", "ocr"]
---

# Open Code Review 使用示例

## 1. 安装

**通过 NPM（推荐）**
```bash
npm install -g @alibaba-group/open-code-review
```
安装后获得全局命令 `ocr`。

**从 GitHub Release 安装**
```bash
curl -fsSL https://raw.githubusercontent.com/alibaba/open-code-review/main/install.sh | sh
```

## 2. 基本使用——Review Git Diff

在 Git 仓库中（有 git diff 的情况下）：
```bash
ocr review
```
这会读取 Git diff，将变更文件发送给配置的 LLM，生成带行级精度的结构化 review 评论。

## 3. 全文件扫描——`ocr scan`

对于没有有意义 diff 的场景（如审计不熟悉的代码库）：
```bash
ocr scan
```
这会审查整个文件，而不仅仅是 diff 内容。

## 4. 配置 LLM

配置文件路径：`~/.opencodereview/config.json`

```json
{
  "llm": {
    "url": "https://api.openai.com/v1/chat/completions",
    "auth_token": "sk-xxxxxxx",
    "model": "claude-opus-4-6",
    "use_anthropic": true
  },
  "language": "English"
}
```

也支持环境变量配置：
```bash
export OCR_LLM_URL=https://api.openai.com/v1/chat/completions
export OCR_LLM_TOKEN=sk-xxxxxxx
export OCR_LLM_MODEL=claude-opus-4-6
export OCR_USE_ANTHROPIC=true
```

## 5. 自定义 Review Rules

创建 `~/.opencodereview/rules.json`（支持多层级：`--rule` > 项目配置 > 全局配置）：

```json
{
  "rules": [
    {"path": "**/*.java", "rule": "Check for null safety"},
    {"path": "**/*.js", "rule": "Check for XSS vulnerabilities"},
    {"path": "**/*.py", "rule": "Check SQL injection patterns"}
  ],
  "include": ["src/main/**/*.java", "lib/**/*.kt"],
  "exclude": ["**/generated/**", "vendor/**"]
}
```

## 6. CI 集成示例

可以集成到 CI 流水线中自动 review PR：
```bash
# 在 CI 中
git fetch origin main
git diff origin/main...HEAD | ocr review
```

## 7. 支持的平台/Agent

- **平台**: Windows / macOS / Linux
- **Agent**: Claude Code / Codex / Cursor
- **LLM 后端**: OpenAI / Anthropic / DashScope(通义) / DeepSeek / Z-AI

## 架构亮点

| 能力 | 说明 |
|------|------|
| **精准文件选择** | 确定哪些文件需要 review，哪些应过滤 |
| **智能文件捆绑** | 将相关文件分组到同一个 review 单元（如多语言 properties） |
| **细粒度规则匹配** | 按文件特性匹配 review 规则，模板引擎驱动更稳定 |
| **外部定位&反思模块** | 独立的位置修正和内容反思模块 |
| **Token 效率** | 相比通用 agent 仅消耗约 **1/9** 的 token |
| **Benchmark** | 50个开源仓库、200个真实PR、10种编程语言验证 |

更多详情请访问官方文档：[alibaba.github.io/open-code-review/](https://alibaba.github.io/open-code-review/)
