---
title: "smolmachines / smolvm 作为不可信 Python 与 JavaScript 的沙箱"
description: "smolmachines / smolvm 作为不可信 Python 与 JavaScript 的沙箱"
pubDate: "2026-08-19"
heroImage: "/post_img.png"
tags: ["simon-willison"]
originalLink: "https://simonwillison.net/2026/Aug/19/smolmachines-untrusted-sandbox"
---

> *摘要：研究：smolmachines / smolvm 作为不可信 Python 与 JavaScript 的沙箱
        我让运行在 Claude Code for web 中的 Claude Fable 5 执行以下研究任务：

Put https://smolmachine*

2026年8月19日

[研究](/elsewhere/research/)
[smolmachines / smolvm 作为不可信 Python 与 JavaScript 的沙箱](https://github.com/simonw/research/tree/main/smolmachines-untrusted-sandbox#readme)
— 测试 smolvm 1.8.3 表明，它非常适合使用硬件隔离的虚拟机而非共享内核容器来对不可信的 Python 和 JavaScript 数据转换进行沙箱隔离。离线本地镜像、无网络执行、CPU/内存限制、来宾端强制超时、存储配额、只读输入挂载、可写输出挂载以及 `--unprivileged` 均按预期正常工作，冷启动时间约为 0.6–1.5 秒，热执行时间约为 50 毫秒。

我让运行在 Claude Code for web 中的 Claude Fable 5 执行以下研究任务：

> `将 https://smolmachines.com 进行全面的性能与安全测试，探索如何利用它来运行不可信的 Python 和 JavaScript 代码，同时限制其可占用的内存和 CPU 时间（防止"while true"死循环），不开放网络访问，文件系统仅能访问指定文件`
>
> `目标是能够利用它来执行用户提供的任务，例如数据转换`

它很快就遇到了一个问题：Claude Code for web 环境无法运行 [smol machines](https://smolmachines.com)。引用它[所写的笔记](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/smolmachines-untrusted-sandbox/notes.md#environment-check)：

> * 此 Claude Code 容器：Linux 6.18.5-fc-v20（本身是一个 Firecracker 来宾虚拟机），4 个 vCPU，15GB 内存。**没有 /dev/kvm，没有 vmx/svm CPU 标志** → 不支持嵌套虚拟化。
> * `smolvm machine run` 如预期般失败："kvm not available"。
> * 备选方案 B：GitHub Actions 的 ubuntu 运行器确实暴露了 /dev/kvm → 通过在此分支上创建一个临时工作流来运行完整的测试套件，收集日志，在最终提交中移除该工作流。

而备选方案 B 正是它[所采取的做法](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/.github/workflows/smolvm-sandbox-test.yml)，在 GitHub Actions 运行器中直接安装 smolvm 并针对该分支运行[这些测试](https://github.com/simonw/research/blob/5e6861e54441472d194de96b49b901fd99ebc153/smolmachines-untrusted-sandbox/run-tests.sh)。

这是针对 Claude Code for web 环境限制的一个富有创意的解决方案。又一个 Fable [不知疲倦地主动行动](https://simonwillison.net/2026/Jun/11/fable-is-relentlessly-proactive/) 的例子。

发布于 [2026年8月19日](/2026/Aug/19/) 晚上 11:16
