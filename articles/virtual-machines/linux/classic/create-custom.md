---
title: "使用 Azure CLI 1.0 创建经典 Linux VM | Microsoft Docs"
description: "了解如何通过 Azure CLI 1.0 和经典部署模型创建 Linux 虚拟机"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: c4d29c453d22a2765f0884c11d98d84676b3bf32
ms.lasthandoff: 03/27/2017


---
# <a name="how-to-create-a-classic-linux-vm-with-the-azure-cli-10"></a>如何使用 Azure CLI 1.0 创建经典 Linux VM
> [!IMPORTANT] 
> Azure 提供两个不同的部署模型用于创建和处理资源：[Resource Manager 和经典模型](../../../resource-manager-deployment-model.md)。 本文介绍如何使用经典部署模型。 Microsoft 建议大多数新部署使用资源管理器模型。 有关 Resource Manager 版本，请参阅[此处](../../virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

本主题介绍如何通过 Azure CLI 1.0 和经典部署模型创建 Linux 虚拟机 (VM)。 本文使用 Azure 上可用**映像**中的 Linux 映像。 Azure CLI 1.0 命令提供以下配置选项以及其他选项：

* 将 VM 连接到虚拟网络
* 将 VM 添加到现有云服务
* 将 VM 添加到现有存储帐户
* 将 VM 添加到可用性集或位置

> [!IMPORTANT]
> 如果希望虚拟机使用虚拟网络，以便可以按主机名直接连接到它或者设置跨界连接，请确保在创建 VM 时指定虚拟网络。 仅在创建 VM 时，才能将该 VM 配置为加入虚拟网络。 有关虚拟网络的详细信息，请参阅 [Azure 虚拟网络概述](http://go.microsoft.com/fwlink/p/?LinkID=294063)。
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>如何使用经典部署模型创建 Linux VM
[!INCLUDE [virtual-machines-create-LinuxVM](../../../../includes/virtual-machines-create-linuxvm.md)]


