---
title: "认识我们 Claude Opus 4.8 Build Day 黑客松的获奖者 | Claude by Anthropic"
description: "---"
pubDate: "2026-06-17"
heroImage: "/post_img.png"
tags: ["claude-blog"]
originalLink: "https://claude.com/blog/meet-the-winners-of-our-claude-opus-4-8-build-day-hackathon"
---

# 认识我们 Claude Opus 4.8 Build Day 黑客松的获奖者 | Claude by Anthropic

---

6月13日，我们邀请了300多位创始人和开发者来到旧金山，参加一场为期12小时、基于Claude Opus 4.8的黑客松。超过1,500人报名申请；最终310人参与其中，许多人从世界各地赶来，每人获得500美元积分，用一天时间将一个想法变成可运行的演示。

我们与三支获胜团队聊了聊他们构建了什么，以及他们如何利用Claude完成这些工作。

祝贺所有获奖者和每一位参与者。我们希望他们的项目能给你们带来一些自己的灵感。

## 第一名：[Tekton](https://tekton-build.vercel.app/)，Holly Tang 和 Austin Burgess

*Holly Tang 和 Austin Burgess 构建了 Tekton，一个3D重建平台，让唐代建筑重现生机，每一个组件都可追溯到其历史来源。*

当一座历史木结构建筑被烧毁时，数百年的工艺可能随之消失。Tekton以3D形式重建这些建筑，并将每一个部件追溯到有据可查的来源。

给Tekton一座历史建筑，Claude会进行研究，汇集图纸、施工文件、照片和图表，然后通过339个增量施工状态组装出一个3D模型。当你点击模型中的任何组件时，Tekton会显示该细节的来源以及放置原因。团队称之为“证据链”，从源材料延伸到经过验证的模型。他们构建这个平台是为了学术验证、修复工作和文化保护，从唐代建筑和巴黎圣母院的塔尖开始。

验证过程完全在Opus 4.8上运行。独立的验证子代理在隔离的上下文窗口中评估每次重建，自我修正循环反复检查组件放置，直到全部20项测试通过。每次构建都对照历史记录及其引用进行衡量，因此最终模型遵循了原始建筑方式的有据可查的规则。

Holly Tang 和 Austin Burgess 一个月前在一次 Code with Claude 活动中排队买咖啡时相识。Holly是一名设计师，一直在帮助Austin的初创公司[Pearl](https://joinpearl.co/)。Holly说：“我喜欢看纪录片，看到美丽的建筑毁于火灾总是让我很难过。”她曾独自制作了一个单一重建的原型；Austin的贡献是将其扩展到适用于任何建筑的端到端方案。

为了构建Tekton，两人分阶段工作：他们首先让巴黎圣母院的塔尖按比例渲染，然后添加更精细的细节，再扩展到结构的其余部分。时间在完整大教堂完成之前就用完了。即便如此，几位黑客松参与者询问了该项目或主动提出帮助提高其准确性。Holly和Austin希望将Tekton开源，以便博物馆、历史学家、非营利组织和政府能够在此基础上继续开发。

**给其他开发者的建议：** 在构建任何部分之前，先规划整个项目。

Austin说：“我们构建了完整的PRD和一个包含约50个工单的Notion看板，每个工单对应一个具体任务。这几乎就像——这是完整的端到端项目，这是我们对每一步的确切要求。”计划确定后，他将构建过程拆分为独立的工作流并并行运行。

[Tekton 在 GitHub 上](https://github.com/tangxiya-star/Tekton)

## 第二名：[Sim Francisco](https://simfrancisco.org/)，Tanmayi Priya Dasari 和 Tejas Prabhune

*Tanmayi Priya Dasari 和 Tejas Prabhune 构建了 Sim Francisco，一个基于人口普查数据的旧金山人口数字孪生系统，可以在数秒内对一座合成城市进行民意调查，并预测现实世界的结果。*

Sim Francisco是旧金山人口的工作模型。它拥有10,000名来自美国人口普查数据的合成居民，每个人都有自己的人口统计特征、个人经历和世界观，被放置在城市地图上，并实时对新闻做出反应。

向这座城市提问，它会逐街区对全部合成选民进行民意调查。在知识截止日期为2023年10月的模型上运行，它预测2024年总统大选民主党得票率为81.3%，而实际为83.8%；预测旧金山2024年3月Prop A的支持率为70%，而实际为70.38%。它跟踪Kalshi和Polymarket等预测市场，误差在几个百分点以内。*

Opus 4.8编写了整个前端和后端，并端到端验证了后端行为。为了验证模型的工作，团队让Claude与一个验证器和一个对抗性代理协同工作，构建了一个能够再现该市真实人口分布特征的后端。

Tanmayi Priya Dasari 和 Tejas Prabhune 是加州大学伯克利分校的电气工程和计算机科学专业学生，通过校园内的机器学习俱乐部相识。对Tejas来说，Sim Francisco同时也是对他正在构建的post-training公司的测试，他在那里研究模拟人格是否能保持足够的一致性，以用于训练长周期任务的模型。

**给其他开发者的建议：** 不要满足于第一个可行的方案，尤其是当它成本高昂时。

团队的第一个版本为10,000名居民中的每一位都单独进行了一次推理调用，这变得很昂贵。“随着时间的推移，Claude运行了一个它自己创建的进化聚类算法，”Tejas说，将居民分批归入约300个代表性人格。分组版本在Kalshi、Polymarket和历史结果上保持了相同的准确性，同时将推理成本降低了10到100倍。

[*Sim Francisco 在 GitHub 上*](https://github.com/tejasprabhune/simfrancisco)

## **第三名：** [**Custom Universe**](https://www.luminal.com/realtime-edit-demo)**，Jake Stevens 和 Mauricio Pereira**

*Jake Stevens 和 Mauricio Pereira 构建了 Custom Universe，一个实时引擎，可将一张手机照片转化为完全可编辑、照片级逼真的3D场景。*

用手机拍一张椅子的照片，Custom Universe就会将其转化为一个3D物体，你可以将其放入场景中，用文本提示重新设计风格，并在渲染图像实时更新的同时移动它。

该项目面向机器人实验室，这些实验室需要大量合成数据来训练机器人完成特定任务和场景。实验室可以扫描工厂车间里的一台机器，将其放入场景中，并生成数据来针对该特定环境微调机器人模型。构建这样的设置通常需要聘请物理学家和工程师来处理物理和碰撞几何。Custom Universe让你通过拖动物体来布置场景，团队计划添加精确定位功能，例如将物体在厨房台面上移动30厘米。

Opus 4.8端到端地构建了该项目，并在整个黑客松期间操作了运行模型的远程NVIDIA H100。团队还使用Claude来确定哪些模型能产生正确的输出，并构建了将使用Apple的RealityKit捕获的手机扫描物体导入Web应用的管道。

Jake Stevens 和 Mauricio Pereira 在活动中相识。Jake是罗切斯特理工学院（RIT）计算机视觉专业毕业生，经营着一家专注于加速AI模型的初创公司[Luminal](https://www.luminal.com/)；场景构建器最初是他一直想尝试的一个副项目。Mauricio是麻省理工学院机器人学毕业生，经营着[Coat Robotics](https://www.coatrobotics.com/)，他带来了自己亲身经历的问题：机器人领域仍然缺乏训练数据，而构建合成环境很困难。Custom Universe依赖开源模型和算法，可免费使用；团队表示用户可以在自己的GPU上运行它。

**给其他开发者的建议：** 使用Claude来选择工具，而不仅仅是编写代码。

“很多迭代过程都是查看哪个模型能给我们正确的输出，所以我们使用Claude做了大量研究，”Mauricio说。团队还将不熟悉的技术交给Claude进行集成。“例如，Apple RealityKit，以及我们如何确保人们能够将扫描的物体输入到我们的网站。我们问Claude：把这个添加到管道中。”

[*Custom Universe 在 GitHub 上*](https://github.com/jss8649/image-edit-realtime-hackathon)

[*了解*](http://claude.com/community)*我们的Claude社区项目，包括聚会、黑客松等。*

*\*Sim Francisco是一个独立的黑客松项目，以预测选举结果作为示例。这并不代表Anthropic认可将AI模拟选举预测作为一种用例。*
