---
title: 将对话路由到代表
description: 选择Brand Concierge如何使用循环配置资源、直接代理或自定义路由规则来确定哪个销售代表接收Live Chat或会议预订。
role: User, Admin
level: Beginner
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
source-git-commit: 91cbec8d6f4e4800e3bdc8c8f60bfe9ed168a963
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 0%
---
# 将对话路由给代表 {#routing}

路由回答了一个问题：当此访客请求某人时，*他们得到了哪个*&#x200B;人？

您单独为&#x200B;**[!UICONTROL Meeting Booking]**&#x200B;集成和&#x200B;**[!UICONTROL Live Chat]**&#x200B;集成选择路由类型，以便门房可以平均分配会议，同时将每一次聊天发送给一位专家或任何其他组合。

## 路由类型 {#routing-types}

| 路由类型 | 谁获得对话 | 需要[!DNL Marketo Engage] |
| --- | --- | --- |
| **[!UICONTROL 轮循资源]** | 分发给所有符合条件的代表 | 否 |
| **[!UICONTROL 直接代理]** | 您指定的一名代表 | 否 |
| **[!UICONTROL 自定义路由]** | 分配给访客符合条件的最高优先级路由规则的代表 | 是 |

默认情况下，循环调度可用，无需配置。 您可以在集成本身上设置路由类型。 查看[激活Live Chat和会议预订](configure-in-composer.md)。

### 轮循方案 {#round-robin}

循环配置资源在会议中的行为与聊天中的行为不同，因为二者的目标不同。

对于会议预订，循环调度会平均分配会议。 首先提供已连接日历的第一个符合条件的代表。 一旦与他们预约了会议，他们就会被带出游泳池，并安排下一位代表。 有五位代表的前五次会议有五位不同的人参加。 每个人都收到一个后，池将重置，然后循环将重新开始。

泳池每24小时重设一次，这样一来，每天的会议都能平均分配。

>[!NOTE]
>
>24小时轮循重置在UTC而不是代表的本地时区运行。

对于实时聊天，没有顺序轮换。 每个符合条件的代表都会同时收到传入聊天通知，第一个接受者会接受对话。

### 直接代理 {#direct-agent}

该门房的每一次会议或聊天都转到您选择的单一代表。 将此项用于由一名客户经理拥有的促销活动登陆页面，或用于拥有一名专家的区域。

### 自定义路由 {#custom-routing}

自定义路由根据访客的[!DNL Marketo]潜在客户和公司属性评估您在[!DNL Adobe Marketo Qualifier]中构建的规则。 当合适的代表依赖于您了解的访客相关内容时，可使用它，例如：

* 地理位置：来自一组国家/地区的访客将前往区域团队。
* 产品兴趣：对特定产品感兴趣的访客请转到该产品的专家。
* 业务单位、公司规模或行业：潜在客户和公司属性的任意组合。

>[!IMPORTANT]
>
>自定义路由是需要[!DNL Adobe Marketo Engage]的Live Chat和Meeting预订的一部分，因为规则是基于[!DNL Marketo]潜在客户和公司属性构建的。 循环调度程序和直接代理程序无需它即可工作。

## 创建自定义路由规则 {#create-rule}

1. 在[!DNL Adobe Marketo Qualifier]中的&#x200B;**[!UICONTROL 管理]**&#x200B;下，选择&#x200B;**[!UICONTROL 路由规则]**。

   该列表显示了贵组织中的每条规则及其优先级、名称、创建者和创建日期。

   ![路由规则页面列出了具有优先级、名称、创建者和创建者的现有规则](../assets/sales-handoff-routing-1.png){width="800" zoomable="yes"}

1. 选择&#x200B;**[!UICONTROL 创建规则]**&#x200B;并为该规则提供一个名称。

1. 构建标准。

   左侧面板列出了&#x200B;**[!UICONTROL 公司]**&#x200B;和&#x200B;**[!UICONTROL 潜在客户]** （人员）对象中可用的[!DNL Marketo]属性。 搜索属性，然后将其拖动到画布上以添加条件。

1. 为每个条件设置运算符和值。

   运算符包括&#x200B;**[!UICONTROL 等于]**、**[!UICONTROL 不等于]**&#x200B;和&#x200B;**[!UICONTROL 包含]**&#x200B;等。 例如，`Country` **[!UICONTROL 等于]** `United States`。

1. 将条件组合到组中。

   将多个条件添加到一个组，并使用&#x200B;**[!UICONTROL AND]**&#x200B;或&#x200B;**[!UICONTROL OR]**&#x200B;将其加入。 添加更多组并使用&#x200B;**[!UICONTROL AND]**&#x200B;或&#x200B;**[!UICONTROL OR]**&#x200B;加入组。 通过这种方式在组内嵌套条件，可表达复杂的条件，如&#x200B;*（国家/地区为美国或加拿大）和（产品兴趣包含Analytics）*。

1. 将规则分配给代表。

   在分配部分下，按名称搜索代表并从列表中选择代表。 根据需要添加任意数量的代表。 任何人都可以接收与规则匹配的对话。

1. 选择&#x200B;**[!UICONTROL 保存]**。

## 优先级应用方式 {#priority}

每个路由规则都有优先级。 仅当访客符合多个规则的条件时，优先级才重要。

发生这种情况时，仅应用最高优先级的匹配规则，并且对话被路由到分配给该规则的代表。 访客也符合条件但排名较低的规则会被忽略。

>[!NOTE]
>
>数字越低，优先级越高。 优先级为1的规则优先于优先级为2的规则。 创建规则的人员设置其优先级。

在获胜规则内，分配给它的代表是符合条件的池，上述会议的循环调度行为和聊天的“通知所有人”行为都适用于该池。

## 下一步 {#next}

在代表已设置且已确定路由的情况下，将权能添加到礼宾处。 查看[激活Live Chat和会议预订](configure-in-composer.md)。
