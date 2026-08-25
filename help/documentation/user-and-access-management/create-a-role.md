---
title: 创建具有Brand Concierge权限的角色
description: 了解如何创建角色并授予其访问Brand Concierge所需的权限。
source-git-commit: 591bd1600e586a0a4ce484dbff3f9fb97e24d43d
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---


# 创建具有Brand Concierge权限的角色

在Adobe Experience Platform权限中创建角色以授予用户访问Brand Concierge的权限。

## 先决条件

* 您必须具有管理角色和权限所需的管理员权限。
* 必须首先将该用户添加到Adobe Experience Platform组织。 有关更多信息，请参阅“将用户添加到组织”（链接）。

## 创建角色

1. 登录到`experienceplatform.adobe.com`。

   >[!NOTE]
   >
   >在发布此过程之前，请通过工程确认生产URL。 源记录使用非正式或可能转录错误的URL。

2. 在左侧导航中，滚动到&#x200B;**权限**&#x200B;并选择。
3. 选择&#x200B;**角色**&#x200B;查看现有角色，然后选择&#x200B;**创建新角色**。
4. 输入角色的名称，如`Brand Concierge Access Users`，添加描述，并确认创建。
5. 打开新角色并分配权限：

   1. 搜索&#x200B;**Brand Concierge**&#x200B;的权限列表。
   2. 选择&#x200B;**管理Brand Concierge**。

   当前，**管理Brand Concierge**&#x200B;是唯一可用的Brand Concierge权限。 粒度权限层当前不可用。

6. 选择角色可以访问的沙盒或沙盒。

   一个组织可以包含多个沙盒，这些沙盒是独立的工作区。 仅选择适用于此角色的沙箱。

7. 选择&#x200B;**保存**。

## 后续步骤

创建角色后，将用户添加到该角色。 有关更多信息，请参阅“将用户添加到角色”（链接）。

## 相关注意事项

* 创建和管理沙盒的过程超出了此过程的范围。
* 在定义长期角色模型之前，确认是否计划了额外的粒度Brand Concierge权限。
