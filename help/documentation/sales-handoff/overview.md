---
title: Live Chat和Meeting预订概述
description: 了解Brand Concierge如何通过实时聊天或代表日程表上的预约会议，将访客交给人类。
role: User, Admin
level: Beginner
TQID: https://experienceleague.adobe.com/vfX6M2-ts7d2-AxKGjaP1Ygkk3FGSGRl4CHslcKYSXU
product_v2:
  - id: b6ee73fe-bdc6-47d9-99a2-80194514dd40
    internal-label: Brand Concierge
role_v2:
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
    internal-label: User
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
    internal-label: Admin
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
    internal-label: Beginner
source-git-commit: 4751ce408fb2aa6d1affae4dd7a5d81ca43ffc8a
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%
---
# Live Chat和Meeting预订概述 {#overview}

门房可以自己回答大多数问题。 有些对话需要人员：定价谈判、采购调查表、演示请求。 Live Chat和Meeting Booking是Brand Concierge将对话传递给人的两种方式，不会丢失访客已提供的上下文。

* **实时聊天**&#x200B;将访客立即连接到同一聊天窗口中的可用代表。
* **会议预订**&#x200B;从代表的日历中为访客提供真正的开放版块，并预订会议。

两者由相同的技能提供，即&#x200B;**[!UICONTROL 移交给销售人员]**，并且两者都通过[!DNL Adobe Marketo Qualifier] （以前为[!DNL Sales Qualifier]）传递，您的代表可以在其中连接其日历、设置其可用性并聊天。

## 移交的工作原理 {#how-it-works}

在访客与他人联系之前，必须排列四个元素。

| 片段 | 作用 | 在何处设置 |
| --- | --- | --- |
| 代表配置文件 | 连接日历并声明代表何时可用于会议和聊天 | [!DNL Adobe Marketo Qualifier] > **[!UICONTROL 配置文件设置]** |
| 路由 | 决定&#x200B;*哪个*&#x200B;代表获得此访客 | [!DNL Adobe Marketo Qualifier] > **[!UICONTROL 路由规则]**，以及每个集成上的路由类型 |
| 集成 | **[!UICONTROL Meeting Booking]**&#x200B;和&#x200B;**[!UICONTROL Live Chat]**&#x200B;使门房能够检查可用性、开始聊天和预订时段 | 编辑器> **[!UICONTROL 浏览集成]** |
| 技能 | 根据访客的意图，**[!UICONTROL 移交给销售人员]**&#x200B;决定&#x200B;*何时*&#x200B;提供移交服务 | 作者> **[!UICONTROL 浏览技能]** |

运行时：访客说了一些与某个技能的&#x200B;**[!UICONTROL 使用条件匹配的东西，该技能触发，它会调用附加到它的任何集成，路由解析代表，并为访客提供聊天、会议或两者。]**

>[!NOTE]
>
>技能和集成是门房的一般组成部分。 有关基础模型，请参阅[技能和集成框架](../skills-and-integrations.md)。

## 访客体验 {#visitor-experience}

当技能触发时，门房会解释这些选项，并将它们作为对话中的按钮呈现。

![门房回应提供移交服务，立即与专家聊天并在其下方安排会议按钮](../assets/sales-handoff-visitor-1.png){width="800" zoomable="yes"}

### 实时聊天 {#visitor-live-chat}

如果访客选择聊天，则当路由查找可用的代表时，礼宾将显示连接状态。

![在访客选择“立即与专家聊天”后，门房显示正在连接到专家状态](../assets/sales-handoff-visitor-2.png){width="800" zoomable="yes"}

在代表加入之前，访客会完成一个简短的聊天前表单。 这些答案随对话一起传递，并在聊天收件箱中显示为代表的&#x200B;**[!UICONTROL 表单详细信息]**，因此访客不必重复任何内容。

访客可随时结束聊天并需要确认。

![结束聊天确认对话框，要求访客确认结束与专家的聊天](../assets/sales-handoff-visitor-4.png){width="800" zoomable="yes"}

### 会议预订 {#visitor-meeting-booking}

如果访客选择预约会议，则礼宾人员会按照访客自己的时区，显示一个日历，其中包含被路由代表的日历上真正打开的插槽。

![在对话中预订会议卡，该会议卡显示月份日历和可用时隙列表](../assets/sales-handoff-visitor-3.png){width="800" zoomable="yes"}

在访客确认时段后，将发送该代表的确认电子邮件，其中可能包括[!DNL Microsoft Teams]链接。 对话上下文将发送给收到邀请的代表。

## 开始之前需要什么 {#prerequisites}

* 访问[!DNL Adobe Marketo Qualifier]，您的代表已添加为[!DNL Adobe Admin Console]中的用户。
* 至少有一个已连接日历的代表。 在激活&#x200B;**[!UICONTROL Meeting Booking]**&#x200B;集成之前，无法激活该集成。
* 至少有一位已设置实时聊天可用性的代表。 **[!UICONTROL 实时聊天]**&#x200B;集成必须等到激活后才可激活。
* 可向其添加集成和技能的Composer中的礼宾。

>[!NOTE]
>
>Live Chat和会议预订不需要[!DNL Adobe Marketo Engage]。 如果没有它，代表、会议和聊天数据将保留在[!DNL Adobe Marketo Qualifier]中。 有两项操作确实需要[!DNL Marketo Engage]：基于[!DNL Marketo]潜在客户和公司属性构建的自定义路由规则，以及写回[!DNL Marketo]人员记录的礼宾活动类型。

## 设置Live Chat和会议预订 {#setup-path}

按顺序处理这些内容。

1. [在Adobe Marketo Qualifier中设置代表](representative-setup.md) — 连接日历、定义确认电子邮件并设置日历和实时聊天可用性。
1. [将对话路由到代表](routing-rules.md) — 选择循环配置资源、直接代理或基于[!DNL Marketo]属性构建的自定义规则。
1. [激活Live Chat和Meeting预订](configure-in-composer.md) — 连接两个集成并激活&#x200B;**[!UICONTROL 移交给销售人员]**&#x200B;技能。
1. [聊天收件箱](chat-inbox.md) — 代表在聊天到达后执行的操作，包括对对话进行评分。
1. Marketo Engage中的[Brand Concierge数据](marketo-activities.md) — 回写到[!DNL Marketo]的潜在客户字段和活动类型，以及如何在智能营销活动中使用它们。
1. [监视会议和实时聊天](monitor.md) — 预订列表和绩效仪表板。

## 数据保留 {#retention}

实时聊天对话将保留30天。
