---
title: "Azure 服务总线诊断日志 | Microsoft 文档"
description: "了解如何为 Azure 中的服务总线设置诊断日志。"
keywords: 
documentationcenter: 
services: service-bus-messaging
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/17/2017
ms.author: babanisa
translationtype: Human Translation
ms.sourcegitcommit: abcb0eee979853948cf6d981ff8f3a457eeeeef0
ms.openlocfilehash: 65fe81dc90f2dc7a251860adfdd8374912cb8d73
ms.lasthandoff: 03/01/2017


---
# <a name="service-bus-diagnostic-logs"></a>服务总线诊断日志

可以查看两种类型的 Azure 服务总线日志：
* **[活动日志](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**。 此类日志提供对作业执行的操作的相关信息。 此类日志始终开启。
* **[诊断日志](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**。 可以配置诊断日志，以便更深入地了解作业发生的所有情况。 诊断日志涵盖从创建作业开始到删除作业为止的所有活动，其中包括作业运行时发生的更新和活动。

## <a name="turn-on-diagnostic-logs"></a>启用诊断日志
诊断日志默认已**禁用**。 若要启用诊断日志，请执行下列步骤：

1.    在 Azure 门户中，转到流式处理作业边栏选项卡。

2.    在“监视”下面，转到“诊断日志”边栏选项卡。

    ![在边栏选项卡中导航到诊断日志](./media/service-bus-diagnostic-logs/image1.png)  

3.    选择“启用诊断”。

    ![启用诊断日志](./media/service-bus-diagnostic-logs/image2.png)

4.    对于“状态”，选择“打开”。

    ![更改诊断日志的状态](./media/service-bus-diagnostic-logs/image3.png)

5.    设置所需的存档目标，例如存储帐户、事件中心或 Azure Log Analytics。

6.    选择想要收集的日志类别，例如“执行”或“创作”。

7.    保存新的诊断设置。

新设置在大约 10 分钟后生效。 在此之后，日志将出现在“诊断日志”边栏选项卡上配置的存档目标中。

有关配置诊断的详细信息，请参阅 [Azure 诊断日志概述](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)。

## <a name="diagnostic-logs-schema"></a>诊断日志架构

所有日志均以 JavaScript 对象表示法 (JSON) 格式存储。 每个日志项目均包含字符串字段，这些字段采用以下示例中所述的格式。

## <a name="operation-logs-example"></a>操作日志示例

**OperationalLogs** 类别中的日志捕获在服务总线操作期间发生的情况。 具体而言，这些日志捕获操作类型，包括队列创建、所用的资源和操作状态。

操作日志 JSON 字符串包括下表列出的元素：

Name | 说明
------- | -------
ActivityId | 用于跟踪的内部 ID
EventName | 操作名称             
resourceId | Azure Resource Manager 资源 ID
SubscriptionId | 订阅 ID
EventTimeString | 操作时间
EventProperties | 操作属性
状态 | 操作状态
调用方 | 操作的调用方（Azure 门户或管理客户端）
category | OperationalLogs

下面是操作日志 JSON 字符串的示例：

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create Queue",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>后续步骤
* [服务总线简介](service-bus-messaging-overview.md)
* [服务总线入门](service-bus-create-namespace-portal.md)
