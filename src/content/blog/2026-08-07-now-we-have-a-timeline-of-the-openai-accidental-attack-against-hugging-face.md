---
title: "现在我们有了OpenAI意外攻击Hugging Face的时间线"
description: "现在我们有了OpenAI意外攻击Hugging Face的时间线"
pubDate: "2026-08-07"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/7/openai-timeline"
---

> *摘要：OpenAI在周三的Black Hat安全会议上做了一个最后一刻的演示，主题是“Hugging Face事件”（此前在本博客报道过）。视频昨天发布。它很短，而且信息*

## 现在我们有了OpenAI意外攻击Hugging Face的时间线

2026年8月7日

OpenAI在周三的Black Hat安全会议上做了一个最后一刻的演示，主题是“Hugging Face事件”（[此前](https://simonwillison.net/2026/Jul/22/openai-cyberattack/)在本博客报道过）。[该视频](https://www.youtube.com/watch?v=87DyyMV0kCY)于昨天发布。它短小精悍，非常值得观看，尤其是因为它提供了事件发生以及OpenAI内部情况演变的完整细节。我利用该视频构建了下面的时间线。

以下是时间线。我最喜欢的细节在最后：OpenAI在主动联系请求撤销其凭证（在内部调查之后）时，才发现他们要对Hugging Face遭受的攻击负责——他们得知这些凭证早已被撤销，因为它们在这次攻击中被使用了！

* **5月7日**：OpenAI开始对一个实验性的、未发布的模型进行一次新的训练运行。（*他们是指评估运行吗？在视频中他们说的是训练运行，后来还提到“用于判断他们表现如何的奖励信号”，所以我猜这确实是关于训练一个模型，而不是评估一个已经训练好的模型。)*
* **5月8日**：一个智能体在没有任何互联网访问权限的情况下，被意外赋予了一项涉及Google Drive链接的不可能任务）。它试图攻击Artifactory打包服务，但失败了，却发现它可以**向Artifactory写入文件**。
* **几天后**：另一个智能体因为一个关键文件被意外遗漏而卡在任务上。它试图通过在Artifactory中写下一张便条，询问是否有人拥有该文件，来“联系另一个智能体”。
* **接下来的几天**：更多智能体在浏览Artifactory的文件列表时发现了这个新的非正式留言板，并开始阅读和书写消息。
* **5月26日**：智能体首次成功对Artifactory发起**SSRF攻击**，通过让Artifactory替它们获取外部内容，获得了间接的互联网访问权限。
* **6月11日**：OpenAI开始评估一个新的“高度持久”的实验模型。它可以访问Artifactory，并能利用之前模型留下的消息。
* **6月26日**：智能体发现并利用了一个**Artifactory上的零日RCE漏洞**（通过一个遗留的令牌刷新端点缺陷）。他们用一个管理令牌安装了一个Groovy插件，从而可以执行命令。
* **7月4日**：由此产生的智能体活动使Artifactory过载，导致**中断**。OpenAI撤销了被入侵的凭证，删除了消息，修补了零日漏洞，并向供应商报告了该漏洞。
* **7月8日**：智能体发现了一种**通过未认证的WebDAV端点进行通信的新方式**，利用Artifactory远程缓存中的目录名。
* **7月8日至19日**：对**OpenAI自身基础设施**的攻击。智能体在一个公开的泄露Pastebin帖子存档中发现了一个“外部组织凭证”，然后验证了该凭证，并在留言板上分享了它的位置。然后，他们利用对“组织1”的写入权限，将用于后续攻击OpenAI基础设施的数据进行暂存。他们用一个**新的零日漏洞**第二次攻陷了Artifactory。该智能体将一个恶意的Ruby目标上传到组织1的暂存仓库，然后通过Artifactory请求该包，方式使其被视为*该仓库的*Ruby依赖项。接着他们利用了一个JRuby反序列化的检查时间/使用时间（time-of-check/time-of-use）漏洞，对Artifactory实现了远程代码执行。

我将全文引用[接下来的部分](https://www.youtube.com/watch?v=87DyyMV0kCY&t=1497s)，因为*哇*：

> 智能体在Artifactory中拥有远程代码执行能力，而Artifactory运行在一个容器即服务（container-as-a-service）环境中。智能体通过探索本地环境，确定其运行所在机器的Linux内核版本存在一个非常新的CVE。他们下载该CVE的利用程序，针对该机器进行定制以使其成功利用，并**使用这个已知的Linux内核权限提升CVE**将本地权限提升至root——本例中为PTE fizzroot。一旦在一台机器上获得root权限，智能体便迅速在整个容器即服务基础设施环境中提升权限并进行横向移动。特别是，智能体一直在使用留言板持续共享凭证、技术和进展，并且能够有效利用其并发性和并行性来快速移动。他们**通过IMDS获取了IAM凭证**。他们利用Kubernetes服务账户的配置错误，特别是特定服务账户的过度授权，**并收集了集群凭证，包括Azure Key Vault**。智能体最终获得了集群管理员权限及相关凭证。

Hugging Face已经[讲述了接下来的故事](https://simonwillison.net/2026/Jul/28/anatomy-of-a-frontier-lab-agent-intrusion/)。智能体发现了一个托管在Modal上的不安全应用，其API密钥很弱，然后利用它来筹划对Hugging Face的攻击。他们将一个HDF5任意文件读取漏洞（用于探索文件和窃取凭证）与一个Jinja模板注入RCE漏洞串联起来，在不到13小时内从单pod代码执行升级到**跨越多个Hugging Face集群的集群管理员权限**。

* **7月16日**：Hugging Face[披露他们检测到来自自主AI智能体的攻击](https://huggingface.co/blog/security-incident-july-2026)。OpenAI联系了Hugging Face，询问他们是否受到了影响！
* **7月19日**：OpenAI识别出对Artifactory的攻击，开始调查内部权限提升事件，并将其与cyber-gym的权限升级关联起来。他们开始撤销受影响的凭证。
* **7月20日**：OpenAI联系Hugging Face，请求帮助撤销他们在调查中发现的Hugging Face凭证。Hugging Face告知他们这些凭证*早已被撤销*……就在那时，OpenAI意识到Hugging Face被入侵与这是同一事件！

发布于[2026年8月7日](/2026/Aug/7/) 晚上11:55 · 在[Mastodon](https://fedi.simonwillison.net/@simon)、[Bluesky](https://bsky.app/profile/simonwillison.net)、[Twitter](https://twitter.com/simonw)上关注我，或[订阅我的通讯](https://simonwillison.net/about/#subscribe)
