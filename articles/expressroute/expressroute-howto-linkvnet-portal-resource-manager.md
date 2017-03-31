---
title: "将虚拟网络链接到 ExpressRoute 线路：Azure 门户 | Microsoft Docs"
description: "本文档概述了如何将虚拟网络 (VNet) 链接到 ExpressRoute 线路。"
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f5cb5441-2fba-46d9-99a5-d1d586e7bda4
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/08/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: b3c0cca9a6d5171b1248b0f463cbbb26641fc5f2
ms.openlocfilehash: a1a689dbfc35107b52f9b84f74ac8bfac0727a0e
ms.lasthandoff: 02/10/2017


---
# <a name="connect-a-virtual-network-to-an-expressroute-circuit"></a>将虚拟网络连接到 ExpressRoute 线路
> [!div class="op_single_selector"]
> * [Resource Manager - Azure 门户](expressroute-howto-linkvnet-portal-resource-manager.md)
> * [Resource Manager - PowerShell](expressroute-howto-linkvnet-arm.md)
> * [经典 - PowerShell](expressroute-howto-linkvnet-classic.md)
> * [视频 - Azure 门户](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-connection-between-your-vpn-gateway-and-expressroute-circuit)
> 
>  

本文将帮助你使用 Resource Manager 部署模型和 Azure 门户将虚拟网络 (VNet) 链接到 Azure ExpressRoute 线路。 虚拟网络可以在同一个订阅中，也可以属于另一个订阅。

**关于 Azure 部署模型**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="configuration-prerequisites"></a>配置先决条件
* 在开始配置之前，请务必查看[先决条件](expressroute-prerequisites.md)、[路由要求](expressroute-routing.md)和[工作流](expressroute-workflows.md)。
* 你必须有一个活动的 ExpressRoute 线路。
  
  * 请按说明[创建 ExpressRoute 线路](expressroute-howto-circuit-arm.md)，并通过连接提供商启用该线路。
  * 请确保为线路配置 Azure 专用对等互连。 有关路由说明，请参阅[配置路由](expressroute-howto-routing-portal-resource-manager.md)一文。
  * 确保配置 Azure 专用对等互连并运行你的网络和 Microsoft 之间的 BGP 对等互连，以便启用端到端连接。
  * 确保已创建并完全预配一个虚拟网络和一个虚拟网络网关。 请按说明创建 [VPN 网关](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)（仅遵循步骤 1-5）。

* 最多可以将 10 个虚拟网络链接到一条标准 ExpressRoute 线路。 使用标准 ExpressRoute 线路时，所有虚拟网络必须都位于同一地缘政治区域。 

* 如果已启用 ExpressRoute 高级外接程序，则可以链接 ExpressRoute 线路的地缘政治区域外部的虚拟网络，或者将更多虚拟网络连接到 ExpressRoute 线路。 有关高级外接程序的更多详细信息，请参阅[常见问题](expressroute-faqs.md)。

## <a name="connect-a-virtual-network-in-the-same-subscription-to-a-circuit"></a>将同一订阅中的虚拟网络连接到线路

### <a name="to-create-a-connection"></a>创建连接

> [!NOTE]
> 如果第 3 层提供商配置了你的对等互连，则 BGP 配置信息将不会显示。 如果你的线路处于已预配状态，你应该能够创建连接。
>

1. 确保已成功配置 ExpressRoute 线路和 Azure 专用对等互连。 请遵循[创建 ExpressRoute 线路](expressroute-howto-circuit-arm.md)和[配置路由](expressroute-howto-routing-arm.md)中的说明。 ExpressRoute 线路应如下图所示：

    ![ExpressRoute 线路屏幕截图](./media/expressroute-howto-linkvnet-portal-resource-manager/routing1.png)
   
2. 现在你可以开始预配连接，以便将虚拟网络网关链接到 ExpressRoute 线路。 单击“连接” > “添加”以打开“添加连接”边栏选项卡，然后配置值。

    ![添加连接屏幕截图](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub1.png)  

3. 成功配置你的连接之后，连接对象将显示连接的信息。

     ![连接对象屏幕截图](./media/expressroute-howto-linkvnet-portal-resource-manager/samesub2.png)

### <a name="to-delete-a-connection"></a>删除连接
可以通过选择连接的边栏选项卡上的“删除”图标来删除连接。

## <a name="connect-a-virtual-network-in-a-different-subscription-to-a-circuit"></a>将不同订阅中的虚拟网络连接到线路
用户可以在多个订阅之间共享 ExpressRoute 线路。 下图是在多个订阅之间共享 ExpressRoute 线路的简单示意图。

![跨订阅连接](./media/expressroute-howto-linkvnet-portal-resource-manager/cross-subscription.png)

- 大型云中的每个较小云用于表示属于组织中不同部门的订阅。
- 组织中的每个部门可以使用自己的订阅部署其服务，但可以共享单个 ExpressRoute 线路以连接回本地网络。
- 单个部门（在此示例中为 IT 部门）可以拥有 ExpressRoute 线路。 组织内的其他订阅可以使用 ExpressRoute 线路。

    > [!NOTE]
    > 专用线路的连接和带宽费用将应用于 ExpressRoute 线路所有者。 所有虚拟网络共享相同的带宽。
    > 
    >

### <a name="administration"></a>管理
线路所有者是 ExpressRoute 线路资源的已授权超级用户。 线路所有者可以创建可由线路用户兑换的授权。 线路用户是虚拟网络网关的所有者（这些网关与 ExpressRoute 线路位于不同的订阅中）。 线路用户可以兑现授权（每个虚拟网络需要一个授权）。

线路所有者有权随时修改和撤消授权。 撤消授权将导致从撤消了访问权限的订阅中删除所有链路连接。

### <a name="circuit-owner-operations"></a>线路所有者操作

#### <a name="creating-an-authorization"></a>创建授权

线路所有者创建授权。 这样即可创建授权密钥，供线路用户用来将其虚拟网络网关连接到 ExpressRoute 线路。 一个授权只可用于一个连接。

1. 在“ExpressRoute”边栏选项卡中单击“授权”，然后键入授权的**名称**并单击“保存”。

    ![授权](./media/expressroute-howto-linkvnet-portal-resource-manager/authorization.png)

2. 保存配置后，复制“资源 ID”和“授权密钥”。

    ![授权密钥](./media/expressroute-howto-linkvnet-portal-resource-manager/authkey.png)

#### <a name="deleting-authorizations"></a>删除授权

可以通过选择连接的边栏选项卡上的“删除”图标来删除连接。

### <a name="circuit-user-operations"></a>线路用户操作

   > [!NOTE]
   > 线路用户需有资源 ID 以及线路所有者提供的授权密钥。 

#### <a name="redeeming-connection-authorizations"></a>兑现连接授权


在“基本信息”选项卡中填写详细信息，然后单击“确定”。

1.    单击“+新建”按钮。

    ![单击“新建”](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection1.png)

2.    在应用商店中搜索“连接”，然后选择它并单击“创建”。

    ![搜索连接](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection2.png)

3.    确保“连接类型”设置为“ExpressRoute”。


4.    在“基本信息”边栏选项卡中填写详细信息，然后单击“确定”。

    ![“基本信息”边栏选项卡](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection3.png)

5.    在“设置”边栏选项卡中，选择“虚拟网络网关”并选中“兑换授权”复选框。

6.    输入“授权密钥”和“对等线路 URI”，并为连接命名。 单击 **“确定”**。

    ![设置边栏选项卡](./media/expressroute-howto-linkvnet-portal-resource-manager/Connection4.png)

7. 在“摘要”边栏选项卡中检查信息，然后单击“确定”。

#### <a name="releasing-connection-authorizations"></a>释放连接授权

可以通过删除 ExpressRoute 线路与虚拟网络之间的连接释放授权。

## <a name="next-steps"></a>后续步骤
有关 ExpressRoute 的详细信息，请参阅 [ExpressRoute 常见问题](expressroute-faqs.md)。
