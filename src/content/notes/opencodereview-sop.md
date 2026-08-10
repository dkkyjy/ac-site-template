---
title: "OpenCodeReview - AI 代码审查"
description: "基于 alibaba/open-code-review 的 Git Diff 代码审查能力。"
pubDate: "2026-08-10"
badge: "guide"
tags: ["review", "ai", "code"]
---

# OpenCodeReview SOP — AI 代码审查能力

## 定位
基于 alibaba/open-code-review 的 Morphling 蒸馏产物。提供 Git Diff-based 代码审查能力，可直接作为 GA 系统的 L3 SOP 调用。

## 前置条件
- 目标目录必须是 Git 仓库
- 需要配置 LLM（通过环境变量）

## LLM Provider 配置

支持以下 Provider（通过环境变量切换，优先级从上到下）：

| Provider | 环境变量 | 默认 API URL |
|---------|---------|-------------|
| Anthropic | `ANTHROPIC_API_KEY` + `OCR_USE_ANTHROPIC=true` | `https://api.anthropic.com` |
| OpenAI | `OPENAI_API_KEY` | `https://api.openai.com/v1` |
| DashScope | `DASHSCOPE_API_KEY` | `https://dashscope.aliyuncs.com/compatible-mode/v1` |
| DeepSeek | `DEEPSEEK_API_KEY` | `https://api.deepseek.com` |
| Kimi | `MOONSHOT_API_KEY` | `https://api.moonshot.cn/v1` |
| 自定义 | `OCR_LLM_URL` + `OCR_LLM_TOKEN` + `OCR_LLM_MODEL` | - |

其他环境变量：
- `OCR_LLM_MODEL` — 模型名称
- `OCR_USE_ANTHROPIC` — 设为 `true` 使用 Anthropic 协议

## 工作模式

### 模式 1: Diff-Review（增量审查）
审查 Git Diff 变更（commit / branch / unstaged）。

```
用户请求 → 解析Diff → 过滤无变更文件 → 规划（PLAN）→ 逐文件LLM审查 → 收集评论 → 输出
```

### 模式 2: Full-Scan（全量审查）
审查整个仓库/目录的所有文件。

```
用户请求 → 枚举文件 → 过滤（类型/大小）→ 批量分配 → 逐文件LLM审查 → 去重 → 输出
```

## 审查提示词（从目标项目剥离）

### 系统提示词（审查角色）
```
You are a code review assistant. You are skilled at code review in the software development process and are responsible for providing professional review feedback for code changes that are about to be submitted.

Capabilities:
- Think step by step progressively.
- First understand the code changes (provided in Unified Diff format).
- Be objective and neutral, make judgments based on facts and logic.
- For the current code changes, provide feedback pointing out areas for improvement or potential issues.
- Focus on newly added code.
- Avoid commenting on correct code or unchanged code.
- Focus on clarity, practicality, and comprehensiveness.

Strict Focus Rules:
- Context tools are for understanding only. Findings from other files must NOT become the subject of your comments.
- If you discover a potential issue in another file while gathering context, ignore it — your task is limited to the current diffs.

Reply limit:
- If the task is complete, call task_done.
- If a code issue is confirmed, call code_comment.
- If additional context is needed, call appropriate context tool.
```

### 审查工具定义（参考目标项目 tools.json）

tools:
1. **task_done** — 完成审查时调用
2. **code_comment** — 提交行级审查评论（含 `comments` 数组，每个含 `content` + `existing_code`）
3. **file_read** — 读取文件内容（用于获取上下文）
4. **file_find** — 按文件名搜索
5. **file_read_diff** — 查看特定文件的 Diff
6. **code_search** — 全文代码搜索

### 审查规则系统

规则按优先级从高到低：
1. 命令行 `--rule` 参数
2. `<repo>/.opencodereview/rule.json`
3. `~/.opencodereview/rule.json`
4. 内置系统默认规则

规则格式：
```json
{
  "rules": [
    {"path": "**/*.java", "rule": "All new methods must validate required parameters for null", "merge_system_rule": true}
  ],
  "include": ["src/**/*.java"],
  "exclude": ["**/test/**", "**/generated/**"]
}
```

### 内置语言/框架审查规则（17种）

| 路径模式 | 聚焦领域 |
|---------|---------|
| `**/*.java` | NPE、线程安全、事务、异常处理 |
| `**/*.{ts,js,tsx,jsx}` | XSS、SQL注入、类型安全、异步错误 |
| `**/*.rs` | 内存安全、unsafe代码、并发 |
| `**/*.{cpp,cc,hpp}` | 内存管理、RAII、未定义行为 |
| `**/*.c` | 缓冲区溢出、指针安全、资源泄漏 |
| `**/*.{kt}` | 空安全、协程 |
| `**/*.ets` | 鸿蒙 ArkTS 特定 |
| `**/pom.xml` | Maven 依赖/插件配置 |
| `**/build.gradle` | Gradle 构建配置 |
| `**/package.json` | npm 依赖/脚本配置 |
| `**/Cargo.toml` | Rust 包配置 |
| `**/*.{json,json5}` | JSON 格式安全 |
| `**/*.{yaml,yml}` | YAML 配置安全 |
| `.github/workflows/**` | CI/CD 安全性 |
| `**/*{mapper,dao}*.xml` | MyBatis SQL 注入 |
| `**/*.properties` | 配置注入 |
| default | 通用错误处理、日志安全性 |

## 执行步骤

### 步骤 1: Diff 解析
```python
# 使用 git diff 获取变更
git diff <from>...<to>           # 分支间 diff
git diff <commit>^...<commit>    # commit diff
git diff                          # unstaged diff

# 解析 unified diff 为文件列表
```

### 步骤 2: 文件过滤
- 按扩展名白名单过滤
- 按.gitignore/排除模式过滤
- 超过2MB的文件跳过

### 步骤 3: 审查计划（可选，对大量变更时启用）
- 按文件分组
- 评估每文件复杂度
- 决定工具调用预算分配

### 步骤 4: LLM 审查循环
```
for each file:
  1. 构建提示词（含文件diff+系统规则）
  2. LLM 调用（Tool-Use 模式）
  3. 处理工具调用（file_read/code_search/code_comment）
  4. 循环直到 task_done 或超限
  5. 收集 comments
```

### 步骤 5: Review Filter（伪证过滤）
对收集的评论执行第二阶段审查（专用 LLM 调用），过滤伪证：
- **Prompt**：`review_filter_task_system.md` — "You are a fact-checker"
- **核心**：只能确认错误，不能确认正确（仅依赖当前 diff）
- **输出**：可证伪的评论 ID 列表（JSON array）
- **目标**：Precision 优先，宁可遗漏不误报

### 步骤 6: 结果汇总
- 按优先级排序评论
- 高优先级：明显bug/安全问题
- 中优先级：合理但需进一步上下文
- 低优先级：自动丢弃

## 内存压缩（Memory Compression）
大量文件审查时自动压缩历史：
- **压缩时机**：超过文件阈值 (pool.go)
- **压缩方法**：5维度结构化摘要
- **维度**：Issues / Tool Conclusions / Completed / Pending / Focus
- **Prompt**：`memory_compression_task_system.md`
- **效果**：继续当前状态，不需重启对话

## diff-review 与 full-scan 的提示词差异

**Diff-Review 提示词重点**:
- 关注新增/修改的代码行
- 不要评论未变更或删除的代码
- Diff 在 `<change_files>` 中提供

**Full-Scan 提示词重点**:
- 审查整个源文件（无diff上下文）
- 识别真实缺陷/bug/安全/性能/可维护性问题
- 文件内容在 `<current_file_content>` 中提供

## 输出格式

每个评论包含：
- `path` — 文件路径
- `content` — 审查评语文本
- `start_line` / `end_line` — 行号范围
- `suggestion_code` — 可选的修复建议
- `priority` — high/medium/low

## 完整示例（参考目标项目）

### 审查一个 commit
```bash
# 目标项目 CLI 方式（参考）
ocr review --commit abc123 --output json

# GA SOP 调用方式（Python）
from ocr_agent import ReviewAgent
agent = ReviewAgent(repo_dir="/path/to/repo")
comments = agent.review_commit("abc123")
```

### 审查 unstaged 变更
```python
comments = agent.review_unstaged()
```

---

## CI/CD 集成示例

### GitHub Actions
```yaml
name: Code Review
on: [pull_request]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with: {fetch-depth: 0}
      - name: OpenCodeReview
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          npx @alibaba-group/open-code-review review --from origin/${{ github.base_ref }} --to ${{ github.sha }}
```

### GitLab CI
```yaml
review:
  stage: test
  script: |
    curl -Lo ocr https://github.com/alibaba/open-code-review/releases/latest/download/opencodereview-linux-amd64
    chmod +x ocr
    export ANTHROPIC_API_KEY=$ANTHROPIC_API_KEY
    ocr review --from $CI_MERGE_REQUEST_DIFF_BASE_SHA --to $CI_COMMIT_SHA
```

---

*本 SOP 基于 alibaba/open-code-review 蒸馏（2026-06-26）*
*形态：混合型 Morphling（核心流程重写 + prompt设计调用 + CLI/session/telemetry 舍弃）*