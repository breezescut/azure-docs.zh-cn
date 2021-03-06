---
title: "Azure 政府版开发人员指南 | Microsoft 文档"
description: "本文比较了多项功能，并提供了 Azure 政府版应用程序开发指南。"
services: azure-government
cloud: gov
documentationcenter: 
author: smichelotti
manager: zakramer
ms.assetid: 6e04e9aa-1a73-442c-a46c-2e4ff87e58d5
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: stemi
translationtype: Human Translation
ms.sourcegitcommit: 0bab116e977a8490bf0c114c8b829aa1ce07e923
ms.openlocfilehash: 41af18426ce3f8124458a61c3af280c3dd17cfcd


---
# <a name="azure-government-developer-guide"></a>Azure 政府版开发人员指南
Azure 政府版环境是与 Microsoft 网络的其余部分隔离的物理实例。 此指南介绍应用程序开发人员和管理员在以交互方式使用不同的 Azure 区域时必须了解的差异。

## <a name="overview"></a>概述
Azure 政府版是 Microsoft Azure 服务的单独实例。 它满足美国联邦机构、州和地方政府及其解决方案提供商的安全和合规性需求。 Azure 政府版提供来自非美国政府部署的物理和网络隔离，并提供筛选过的美国人员。

Microsoft 提供多种工具，帮助开发人员创建云应用程序并将其部署到全局 Microsoft Azure 服务（简称“全局服务”）和 Microsoft Azure 政府版服务。

开发人员在创建应用程序并将其部署到 Azure 政府版服务而不是全局服务时，需要了解这两个服务的主要区别。 需要了解的具体区域包括：设置和配置其编程环境、配置终结点、编写应用程序以及将应用程序作为服务部署到 Azure 政府版。

本文档中的信息总结了两种服务之间的差异， 并补充了 [Azure 政府版](http://www.azure.com/gov "Azure 政府版")站点和 MSDN 上 [Microsoft Azure 技术库](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN")的可用信息。 官方信息也可在其他位置提供，如 [Microsoft Azure 信任中心](https://azure.microsoft.com/support/trust-center/ "Microsoft Azure 信任中心")、[Azure 文档中心](https://azure.microsoft.com/documentation/)和 [Azure 博客](https://azure.microsoft.com/blog/ "Azure 博客")。

此内容适用于部署到 Microsoft Azure Government 的合作伙伴和开发人员。

## <a name="guidance-for-developers"></a>开发人员指南
大多数当前提供的技术内容都假设应用程序是为全局服务而不是为 Azure 政府版开发的。 因此，必须了解你开发的在 Azure 政府版中托管的应用程序中的两大差异。

* 在全局服务的特定区域中的某些服务和功能可能在 Azure 政府版中不可用。
* Azure 政府版中的功能配置可能不同于全局服务中的功能配置。 因此，必须检查示例代码、配置和步骤，确保你是在 Azure 政府版云服务环境中构建并执行的。

目前，美国爱荷华州政府和美国弗吉尼亚州政府是支持 Azure 政府版的数据中心。 如需最新的数据中心和可用服务，请参阅[可用产品(按区域)](https://azure.microsoft.com/regions/services)。


## <a name="endpoint-mapping"></a>终结点映射
若要了解如何映射公共 Azure 和 SQL 数据库终结点到 Azure 政府版特定终结点，请参考下表：

| Name | Azure 政府版终结点 |
| --- | --- |
| ActiveDirectoryServiceEndpointResourceId  | https://management.core.usgovcloudapi.net/ |
| GalleryUrl | https://gallery.usgovcloudapi.net/ |
| ManagementPortalUrl | https://manage.windowsazure.us |
| ServiceManagementUrl | https://management.core.usgovcloudapi.net/ |
| PublishSettingsFileUrl | https://manage.windowsazure.us/publishsettings/index |
| ResourceManagerUrl | https://management.usgovcloudapi.net/ |
| SqlDatabaseDnsSuffix | .database.usgovcloudapi.net |
| core.usgovcloudapi.net | core.usgovcloudapi.net |
| ActiveDirectoryAuthority | https://login-us.microsoftonline.com/ |
| GraphUrl | https://graph.windows.net/ |
| GraphEndpointResourceId | https://graph.windows.net/ |
| TrafficManagerDnsSuffix | usgovtrafficmanager.net |
| AzureKeyVaultDnsSuffix | vault.usgovcloudapi.net |
| AzureKeyVaultServiceEndpointResourceId | https://vault.usgovcloudapi.net |



## <a name="next-steps"></a>后续步骤
有关 Azure 政府版的详细信息，请参阅以下资源：

* [注册试用版](https://azuregov.microsoft.com/trial/azuregovtrial)
* [获取和访问 Azure 政府版](http://azure.com/gov)
* [Azure 政府版概述](/azure-government-overview)
* [Azure 政府版博客](http://blogs.msdn.microsoft.com/azuregov/)
* [Azure 合规性](https://www.microsoft.com/trustcenter/compliance/complianceofferings)



<!--HONumber=Feb17_HO3-->


