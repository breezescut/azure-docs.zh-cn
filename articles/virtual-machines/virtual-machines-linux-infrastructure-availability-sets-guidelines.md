---
title: "Azure 中 Linux VM 的可用性集 | Microsoft 文档"
description: "了解用于在 Azure 基础结构服务中部署可用性集的关键设计和实施准则。"
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 24f1d91c-8cc0-4251-bb67-ac4c4c37e8cd
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: b67f13d279d68712beea78c93d40881d92ba8896
ms.lasthandoff: 03/18/2017


---
# <a name="azure-availability-sets-guidelines-for-linux-vms"></a>适用于 Linux VM 的 Azure 可用性集准则

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

本文重点介绍可用性集的必需规划步骤，按照这些步骤可确保应用程序在计划内或计划外事件期间保持可访问性。

## <a name="implementation-guidelines-for-availability-sets"></a>可用性集的实施准则
决策：

* 你的应用程序基础结构中的各种角色和层需要多少可用性集？

任务：

* 定义所需的每个应用程序层中的 VM 数量。
* 确定是否需要调整用于你的应用程序的容错域或更新域数。
* 使用命名约定定义所需的可用性集以及其中包含的 VM。 一台 VM 只能驻留在一个可用性集内。 

## <a name="availability-sets"></a>可用性集
在 Azure 中，可将虚拟机 (VM) 置于名为可用性集的逻辑分组中。 在可用性集内创建 VM 时，Azure 平台在底层基础结构内分布放置这些 VM。 发生 Azure 平台或底层硬件/基础结构故障的计划内维护事件时，使用可用性集确保至少一台 VM 保持运行。

最佳做法是，不应使应用程序于仅驻留在单一 VM 上。 当 Azure 平台内发生计划内或计划外事件时，包含单一 VM 的可用性集得不到任何保护。 [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) 要求一个可用性集内有两台或更多 VM，以允许在底层基础结构内分布 VM。 如果使用[Azure 高级存储](../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，则 Azure SLA 适用于单个 VM。

Azure 中的底层基础结构分为多个硬件群集。 每个硬件群集可支持许多不同的 VM 大小。 任何时候都只能在单个硬件群集上托管可用性集。 因此，可在单个可用性集中存在的 VM 大小范围被限制为硬件群集支持的 VM 大小范围。 在可用性集中部署第一个 VM 时或在可用性集（所有 VM 当前均处于停止-解除分配状态）中启动第一个 VM 时，可用性集的硬件群集处于选中状态。 可使用以下 CLI 命令来确定可用于可用性集的 VM 大小范围：“az vm list-sizes --location \<string\>”

每个硬件群集分为多个更新域和容错域。 这些域是按共享公用更新周期或共享相同物理基础结构（如电源和网络）的主机定义的。 Azure 自动跨域分布可用性集中的 VM 来维持可用性和容错能力。 根据你的应用程序的大小和可用性集内的 VM 数，你可以调整要使用的域的数目。 参阅有关[管理更新域和容错域的可用性和使用](virtual-machines-linux-manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)的详细信息。

设计应用程序基础结构时，应规划好要使用的应用程序层。 将服务于同一目的的 VM 分为一组，放到可用性集中，例如用于容纳运行 nginx 或 Apache 的前端 VM 的可用性集。 为运行 MongoDB 或 MySQL 的后端 VM 创建另一个可用性集。 目标是确保应用程序的每个组件都受可用性集保护，且其至少一个实例将始终保持运行。

可对每个应用程序层利用负载平衡器，使其与可用性集一起工作，确保始终可将流量路由到正在运行的实例。 如果不使用负载均衡器，VM 可能会在计划内和计划外维护事件中继续运行；但如果主 VM 不可用，最终用户可能无法解决这些问题。

使用非托管磁盘时，在存储层针对高可用性设计应用程序。 最佳实践是为可用性集中的每个 VM 使用单独的存储帐户。 将与同一 VM 关联的所有磁盘（OS 和数据）放置在同一存储帐户中。 向存储帐户中添加更多 VHD 时，考虑存储帐户[限制](../storage/storage-scalability-targets.md)。 对于 [Azure 托管磁盘](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，将代为处理基础磁盘的分发。

## <a name="next-steps"></a>后续步骤
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]


