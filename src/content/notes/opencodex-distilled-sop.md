---
title: "OpenCodex - 蒸馏笔记"
category: "OMC / OMX 工作流"
description: "Universal provider proxy 蒸馏：任意 LLM 网关路由方法论。"
pubDate: "2026-08-10"
badge: "distilled"
tags: ["opencodex", "proxy", "gateway"]
---

# opencodex Distilled — provider proxy 蒸馏存档

## 定位
[lidge-jun/opencodex](https://github.com/lidge-jun/opencodex) v2.11.0 (npm `@bitkyc08/opencodex`，★8.4k) — "make codex open"。**Universal provider proxy**：在本地起一个代理（`ocx start`，localhost:10100），把任意 LLM（Claude/Gemini/Grok/DeepSeek/Kimi/OpenRouter/Ollama…）统一映射到 Codex CLI/App、Claude Code、Claude Desktop、Grok Build 的标准接口，让这些工具换"任意大脑"但换流程不改。与 OMX/OMC（agent 编排层）不同，这是**模型网关/路由 infra** 类。

## 核心三元组（Morphling）
- **目标**：GA 不依赖单一官方 provider，能用/能切换任意 LLM 网关；并借鉴其"证据路由/多 key 容错"让 GA 的模型调度更稳。
- **测例（Tests）**：官方 `tests/`（601 项，其覆盖 routing/failover/adapters 的单元与集成）；`docs/adr` 记录架构决策。GA 对照 = 把 opencodex 接为可选 proxy 跑一次真实任意模型请求，或移植其"证据路由"逻辑单测对照。未实跑 SWE-bench（不适用；这是网关非基准题）。
- **行为**：混合型 —— 核心路由方法论**重写/提炼**入 GA；网关本体**调用**（装为 proxy）；体积化 gui/docs-site/devlog = **舍弃**。

## 核心组件决策表
| 组件 | 决策 | 理由 |
|---|---|---|
| provider registry（providers/registry.ts，119KB） | 重写（提炼 schema） | 静态元数据：auth kind(forward/oauth/key/local)、inbound wire(responses/chat/anthropic)、model discovery、provider‹-›transport 映射；GA 可做轻量 provider 目录 |
| 证据路由（routing/evaluator+profile） | 重写（核心吸收） | 按 requirement/health/quota/cost 证据加权排序，**UNKNOWN 惩罚分 0.3**，显式排除理由，RouteDecisionTrace 可溯源。GA 调度/取舍可用此心智模型 |
| key-failover（providers/key-failover） | 重写（方法论吸收） | 429/限流触发 cooldown 换 key、解析 Retry-After、池轮换、cooldown 过期清除 |
| 多协议 adapters（openai-responses/chat、anthropic、google、kiro 等） | 调用 | 把各家 wire 智能 → 目标标准接口；体量大，GA 直接借 opencodex 或需要时照 adapter 契约自身映射 |
| App 无感集成（proxy 替换 Codex/Claude/Grok） | 舍弃/调用 | 机制是本地 proxy，GA 不需要；作为使用方法吸纳 |
| gui/docs-site/devlog/assets | 舍弃 | 非编排核心，体积大 |
| CLI `ocx` | 调用型接入 | `ocx start` 即可把任意 provider 挂到标准端点 |

## 蒸馏方法论（GA 可取）
1. **[Execution 真] Provider 注册表**：用一张静态声明表存 provider（auth/endpoint/wire/模型定标），而非散落 if-else。
2. **[+Enhancement] 证据路由策略**：决策 = 需求匹配(requirement) + 健康(health) + 配额(quota) + 成本(cost) + 分数(score) 加权；每个维度**未知就惩罚**（-0.3），已知错误则显式排除；结果留 trace。用"可解释打分"替"盲试错"。
3. **[Guarantee] 多 key 容错**：429/限流按 cooldown 自动轮换 key 池，Retry-After 纳入退避，冷却过期回收。
4. **协议适配单向化**：中心 agent 只认标准 wire，各厂商差异收敛到 adapter；image/truncation/tool-schema 归一。

## 对 GA 的应用方式
- **调用型（推荐最快）**：把 opencodex 安装为本机 proxy，GA 的 LLM 配置指向 `http://localhost:10100`，即可在 GA 内用任意已配 provider；避免 GA 自身扩散网络各厂商 SDK 依赖。
- **重构型**：在 GA 模型调用层引入"支撑 smd upstream"轻量路由（证据打分 + key 池），参照 registry/evaluator/failover 三元组；不搬 119KB 全量注册表。

## 结论
目标定位为"基础网关"。GA 偏移复用最大收益点：**①provider 注册表（声明式）②证据路由（UNKNOWN 惩罚 + 可溯源）③多 key 429 容错**。这些在 source `/memory/opencodex/opencodex-source/src/{providers,routing}` 可读（先例同 OMX/OMC，轻量归档，跳过 gui/docs-site/devlog/tests）。