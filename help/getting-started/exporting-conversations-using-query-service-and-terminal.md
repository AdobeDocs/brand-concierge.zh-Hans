---
title: 使用查询服务和终端导出对话
description: 了解如何使用Brand Concierge查询服务和终端，以及针对凭据和个人数据的指南，将Experience Platform对话导出为CSV。
topic: Development
role: Developer
level: Experienced
doc-type: Tutorial
duration: 260
last-substantial-update: 2026-10-15
jira: KT-22874
source-git-commit: 04c844e16a8fb13e54dfcbb29c776896674532b9
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%
---
# 使用查询服务和终端导出对话

了解如何使用Adobe Experience Platform查询服务和终端保存和导出Adobe Brand Concierge对话。 此视频介绍如何查找数据集表名称、配置PostgreSQL命令行客户端以及将日期范围的对话数据导出到CSV文件。

## 此视频面向谁？

* 需要使用命令行工具导出Brand Concierge对话数据的开发人员

## 视频内容

* 在Experience Platform中查找对话数据集并复制其表名
* 使用Homebrew安装PostgreSQL客户端并配置其命令行路径
* 从查询服务凭据选项卡复制PSQL连接命令
* 选择数据集和日期范围，然后将查询结果导出到本地CSV文件
* 检查导出的结果，并在需要时刷新过期的凭据

>[!VIDEO](https://video.tv.adobe.com/v/3504643)

```bash
psql "sslmode=require host=... password=..." --csv -o ~/brand-concierge-conversation-export/oct-1-2026-till-today-responses.csv -c "SELECT * FROM cja_brand_concierge_responses_dataset_b4c15126_bd9b_4d52_8151_26 WHERE timestamp >= TIMESTAMP '2026-10-01 00:00:00' AND timestamp <= NOW() ORDER BY timestamp ASC LIMIT 1000"
```

>[!IMPORTANT]
>
> PSQL connection命令包含实时密码。 请勿将其共享或粘贴到聊天、人工智能代理或电子邮件中。 如果验证在密码过期后失败，请从Query Service Credentials选项卡复制新的命令。

>[!NOTE]
>
> 导出的对话可能包含个人身份信息(PII)。 存储CSV文件时，请按照您组织的数据处理策略操作。

有关文档，请参阅[Brand Concierge帮助](../documentation/overview.md)。

## 相关教程

* [使用Adobe CX Enterprise Coworker导出对话](exporting-conversations-using-adobe-cx-enterprise-coworker.md)
