---
title: "如何在虚拟网络中使用公共 IP 地址"
description: "了解如何将虚拟网络配置为使用公共 IP 地址"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: b6e5dd7c-84ea-491d-8314-88e63a4da108
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6ac3d099430267b33192899dbe9c9c35eb25b02e


---
# <a name="public-ip-address-space-in-a-virtual-network-vnet"></a>虚拟网络 (VNet) 中的公共 IP 地址空间
虚拟网络 (VNet) 可以包含公共和专用（RFC 1918 地址块）IP 地址空间。 添加公共 IP 地址范围时，会将其视为只能在 VNet 内、互连 VNet 和本地位置访问的专用 VNet IP 地址空间的一部分。

下图显示了包含公共和专用 IP 地址空间的 VNet。

![公共 IP 概念](./media/virtual-networks-public-ip-within-vnet/IC775683.jpg)

## <a name="how-do-i-add-a-public-ip-address-range"></a>如何添加公共 IP 地址范围？
添加公共 IP 地址范围的方式与添加专用 IP 地址范围相同：使用 *netcfg* 文件，或通过在 [Azure 门户](http://portal.azure.com)中添加配置。 可以在创建 VNet 时添加公共 IP 地址范围，也可以稍后返回添加。 以下示例演示了在同一个 VNet 中配置的公共和专用 IP 地址空间。

![门户中的公共 IP 地址](./media/virtual-networks-public-ip-within-vnet/IC775684.png)

## <a name="are-there-any-limitations"></a>是否有任何限制？
有几个不允许使用的 IP 地址范围：

* 224.0.0.0/4（多播）
* 255.255.255.255/32（广播）
* 127.0.0.0/8（环回）
* 169.254.0.0/16（本地链路）
* 168.63.129.16/32（内部 DNS）

## <a name="next-steps"></a>后续步骤
[如何管理 VNet 使用的 DNS 服务器](virtual-networks-manage-dns-in-vnet.md)




<!--HONumber=Nov16_HO3-->


