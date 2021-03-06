---
title: "使用 Azure 网络观察程序“下一跃点”功能查找下一跃点 - Azure 门户 | Microsoft 文档"
description: "本文将介绍如何使用 Azure 门户通过“下一跃点”功能查找下一跃点类型和 ip 地址"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 866d7f77391fb7d23f7a8e651cb4d2fc8f56fba8
ms.openlocfilehash: 5fb85551d3c5fb8d1c63a965d89bae788000afe8
ms.lasthandoff: 02/23/2017


---

# <a name="find-out-what-the-next-hop-type-is-using-the-next-hop-capability-in-azure-network-watcher-using-the-portal"></a>使用门户通过 Azure 网络观察程序中的“下一跃点”功能找到下一跃点类型

> [!div class="op_single_selector"]
> - [Azure 门户](network-watcher-check-next-hop-portal.md)
> - [PowerShell](network-watcher-check-next-hop-powershell.md)
> - [CLI](network-watcher-check-next-hop-cli.md)
> - [Azure REST API](network-watcher-check-next-hop-rest.md)

“下一跃点”是网络观察程序的一项功能，提供基于指定的虚拟机获取下一跃点类型和 IP 地址的功能。 此功能对于确定离开虚拟机的流量是否通过网关、Internet 或虚拟网络到达其目标很有用。

## <a name="before-you-begin"></a>开始之前

此方案假定你已按照[创建网络观察程序](network-watcher-create.md)中的步骤创建网络观察程序。 此方案还假定要使用的是存在有效虚拟机的资源组。

[!INCLUDE [network-watcher-preview](../../includes/network-watcher-public-preview-notice.md)]

## <a name="scenario"></a>方案

本文中介绍的方案使用“下一跃点”找出资源的下一跃点类型和 IP 地址。 若要了解有关下一跃点的详细信息，请访问[下一跃点概述](network-watcher-next-hop-overview.md)。

在此方案中，你将：

* 从虚拟机中检索下一跃点。

## <a name="get-next-hop"></a>获取下一跃点

### <a name="step-1"></a>步骤 1

在 Azure 门户中导航到网络观察程序资源。

### <a name="step-2"></a>步骤 2

在导航窗格中单击“下一跃点”，选择虚拟机和网络接口，填写源和目标 IP，然后单击“下一跃点”按钮。

> [!NOTE]
> “下一跃点”功能要求 VM 资源已分配，从而可以运行。

![获取下一跃点概述][1]

### <a name="step-3"></a>步骤 3

任务完成后，将提供结果。 将显示下一跃点所属的 IP 地址和设备类型。 下表显示门户中可用的返回值。

**下一跃点类型**

* Internet
* VirtualAppliance
* VirtualNetworkGateway
* VnetLocal
* HyperNetGateway
* VnetPeering
* 无

如果使用了自定义路由来路由此流量，则也将在结果中显示用户定义的路由 (UDR)。

![获取下一跃点结果][2]

## <a name="next-steps"></a>后续步骤

通过访问[使用网络观察程序进行 NSG 审核](network-watcher-nsg-auditing-powershell.md)，了解如何以编程方式查看网络安全组设置

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png















