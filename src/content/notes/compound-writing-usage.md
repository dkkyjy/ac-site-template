---
title: "Compound Writing 写作技能库 · 使用示例"
category: "技能库"
description: "Every 出品的 context-first 写作工具箱（33 个 cw-* 技能）怎么用：写作之家、上下文加载序、七类实战示例与全部技能清单。"
pubDate: "2026-10-08"
badge: "guide"
tags: ["writing", "skills", "cw", "compound-writing"]
---

# Compound Writing 写作技能库 · 使用示例

## 📌 概述

**Compound Writing** 是 [Every](https://every.to) 开源的一套 **context-first 写作工具箱**（MIT），覆盖「找素材 → 大纲 → 成稿 → 结构编辑 → 逐行编辑 → 压力测试 → 发布前检查」全链路，共 **33 个 `cw-*` 技能**。

它的设计目标不是"替你写"，而是 **不把作者磨平**：把"作者怎么说话"和"文章要做什么"分开存成可复用上下文，每次写作只加载当前需要的那一步。

> 版本: **v2.4.1** | 项目: [github.com/EveryInc/compound-writing](https://github.com/EveryInc/compound-writing) | 本机安装记录见 [技能库总索引](/notes/every-skills-sop)

本机（macOS）安装落点：

| 位置 | 内容 |
|---|---|
| `~/.agents/skills/cw-*` | 33 个技能实体（唯一实体层） |
| `~/.claude/skills`、`~/.codex/skills`、`~/.cursor/skills`、`~/.pi/agent/skills` … | 全部 symlink 指向上一行 |
| `~/.agents/references/`、`~/.agents/defaults/` | 共享契约与模板（**必须一起装**，多个技能用相对路径引用） |
| `~/storage/github/every/compound-writing` | 源码镜像 |

---

## 🧭 核心机制：写作之家 + 上下文加载序

### 写作之家（writing home）

技能不靠"记住你"来保持一致性，而是靠一个**可移植的文件夹**：

```text
writing-home/
├── VOICE.md        # 句子怎么响：句法、用词、语气
├── STYLE.md        # 文章要做什么：论点、证据、结构、实质标准、发布就绪度
├── AUDIENCE.md     # 可选：读者处境、已有知识、兴趣、抵触点
├── examples/       # 正/反例，为上面两条规则提供证据
└── drafts/
    └── piece-slug/
        ├── research.md      # 来源、论断、张力、出处
        ├── notes.md         # 原始素材
        ├── outline.md       # 当前结构
        ├── draft-version-one.md
        └── review.md        # 可选：编辑发现
```

两条最重要的分工：**VOICE 管"怎么写"，STYLE 管"必须做到什么"**。改一处措辞 → 归 VOICE；改一条论证/证据/结构规则 → 归 STYLE（这就是 `cw-save` 的路由逻辑）。

### 上下文权威顺序

每次开工按此顺序加载，**上层覆盖下层**：

1. 用户显式指令与提供的材料
2. 仓库 / 工作区指令
3. 它们指定的全局身份、偏好、规则、声音文件
4. 当前写作之家的 `VOICE.md` / `STYLE.md` / `AUDIENCE.md`
5. `examples/` 里相关的范例
6. 任务笔记、来源、研究、大纲、草稿与目标平台要求
7. 遗留 `TASTE.md`（仅当项目仍维护它）
8. 插件默认值（**只补未决缺口**）

> 这意味着：**先喂上下文，再要输出**。同一个技能，在写了 `VOICE.md` 的目录里跑，和在空目录里跑，产物完全是两回事。

---

## 🚀 三种调用方式

### 1️⃣ 自然语言（推荐入口 `cw-scribe`）

不用记技能名，直接说结果。`cw-scribe` 是**编排层**：先看你有什么现成材料，再挑**最小够用**的工作流。

```text
帮我把上周那篇关于缓存失效的笔记，改成一篇能发出去的博客。
```

```text
我这篇讲的东西没错但读起来像没人味的周报，帮我看看问题在哪。
```

首次有效交互时，Scribe 会检查"有没有草稿 / 有没有工作区 / 有没有可用上下文"：
- **有材料** → 直接干活，不打断你做校准访谈；
- **完全没有** → 简短说明"一个写作之家"的好处，问清落点，再建目录并开始 `VOICE.md` / `STYLE.md` 的引导（`cw-onboarding`）。

### 2️⃣ 点名技能 / 斜杠命令

已经知道要哪把刀，就直接点名（Claude Code 里是 `/cw-draft`，其他 runtime 直接说"用 cw-draft 处理 xxx"）：

```text
/cw-outline        把 interview 的素材整理成结构
/cw-hook            给这个内容出 3 个开头方案
/cw-nemesis         用最不宽容的读者视角攻击我这份稿子
```

辅助命令：`/cw-help`（上手导航）、`/cw-commands`（命令总表）。

### 3️⃣ 手动搭工作区（CLI，可脱离 Agent 跑）

```bash
# 建一个写作之家（含可选的 AUDIENCE.md 模板）
python3 ~/.agents/skills/cw-setup-project/scripts/create_project.py ~/writing/mybook --with-audience

# 只补缺失项，不覆盖已写内容（幂等）
python3 ~/.agents/skills/cw-setup-project/scripts/create_project.py ~/writing/mybook --add-missing
```

实测产物：`VOICE.md`、`STYLE.md`、`AUDIENCE.md`、`examples/`、`drafts/`（含 `project-template` 下的 `AUDIENCE/STYLE/VOICE` 与 `drafts/README.md`）。第二次带 `--add-missing` 跑会输出 `Preserved existing`，不动你的内容。

> 对话式也能达到同样效果：`/cw-setup-project`。

---

## 🎯 七类实战示例

### 示例 1 · 从零写一篇（起草链）

```text
（还没有具体想法）
/cw-brainstorm 我最近总在想"为什么代码审查会退化成礼貌的沉默"
```

```text
（有想法但没结构）
/cw-interview 我想写的是：团队里没人反对，但也没人真的同意。
```

```text
/cw-outline
```

`cw-outline` 用 **10% / 30% 框架**：

| 层级 | 含义 | 用途 |
|---|---|---|
| **10% 大纲** | 只用少量几行给出"形状" | 快速确认方向："这是 10% 大纲——形状对吗？" |
| **30% 大纲** | 完整形状的压缩形态：主要章节 + 每节要点 + 将放什么材料 + 章节如何衔接（**不是 30% 的正文**） | 长文、复杂主题，或"想先看清全貌再动笔"时 |

```text
/cw-draft 按 30% 大纲写，论点归我，来源要标出出处
```

> `cw-draft` 的硬约束：**保留作者声音、论点归作者、来源可追溯**。它不是"给你一篇 AI 文章"，是"把你的材料铺成一稿"。

### 示例 2 · 我手里已经有稿子（结构 → 句子）

```text
/cw-dev-edit drafts/cache-article.md    # 先看大结构：论点/利益/证据/收束
/cw-bluf                                # 最重要的那句话，是否出现在它该在的位置？
/cw-line-edit drafts/cache-article.md   # 结构稳了再逐句打磨（保声、保义、保"有用的怪"）
/cw-tracks                              # 擦掉脚手架：过程叙述、"如我们所见"这类到达痕迹
/cw-final-pass                          # 发布就绪裁决：ready / almost-ready / needs-work
```

`cw-bluf` 默认**只诊断不改写**：它比较"开头在前景化了什么"与"文章实际交付了什么"，判断延迟是否**挣得**（earned），然后给出**最小的结构位移**建议。

### 示例 3 · 压力测试（把稿子往死里打）

```text
/cw-nemesis     # 最恶意的读者：怀疑每个论据、每个假设
/cw-objections  # 读者的"但是呢…"清单
/cw-panel       # 召集多视角评审团，合成共识 / 张力 / 优先级建议
/cw-debate      # 让评审员互相反驳，直到张力解决或明确成"选择"
```

区别记法：

| 技能 | 行为 |
|---|---|
| `cw-panel` | 多视角**汇总**（synthesize） |
| `cw-debate` | 多视角**互相交锋**（respond to each other），若干轮 |
| `cw-nemesis` | 单视角，**最不宽容** |
| `cw-objections` | 单视角，聚焦**反论与"Yeah but"** |

### 示例 4 · 风格透镜（一键换一副眼镜）

```text
/cw-hemingway    # 砍：逐个数落形容词/副词/多余词，逼你杀掉心爱之物
/cw-vonnegut     # 冯内古特八条：贴近结尾开场、人物要有想要的东西、当个虐待狂…
/cw-sorkin       # 节奏与动量：是在边走边说，还是站着不动？
/cw-hitchcock    # 悬念：桌子底下的炸弹在哪？谁知道什么、什么时候知道？
/cw-sedaris      # 找笑点与自嘲：哪里本来可以更好笑、更具体、更把自己搭进去
/cw-mom          # 慈爱但没跟上的读者：哪里把普通读者弄丢了
/cw-reader       # 冷读：第一次读到的人会困惑/被劝退/停下不读的地方
```

### 示例 5 · 去 AI 腔与声音漂移

```text
/cw-ai-check     # 扫 AI 残留：空话连接组织、认知膨胀、过度完成的论证、表演式口吻、
                 # 模板化结构、空洞比喻、套话、企业抽象语、含糊对冲、假热情
/cw-voice-check  # 这段还像我吗？漂移了就给出更贴近的改法
```

`cw-ai-check` 自带**词表**（`references/ai_tells_lexicon.csv`），条目形如：

| 类别 | 例子 | 严重度 | 为什么读着像 AI |
|---|---|---|---|
| Opener | `In today's fast-paced world` | High | 套话式无上下文开场 |
| Opener | `Let's dive in` | High | 模板化过渡 |
| Opener | `In the ever-evolving landscape of [X]` | High | 空洞框架 |

可执行建议通常长这样：*"从一个具体事实、日期或场景开始；点出专有名词；删掉清嗓子式的开场。"*

### 示例 6 · 微部件生成器（小工具，高频用）

| 技能 | 作用 |
|---|---|
| `cw-hook` | 出 3 个开头钩子 |
| `cw-promise` | 出 3 个"读者将会得到什么"的承诺 |
| `cw-thesis` | 出 3 个中心论点选项 |
| `cw-transition` | 生成两节之间的过渡（`/cw-transition [从哪节到哪节]`） |
| `cw-analogy` | 为难点找类比（`/cw-analogy [要解释的概念]`） |
| `cw-simplify` | 把复杂文本改写成更朴素的语言 |

### 示例 7 · 没有单一技能适配时：组合与沉淀

```text
/cw-emergent   # 组合既有技能与运行时能力，做开放式目标：
               # 跨草稿分析、来源-大纲对照、多轮修订流水线
```

```text
/cw-save 以后所有稿子都别用"综上所述"，收束段直接落到具体动作
```

`cw-save` 会把这条确认过的偏好**路由到正确的持久文件**：措辞/句法/语气 → `VOICE.md`；论点/证据/结构/发布标准 → `STYLE.md`；读者认知 → `AUDIENCE.md`。它**不会**偷偷改已安装的插件，也不宣称拥有"私密记忆"。

---

## 📚 全部 33 个技能

### 构思（Ideate）

| 技能 | 做什么 |
|---|---|
| `cw-brainstorm` | 还没有明确想法时，把原始素材翻出来 |
| `cw-interview` | 把已有想法的素材、利害、思考**问**出来（不替你想） |
| `cw-thesis` | 生成可能的中心论断 |
| `cw-promise` | 明确读者该期待/获得什么 |
| `cw-outline` | 用 10%/30% 框架把素材组织成结构 |
| `cw-hook` | 生成开头选项 |
| `cw-transition` | 生成节间过渡 |
| `cw-analogy` | 为抽象概念找具体说法 |
| `cw-simplify` | 改写成更朴素的语言 |

### 起草与修订（Draft & Revise）

| 技能 | 做什么 |
|---|---|
| `cw-draft` | 笔记/来源/大纲/半成品 → 完整一稿（保声、保义、保住出处） |
| `cw-bluf` | 最重要的想法是否出现在该在的位置 |
| `cw-dev-edit` | 论点、结构、利害、证据、收束的**大图**编辑 |
| `cw-line-edit` | 句子与词级打磨，不磨平声音 |
| `cw-voice-check` | 诊断声音漂移并给出更贴近的改法 |
| `cw-ai-check` | 清机器味残留 |
| `cw-tracks` | 删脚手架、过程叙述、思考残留 |
| `cw-final-pass` | ready / almost-ready / needs-work 裁决 |

### 压力测试与透镜（Pressure-Test & Lens）

| 技能 | 做什么 |
|---|---|
| `cw-objections` | 读者最强抵抗与反论 |
| `cw-panel` | 多视角评审团 + 合成 |
| `cw-debate` | 评审员互相交锋 |
| `cw-emergent` | 无单一技能适配时自组工作流 |
| `cw-nemesis` | 最不宽容的读法，攻击弱论断 |
| `cw-hemingway` | 砍字，要求经济 |
| `cw-hitchcock` | 悬念、张力、读者何时知道什么 |
| `cw-reader` | 首次阅读体验的追踪 |
| `cw-mom` | 聪明但没跟上的普通读者 |
| `cw-sedaris` | 具体化、幽默、自我牵连 |
| `cw-sorkin` | 节奏、动量、前进感 |
| `cw-vonnegut` | 故事基本功：想要、利害、人物、有目的的句子 |

### 编排与上下文（Front door & Context）

| 技能 | 做什么 |
|---|---|
| `cw-scribe` | **总入口**：加载上下文，按结果挑最小工作流 |
| `cw-setup-project` | 建/迁移自包含写作文件夹 |
| `cw-onboarding` | 从短对话/旧作/既有上下文生成或刷新 `VOICE.md` / `STYLE.md`（可选 `AUDIENCE.md`） |
| `cw-save` | 把确认过的偏好/教训沉淀进正确的持久文件 |

> 空目录里喊 `cw-draft` 也能出稿，但那是一次性的；**先把写作之家和上下文喂到位，33 个技能才会互相"复利"**——这也是 "Compound"（复利）一词的来历。

---

## 🧪 本机实测备注

- **安装坑**：`npx skills add` 只搬 `skills/`，而 `cw-setup-project` 等用 `parents[3]/defaults/…`、`../../references/…` 引共享文件 → 必须把仓库根级 `references/`、`defaults/` 按同名相对位置补装到 `~/.agents/`，否则技能跑到一半才报缺文件。
- **端到端验证**：装后实跑 `create_project.py` 成功产出 `VOICE.md`/`STYLE.md`/`AUDIENCE.md`/`drafts/`/`examples/`，证明相对路径解析正确。
- 在 Claude 下 **`cw-panel`** 会调用配套的 `compound-writing:review:*` 子代理（对应仓库 `agents/`，本机摊平放入 `~/.claude/agents/`）；其他 runtime 走普通多视角流程。
- 在自带技能目录注入的仓库里（例如 `~/storage/github/every/compound-writing` 自身）运行 pi 会出现同名技能冲突告警，属预期。

## 🔗 参考

- 官方仓库: [github.com/EveryInc/compound-writing](https://github.com/EveryInc/compound-writing)
- 架构契约: 仓库 `ARCHITECTURE.md`、`references/context-contract.md`
- 工作区约定: `defaults/workspace-convention.md`
- 相关笔记: [技能库总索引（2026-09-08）](/notes/every-skills-sop)
