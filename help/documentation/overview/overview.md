---
title: Brand Concierge概述
description: 了解Brand Concierge是什么、其主要组件如何组合在一起，以及在整个编辑器界面中遇到的主要术语词汇表。
source-git-commit: 60835c7971d86341194d773f9cf487c4cb6f171a
workflow-type: tm+mt
source-wordcount: '509'
ht-degree: 1%

---

# Brand Concierge概述

Brand Concierge是一个代理平台，它允许企业和品牌在其面向客户的表面（网站、移动应用程序和其他数字资产）上启动个性化的对话体验。 每个对话都基于品牌自己的内容和护栏，而集成功能让这些对话中的见解流入品牌生态系统的其他部分，如Marketo Engage。

## 主要组件

Brand Concierge部署包含两个主要部分：

| 片段 | 内容 |
|---|---|
| **访客体验** | 面向品牌的界面，例如网站或移动应用程序，访客可在其中与门房互动并实时获取响应。 |
| **作者** | 用于设计礼宾体验和管理礼宾、集成、配置、评估、部署和分析的从业者界面。 |

## 编辑器模块

在Composer中，主要模块包括：

- [用户和访问管理](../user-and-access-management/add-a-user-to-the-org.md)
- [知识源的创建和管理](../knowledge-sources/knowledge-sources.md)，在门房之间共享
- [门房管理](../concierge-management/concierge-management.md)：集成、技能、门房指示、音调和语音、视觉风格和聊天组件
- [评估](../evaluation/evaluation.md)
- [部署](../deployment/deployment.md)
- [上线清单](../go-live-checklist/go-live-checklist.md)
- [Analytics](../analytics/analytics.md)

## 各部件如何连接

知识源（内容）由集成（连接）查询，该集成（连接）由技能（行为）调用，所有技能都包装在访客与之交互的礼宾（整体体验）中。

## 术语表

这些术语显示在作者的整个界面中。

| 术语 | 定义 |
|---|---|
| **门房** | AI聊天体验本身：每个品牌、网站或用例一个。 一个帐户可以有多个帐户。 |
| **作者** | 用于构建和管理门房的界面，与网站访客看到的不同。 |
| **知识源** | 门房在回答问题（如网站页面或产品列表）时可以使用的内容。 没有礼宾员，礼宾员就无话可说。 |
| **集成** | 与可检索信息（如网站内容或实时产品目录）的系统的连接。 |
| **技能** | 门房可以执行的特定功能，例如回答一般性问题、比较产品或预订会议。 技能使用一个或多个集成来执行其功能。 |
| **护栏** | 规定门房不应做或讨论的内容（如竞争对手或法律建议）的规则。 |
| **评估** | 由样本问题和预期答案组成的结构化测试，用于评估门房表现。 |
| **数据流ID** | 技术标识符，指定在Adobe系统中将访客活动数据发送的位置。 它由IT或分析团队提供。 |
| **沙盒** | 组织内的独立工作区。 一个组织可以有多个门卫，每个门卫可以有多个门卫。 |
| **IMS组织** | Adobe对组织整体帐户的术语。 |
| **MCP**（例如，Commerce MCP） | 特定系统（例如实时产品目录）的Adobe管理的连接器，使用IT或商业团队提供的代码或密钥进行配置。 |
| **CJA (Customer Journey Analytics)** | Adobe的分析产品。 Brand Concierge会自动在此处设置入门仪表板，而无需其他设置。 |

>[!NOTE]
>
>营销人员通常可以完全跳过[用户和访问管理](../user-and-access-management/add-a-user-to-the-org.md)（IT中的某人只完成一次操作），从[知识源](../knowledge-sources/knowledge-sources.md)开始。 只有在设置新队友时，才能返回到用户和访问管理。
