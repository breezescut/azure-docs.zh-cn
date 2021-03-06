---
title: "监视应用程序网关的访问、性能日志、后端运行状况及度量值 | Microsoft Docs"
description: "了解如何启用和管理应用程序网关的访问和性能日志"
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: 2c4b3e23c478a006b081929269ae066d00af20cd
ms.lasthandoff: 03/22/2017


---
# <a name="backend-health-diagnostics-logging-and-metrics-for-application-gateway"></a>应用程序网关的后端运行状况、诊断日志记录和度量值

Azure 提供使用日志记录和度量值来监视资源的功能。 应用程序网关为后端运行状况、日志记录和度量值提供以下功能。

[**后端运行状况**](#backend-health) - 应用程序网关提供通过门户和 powershell 监视后端池中的服务器运行状况的功能。 还可在性能诊断日志中找到后端池的运行状况。

[**日志记录**](#enable-logging-with-powershell) - 通过日志记录，可出于监视目的从资源保存或使用性能、访问及其他日志。

[**度量值**](#metrics) - 应用程序网关当前有一个度量值。 此度量值可衡量应用程序网关的吞吐量，以每秒字节数为单位。

## <a name="backend-health"></a>后端运行状况

应用程序网关提供通过门户、PowerShell 和 CLI 监视后端池的各个成员的运行状况的功能。 还可通过性能诊断日志找到后端池的聚合运行状况摘要。 后端运行状况报告反映对后端实例的应用程序网关运行状况探测的输出。 如果探测成功且后端可接收流量，则视为正常，否则视为不正常。

> [!important]
> 如果应用程序网关子网上存在 NSG，则应在应用程序网关子网上为后端池成员打开端口范围 65503-65534。 这些端口是确保后端运行正常所必需的。

### <a name="view-backend-health-through-the-portal"></a>通过门户查看后端运行状况

查看后端运行状况无需任何操作。 在现有的应用程序网关中，导航到“监视” > “后端运行状况”。 此页面将列出后端池中的每个成员（无论其位于 NIC、IP 或 FQDN）。 将显示后端池名称、端口、后端 http 设置名称和运行状况状态。 运行状况状态的有效值为“正常”、“不正常”和“未知”。

> [!WARNING]
> 如果后端运行状况状态为“未知”，请确保网络安全组 (NSG) 规则或 VNet 中的自定义 DNS 未阻止访问后端。

![后端运行状况][10]

### <a name="view-backend-health-with-powershell"></a>使用 PowerShell 查看后端运行状况

还可通过 PowerShell 检索后端运行状况。 下面的 PowerShell 代码演示如何使用 `Get-AzureRmApplicationGatewayBackendHealth` cmdlet 拉取后端运行状况。

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

将会返回结果，响应的示例在以下代码段中显示。

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>诊断日志记录

可在 Azure 中使用不同类型的日志来对应用程序网关进行管理和故障排除。 通过门户可以访问其中部分日志，所有日志都可从 Azure Blob 存储中提取并在不同工具（例如 [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)、Excel 和 PowerBI）中查看。 可从以下列表了解有关不同类型日志的详细信息：

* **活动日志：**可以使用 [Azure 活动日志](../monitoring-and-diagnostics/insights-debugging-with-events.md)（以前称为操作日志和审核日志）查看提交到 Azure 订阅的所有操作及其状态。 默认情况下会收集活动日志条目，可在 Azure 门户中查看这些条目。
* **访问日志：**可以使用此日志来查看应用程序网关访问模式并分析重要信息，包括调用方的 IP、请求的 URL、响应延迟、返回问代码、输入和输出字节数。 每隔 300 秒会收集一次访问日志。 此日志包含每个应用程序网关实例的一条记录。 应用程序网关实例可以由“instanceId”属性标识。
* **性能日志：**可以使用此日志来查看应用程序网关实例的执行情况。 此日志会捕获每个实例的性能信息，包括提供的请求总数、吞吐量（以字节为单位）、失败的请求计数、正常和不正常的后端实例计数。 每隔 60 秒会收集一次性能日志。
* **防火墙日志：**可以使用此日志来查看通过应用程序网关的检测或阻止模式（通过 Web 应用程序防火墙配置）记录的请求。

> [!WARNING]
> 日志仅适用于在资源管理器部署模型中部署的资源。 不能将日志用于经典部署模型中的资源。 要更好地了解两种模型，可参考[了解资源管理器部署和典型部署](../azure-resource-manager/resource-manager-deployment-model.md)一文。

对于日志，若要存储日志，可使用三个不同的选项。

* 存储帐户 - 如果日志存储时间较长并且需要进行查看，最好使用存储帐户。
* 事件中心 - 若要集成其他 SEIM 工具以获取对资源的警报，最好使用事件中心
* Log analytics - Log analytics 最适合用于常规实时监视应用程序或查看趋势。

### <a name="enable-logging-with-powershell"></a>使用 PowerShell 启用日志记录

每个 Resource Manager 资源都会自动启用活动日志记录。 必须启用访问和性能日志记录才能开始收集通过这些日志提供的数据。 若要启用日志记录，请参阅以下步骤：

1. 记下存储帐户的资源 ID，其中存储日志数据。 此值的形式如下：/subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Storage/storageAccounts/\<存储帐户名称\>。 订阅中的所有存储帐户均可使用。 可以使用预览门户来查找此信息。

    ![预览门户 - 应用程序网关诊断](./media/application-gateway-diagnostics/diagnostics1.png)

2. 记下应用程序网关的资源 ID（会为其启用日志记录）。 此值的形式如下：/subscriptions/\<subscriptionId\>/resourceGroups/\<resource group name\>/providers/Microsoft.Network/applicationGateways/\<应用程序网关名称\>。 可以使用预览门户来查找此信息。

    ![预览门户 - 应用程序网关诊断](./media/application-gateway-diagnostics/diagnostics2.png)

3. 使用以下 PowerShell cmdlet 启用诊断日志记录：

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>活动日志不需要单独的存储帐户。 使用存储来记录访问和性能需支付服务费用。

### <a name="enable-logging-with-azure-portal"></a>使用 Azure 门户启用日志记录

#### <a name="step-1"></a>步骤 1

在 Azure 门户中导航到资源。 单击“诊断日志”。 如果是首次配置诊断，边栏选项卡与下图类似：

对于应用程序网关，提供 3 种日志。

* 访问日志
* 性能日志
* 防火墙日志

若要开始收集数据，请单击“启用诊断”。

![诊断设置边栏选项卡][1]

#### <a name="step-2"></a>步骤 2

“诊断设置”边栏选项卡用于设置诊断日志的设置。 在此示例中，Log Analytics 用于存储日志。 单击“Log Analytics”下的“配置”来配置工作区。 事件中心和存储帐户也可用于保存诊断日志。

![诊断边栏选项卡][2]

#### <a name="step-3"></a>步骤 3

选择现有的 OMS 工作区或者创建新的 OMS 工作区。 对于此示例，使用现有的 OMS 工作区。

![OMS 工作区][3]

#### <a name="step-4"></a>步骤 4

完成后，确认设置，然后单击“保存”保存设置。

![确认所选内容][4]

### <a name="activity-log"></a>活动日志

默认情况下由 Azure 生成此日志（以前称为“操作日志”）。  日志在 Azure 的事件日志存储区中保留 90 天。 通过阅读[查看事件和活动日志](../monitoring-and-diagnostics/insights-debugging-with-events.md)一文可了解有关这些日志的详细信息。

### <a name="access-log"></a>访问日志

只有按照上述步骤基于每个应用程序网关启用了此日志，才会生成此日志。 数据存储在你启用日志记录时指定的存储帐户中。 应用程序网关的每次访问均以 JSON 格式记录下来，如以下示例所示：

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayAccess",
    "time": "2016-04-11T04:24:37Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIP":"37.186.113.170",
        "clientPort":"12345",
        "httpMethod":"HEAD",
        "requestUri":"/xyz/portal",
        "requestQuery":"",
        "userAgent":"-",
        "httpStatus":"200",
        "httpVersion":"HTTP/1.0",
        "receivedBytes":"27",
        "sentBytes":"202",
        "timeTaken":"359",
        "sslEnabled":"off"
    }
}
```

### <a name="performance-log"></a>性能日志

只有按照上述步骤基于每个应用程序网关启用了此日志，才会生成此日志。 数据存储在你启用日志记录时指定的存储帐户中。 将记录以下数据：

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> 延迟时间的计算是从接收到 HTTP 请求的第一个字节开始，到发出 HTTP 响应的最后一个字节为止。 它是应用程序网关处理时间加上后端的网络成本，再加上后端处理请求所花费的时间之和。

### <a name="firewall-log"></a>防火墙日志

只有按照上述步骤基于每个应用程序网关启用了此日志，才会生成此日志。 此日志还需要在应用程序网关上配置 Web 应用程序防火墙。 数据存储在你启用日志记录时指定的存储帐户中。 将记录以下数据：

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
    "operationName": "ApplicationGatewayFirewall",
    "time": "2016-09-20T00:40:04.9138513Z",
    "category": "ApplicationGatewayFirewallLog",
    "properties":     {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIp":"108.41.16.164",
        "clientPort":1815,
        "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
        "ruleId":"OWASP_973336",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "action":"Logged",
        "site":"Global",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
}
```

### <a name="view-and-analyze-the-activity-log"></a>查看和分析活动日志

可以使用任何以下方法查看和分析活动日志数据：

* **Azure 工具：**通过 Azure PowerShell、Azure 命令行界面 (CLI)、Azure REST API 或 Azure 预览门户检索活动日志中的信息。  [使用 Resource Manager 活动操作](../azure-resource-manager/resource-group-audit.md)一文中详细介绍了每种方法的分步说明。
* **Power BI：**如果还没有 [Power BI](https://powerbi.microsoft.com/pricing) 帐户，你可以免费试用。 使用[适用于 Power BI 的 Azure 活动日志内容包](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/)，你可以借助预配置的仪表板（可直接使用或进行自定义）分析你的数据。

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>查看并分析访问、性能和防火墙日志

Azure [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) 可以从 Blob 存储帐户收集计数器和事件日志，其中包括用于分析日志的直观显示和功能强大的搜索功能。

还可以连接到存储帐户并检索访问和性能日志的 JSON 日志条目。 下载 JSON 文件后，你可以将它们转换为 CSV 并在 Excel、PowerBI 或任何其他数据可视化工具中查看。

> [!TIP]
> 如果你熟悉 Visual Studio 和更改 C# 中的常量和变量值的基本概念，则可以使用 Github 提供的[日志转换器工具](https://github.com/Azure-Samples/networking-dotnet-log-converter)。
> 
> 

## <a name="metrics"></a>度量值

度量值是某些 Azure 资源的一项功能，可供在门户中查看性能计数器。 撰写本文时，应用程序网关有一个可用的度量值。 此度量值是吞吐量，可在门户中看到。 导航到应用程序网关，然后单击“度量值”。 若要查看这些值，请在“可用度量值”部分选择吞吐量。 在下图中，可以看到一个示例，其中有可用于显示不同时间范围的数据的筛选器。

若要查看当前支持的度量值的列表，请参阅 [Azure Monitor 支持的度量值](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![度量值视图][5]

### <a name="alert-rules"></a>警报规则

可以根据资源的度量值启动警报规则。 这对于应用程序网关的意义是，如果应用程序网关的吞吐量在指定时间段内高于、低于或等于阈值，警报即可调用 webhook 或给管理员发送电子邮件。

以下示例指导创建警报规则，以在达到吞吐量阈值时给管理员发送电子邮件。

#### <a name="step-1"></a>步骤 1

单击“添加度量值警报”开始。 还可通过度量值边栏选项卡访问此边栏选项卡。

![警报规则边栏选项卡][6]

#### <a name="step-2"></a>步骤 2

在“添加规则”边栏选项卡中，填写名称、条件和通知部分，完成时单击“确定”。

**条件**选择器允许 4 个值：**大于**、**大于或等于**、**小于**或**小于或等于**。

**期间**选择器允许挑选从 5 分钟到 6 小时的期间。

通过选择“给所有者、参与者和读者发送电子邮件”，可根据有权访问该资源的用户动态发送电子邮件。 否则，可以在“其他管理员电子邮件”文本框中提供用户的逗号分隔列表。

![添加规则边栏选项卡][7]

如果达到阈值，送达的电子邮件内容类似下图所示：

![达到阈值时的电子邮件][8]

创建度量值警报后即会显示警报列表，并提供所有警报规则的概述。

![警报规则视图][9]

若要了解有关警报通知的详细信息，请参阅[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

若要了解有关 webhook 以及如何与其搭配使用警报的详细信息，请参阅[针对 Azure 度量值警报配置 webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>后续步骤

* 使用 [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) 直观显示计数器和事件日志
* [使用 Power BI 直观显示你的 Azure 活动日志](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx)博客文章。
* [在 Power BI 和其他组件中查看和分析 Azure 活动日志](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)博客文章。

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png

