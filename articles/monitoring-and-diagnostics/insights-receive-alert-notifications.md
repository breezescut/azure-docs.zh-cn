---
title: "接收 Azure 服务的警报通知 | Microsoft Docs"
description: "在满足警报规则条件时收到通知。"
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: bc2fa75e-bc26-474e-af43-de30de5e604f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2015
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 5c929849eb71d5ebace7ef274700626d4c5e5f3e


---
# <a name="receive-alert-notifications"></a>接收警报通知
可以根据监视指标或事件接收 Azure 服务的警报。

对于指标值的警报规则，当指定指标的值超过分配的阈值时，警报规则会变为活动状态并发送一条通知。 对于事件的警报规则，规则可以针对每个事件发送通知，或仅当发生一定数量的事件时发送通知。

在创建警报规则时，你可以选择向服务管理员和协同管理员或向你可以指定的另一位系统管理员发送电子邮件通知的选项。 当规则变为活动状态以及当警报条件满足时发送通知电子邮件。

可使用 [REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx) 或 [.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/) 来配置警报规则并以编程方式获取相关信息。

## <a name="create-an-alert-rule"></a>创建警报规则
1. 在[门户](https://portal.azure.com/)中，单击“浏览”，然后单击想要监视的资源。
2. 在“操作”可重用功能区中，单击“警报规则”磁贴。
3. 单击“添加通知”命令。
   
    ![添加警报](./media/insights-receive-alert-notifications/Insights_AddAlert.png)
4. 可以命名警报规则，并选择显示在通知电子邮件中的说明。
5. 选择“指标”时，将为该指标选择一个条件和阈值。 这是 Azure 用于监视并显示警报活动的时间段。
   
    ![条件和阈值](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)
6. 还可选择“事件”，并在某个事件发生时获得通知。
   
    ![事件](./media/insights-receive-alert-notifications/Insights_Events.png)
7. 最后，可以选择向责任管理员发送电子邮件通知。

单击“保存”后，只要选择的指标超出阈值，都可在几分钟内获知。

## <a name="managing-your-alert-rules"></a>管理你的警报规则
创建警报规则后，可查看与前一天指标相比的警报阈值预览。

![事件](./media/insights-receive-alert-notifications/Insights_EditAlert.png)

当然可以编辑此警报规则，如果要暂时停止接收有关通知，可以“禁用”或“启用”它。

## <a name="next-steps"></a>后续步骤
* [为“警报”配置 webhook](insights-webhooks-alerts.md)，可将通知路由到各个通道
* [监视服务指标](insights-how-to-customize-monitoring.md)以确保你的服务可用且响应迅速。
* [启用监视和诊断](insights-how-to-use-diagnostics.md)以收集有关服务的详细高频率指标。
* 使用 Application Insights [监视任何网页的可用性和响应能力](../application-insights/app-insights-monitor-web-app-availability.md)，以便可以在页面出现故障时及时发现。
* 要确切了解代码在云中的执行情况时[监视应用程序性能](../application-insights/app-insights-azure-web-apps.md)。
* [查看事件和活动日志](insights-debugging-with-events.md)，可了解服务中发生的所有事件。
* [跟踪服务运行状况](insights-service-health.md)，以在 Azure 遇到性能下降或服务中断时及时发现。




<!--HONumber=Nov16_HO3-->


