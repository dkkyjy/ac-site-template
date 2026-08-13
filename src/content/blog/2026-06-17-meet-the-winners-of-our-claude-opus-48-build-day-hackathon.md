---
title: "欢迎了解我们的 Claude Opus 4.8 黑客马拉松获奖者"
description: "6月13日，我们邀请300多位创始人和开发者来到旧金山，参加为期12小时、使用Claude Opus 4.8的黑客马拉松。超过1500人报名；310人参与其中，许多人从世界各地赶来，每人获得500美元信用额度，用一天时间将创意转化为可运行的演示。"
pubDate: "2026-06-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/meet-the-winners-of-our-claude-opus-4-8-build-day-hackathon"
---
            6月13日，我们邀请300多位创始人和开发者来到旧金山，参加为期12小时、使用Claude Opus 4.8的黑客马拉松。超过1500人报名；310人参与其中，许多人从世界各地赶来，每人获得500美元信用额度，用一天时间将创意转化为可运行的演示。

我们采访了三支获胜团队，了解他们构建了什么以及如何利用Claude实现目标。

祝贺所有获奖者和参与者。希望他们的项目能给您带来一些启发。

## 第一名：[Tekton](https://tekton-build.vercel.app/)，Holly Tang 和 Austin Burgess

*Holly Tang 和 Austin Burgess 构建了 Tekton，这是一个3D重建平台，让唐代建筑重现生机，每个组件都可追溯至其历史来源。*

当一座历史木制建筑被烧毁时，数百年的工艺可能随之消失。Tekton 以3D形式重建这些建筑，并将每个构件追溯至有文献记载的来源。

向 Tekton 提供一座历史建筑，Claude 就会进行研究，收集图纸、施工文件、照片和示意图，然后通过339个增量施工状态组装出3D模型。当您点击模型中的任何组件时，Tekton 会显示该细节的来源以及放置原因。团队称之为证据链，从源材料到经过验证的模型。他们为学术验证、修复工作和文化保护而构建，从唐代建筑和巴黎圣母院的尖顶开始。

验证完全在 Opus 4.8 上运行。独立的验证子代理在隔离的上下文窗口中对每次重建进行评分，自我修正循环重新检查组件放置，直到所有20项测试通过。每次构建都根据历史记录及其引用进行衡量，因此完成的模型遵循建筑原始建造的记载规则。

Holly Tang 和 Austin Burgess 一个月前在"与 Claude 一起编码"活动的咖啡排队时相识。设计师 Holly 一直在帮助 Austin 的初创公司 [Pearl](https://joinpearl.co/)。"我喜欢看纪录片，看到美丽的建筑被大火烧毁总是让我感到难过，"Holly 说。她曾自己制作了一个单一重建的原型；Austin 的贡献是将其扩展为可端到端适用于任何建筑。

为了构建 Tekton，两人分阶段工作：首先让巴黎圣母院尖顶按比例渲染，然后添加更精细的细节，再扩展到结构的其余部分。时间在整座大教堂完成之前就用完了。即便如此，几位黑客马拉松参与者询问了该项目或主动提出帮助使其更准确。Holly 和 Austin 希望将 Tekton 开源，以便博物馆、历史学家、非营利组织和政府能够在此基础上继续开发。

**给其他开发者的建议：** 在构建任何部分之前，先规划整个项目。

"我们构建了整个 PRD 和一个包含大约50个任务的 Notion 看板，每个任务对应一个具体工作，"Austin 说。"这几乎就像，这里是完整的端到端项目，每个步骤我们想要什么都很明确。"计划确定后，他将构建过程分解为独立的工作流并并行运行。

[Tekton 在 GitHub 上](https://github.com/tangxiya-star/Tekton)

## 第二名：[Sim Francisco](https://simfrancisco.org/)，Tanmayi Priya Dasari 和 Tejas Prabhune

*Tanmayi Priya Dasari 和 Tejas Prabhune 构建了 Sim Francisco，这是一个基于人口普查数据的旧金山人口数字孪生系统，可在数秒内对合成城市进行民意调查并预测现实世界的结果。*

Sim Francisco 是旧金山人口的工作模型。它拥有10,000名基于美国人口普查数据生成的合成居民，每个人都有自己的 demographics、个人历史和世界观，分布在城市地图上，并实时对新闻做出反应。

向这座城市提问，它会逐个街区对整个合成选民群体进行民意调查。运行在知识截止日期为2023年10月的模型上，它预测2024年总统选举民主党得票率为81.3%，实际结果为83.8%；预测旧金山2024年3月Prop A支持率为70%，实际结果为70.38%。它对 Kalshi 和 Polymarket 等预测市场的跟踪误差在几个百分点以内。*

Opus 4.8 编写了整个前端和后端，并端到端验证了后端的行为。为了验证模型的工作，团队让 Claude 与一个验证器和一个对抗性代理协同工作，构建了一个能够再现城市真实人口分布的后端。

Tanmayi Priya Dasari 和 Tejas Prabhune 是加州大学伯克利分校的电气工程和计算机科学专业学生，通过校园机器学习俱乐部相识。对 Tejas 来说，Sim Francisco 同时也是对他正在构建的 post-training 公司的测试，他正在研究模拟角色是否能够保持足够的一致性，以便在长期任务上训练模型。

**给其他开发者的建议：** 不要满足于第一个有效的方法，尤其是在成本高昂的情况下。

团队的第一个版本为每个10,000名居民单独进行推理调用，成本很高。"随着时间的推移，Claude 运行了一个它自己创建的进化聚类算法，"Tejas 说，将居民分批归约为大约300个代表性角色。分组版本在保持对 Kalshi、Polymarket 和历史结果的相同准确性的同时，将推理成本降低了10到100倍。

[*Sim Francisco 在 GitHub 上*](https://github.com/tejasprabhune/simfrancisco)

## **第三名：** [**Custom Universe**](https://www.luminal.com/realtime-edit-demo) **，Jake Stevens 和 Mauricio Pereira**

*Jake Stevens 和 Mauricio Pereira 构建了 Custom Universe，这是一个实时引擎，可将单张手机照片转换为完全可编辑、逼真的3D场景。*

用手机拍摄一张椅子的照片，Custom Universe 会将其转换为3D物体，您可以将其放入场景中，通过文本提示重新设计样式，并在渲染图像实时更新的同时移动它。

该项目面向机器人实验室，这些实验室需要大量合成数据来训练机器人执行特定任务和适应特定环境。实验室可以扫描工厂车间中的机器，将其放入场景中，并生成数据以微调该特定环境的机器人模型。构建这种设置通常需要聘请物理学家和工程师来处理物理和碰撞几何。Custom Universe 允许您通过拖动物体来布置场景，团队计划添加精确定位功能，例如将物体在厨房台面上移动30厘米。

Opus 4.8 端到端构建了该项目，并在整个黑客马拉松期间操作运行模型的远程 NVIDIA H100。团队还使用 Claude 来确定哪些模型能产生正确的输出，并构建了将使用 Apple RealityKit 捕获的手机扫描物体引入 Web 应用的管道。

Jake Stevens 和 Mauricio Pereira 在活动中相识。Jake 是罗切斯特理工学院（RIT）计算机视觉专业毕业生，经营着专注于加速 AI 模型的初创公司 [Luminal](https://www.luminal.com/)；场景构建器是他一直想尝试的副项目。Mauricio 是麻省理工学院机器人学毕业生，经营着 [Coat Robotics](https://www.coatrobotics.com/)，他带来了自己亲身经历的问题：机器人领域仍然缺乏训练数据，而构建合成环境很困难。Custom Universe 依赖开源模型和算法，可免费使用；团队表示用户可以在自己的 GPU 上运行。

**给其他开发者的建议：** 使用 Claude 来选择工具，而不仅仅是编写代码。

"很多迭代工作在于查看哪个模型能给我们正确的输出，所以我们使用 Claude 做了大量研究，"Mauricio 说。团队还让 Claude 集成不熟悉的技术。"例如，Apple RealityKit，以及我们如何确保用户能够将扫描的物体上传到我们的网站。我们问 Claude：把这个加入管道。"

[*Custom Universe 在 GitHub 上*](https://github.com/jss8649/image-edit-realtime-hackathon)

[*了解*](http://claude.com/community)*我们的 Claude 社区项目，包括聚会、黑客马拉松等。*

*\*Sim Francisco 是一个独立的黑客马拉松项目，以预测选举结果为例。这并不代表 Anthropic 认可使用 AI 模拟选举预测作为用例。*

‍
