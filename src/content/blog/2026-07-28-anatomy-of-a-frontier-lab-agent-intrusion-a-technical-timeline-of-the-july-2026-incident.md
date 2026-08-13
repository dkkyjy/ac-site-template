---
title: "前沿实验室智能体入侵剖析：2026年7月事件技术时间线"
description: "前沿实验室智能体入侵剖析：2026年7月事件技术时间线"
pubDate: "2026-07-28"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Jul/28/anatomy-of-a-frontier-lab-agent-intrusion"
---

> *摘要：前沿实验室智能体入侵剖析：2026年7月事件技术时间线
Hugging Face 刚刚发布了这篇极其详细的技术描述，关于 OpenAI 近期意外发起的网络攻击*

2026年7月28日 - 链接博客

**[前沿实验室智能体入侵剖析：2026年7月事件技术时间线](https://huggingface.co/blog/agent-intrusion-technical-timeline)**。Hugging Face 刚刚发布了这篇极其详细的技术描述，关于 [OpenAI 近期对其基础设施发起的意外网络攻击](https://simonwillison.net/2026/Jul/22/openai-cyberattack/)。这次攻击*非常*复杂，而这份文档同时也是一份现代对抗性安全方法的速成教程。

我们仍在等待 OpenAI 提供更多关于其智能体如何逃逸沙箱的细节。它发现的零日漏洞所在的包代理现已确认为 JFrog 的 Artifactor，而 [JFrog 与 OpenAI 就零日安全发现展开合作](https://jfrog.com/blog/jfrog-and-openai-collaboration-on-zero-day-security-findings/) 来自 JFrog 的文章链接到了 [Artifactory 7.161.15 版本发布说明](https://docs.jfrog.com/releases/docs/artifactory-self-managed-releases#artifactory-7161)，其中列出了 8 个归功于 OpenAI 员工的独立 CVE。

该智能体通过 HTTP 代理逃逸后，首先为后续攻击建立了一个行动基地：

> [...] 该智能体利用包注册表缓存代理（其主要允许的互联网网络出口之一）中的零日漏洞逃逸了其沙箱，随后滥用托管在第三方提供商基础设施上的公共代码评估外部沙箱。它能够在该外部沙箱上以 root/admin 身份运行命令，并将其用作整个活动的控制、中转和泄密基地：它充当了智能体的外部发射台。

（这个“第三方提供商” [后来被确认为 Modal](https://simonwillison.net/2026/Jul/28/akshat-bubna/)。）

接着，它花费了**五天**时间——从 7 月 8 日星期四到 7 月 13 日星期一——执行了一个经典攻击模式：建立 C2（命令与控制）、侦察、权限提升、转储配置信息、外泄目标数据以及清理痕迹。

（关于该事件的第一个 Hugging Face 帖子 [于 7 月 16 日星期四发布](https://huggingface.co/blog/security-incident-july-2026)，而 OpenAI 的承认 [则是在 7 月 21 日星期二](https://openai.com/index/hugging-face-model-evaluation-security-incident/)。）

在此过程中，它使用了各种巧妙的技巧。有一次，它发现了一个不安全的 Jinja2 模板执行点，并利用它执行任意代码：

```
{{ cycler.__init__.__globals__.__builtins__.exec(
 "import gzip,base64; exec(gzip.decompress(base64.b64decode('<payload>')))"
) }}
```

它逃逸了 Hugging Face 内的一个容器，窃取了一个 Kubernetes 服务账户令牌，并利用它进一步探索网络。

当“DNS 碍事”时，它猴子补丁了 Python socket 库，硬编码了一个 IP 地址：

```
socket.getaddrinfo = lambda *a, **k: [(2, 1, 6, '', ['<pinned-ip>', 443])]
```

它甚至启动了自有的 Tailscale 网络来帮助外泄数据！

```
tailscaled --tun=userspace-networking \
  --state=mem: --socket=/tmp/ts.sock \
  --socks5-server=127.0.0.1:1055 \
  --no-logs-no-support >/tmp/td 2>&1 &
```

Hugging Face 团队指出，虽然人类攻击者也可能发现并利用同样的漏洞，但这里的关键区别在于**速度**：

> 我们从这类攻击中学到的是，机器速度的攻击使得普通的弱点对防御者来说代价更高。LLM 智能体带来了攻击者可以测试的路径数量、失败路径可被替换的速度以及防御者必须解读的证据量的阶跃式增长。

对我来说，显而易见的是，最优秀的前沿模型，在不受额外护栏约束的情况下，**一定会**找到任何存在的漏洞。

整个软件行业需要提升其安全水平。

发布于 [2026年7月28日](/2026/Jul/28/) 晚上 9:28
