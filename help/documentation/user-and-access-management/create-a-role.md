---
title: 创建具有Brand Concierge权限的角色
description: 了解如何创建角色并授予其访问Brand Concierge所需的权限。
source-git-commit: 60835c7971d86341194d773f9cf487c4cb6f171a
workflow-type: tm+mt
source-wordcount: '212'
ht-degree: 1%

---


# 创建具有Brand Concierge权限的角色

在Adobe Experience Platform权限中创建角色以授予用户访问Brand Concierge的权限。

>[!PREREQUISITES]
>
>- 您必须具有管理角色和权限所需的管理员权限。
>- 必须首先将该用户添加到Adobe Experience Platform组织。 有关详细信息，请参阅[将用户添加到组织](./add-a-user-to-the-org.md)。

## 创建角色

1. 登录到`experienceplatform.adobe.com`。

1. 在左侧导航中，滚动到&#x200B;**权限**&#x200B;并选择。
1. 转到&#x200B;**角色**&#x200B;查看现有角色，然后选择&#x200B;**创建新角色**。
1. 输入角色的名称，如`Brand Concierge Access Users`，添加描述，并确认创建。
1. 打开新角色并分配权限：

   1. 搜索&#x200B;**Brand Concierge**&#x200B;的权限列表。
   1. 选择&#x200B;**管理Brand Concierge**。

   目前，**管理Brand Concierge**&#x200B;是唯一可用的Brand Concierge权限；粒度权限层尚不可用。

1. 选择角色可以访问的沙盒或沙盒。

   一个组织可以包含多个沙盒，这些沙盒是独立的工作区。 仅选择适用于此角色的沙箱。

1. 选择&#x200B;**保存**。

## 后续步骤

创建角色后，将用户添加到该角色。 有关详细信息，请参阅[将用户添加到Brand Concierge角色](./add-a-user-to-the-role.md)。
