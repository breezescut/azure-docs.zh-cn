---
title: "配置独立群集 | Microsoft 文档"
description: "本文介绍如何配置独立的或专用的 Service Fabric 群集。"
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: 
ms.assetid: 0c5ec720-8f70-40bd-9f86-cd07b84a219d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/17/2017
ms.author: ryanwi
translationtype: Human Translation
ms.sourcegitcommit: 4cde82601758c9f92ab36c692265a8b6c192cbdc
ms.openlocfilehash: eef19d304ec63d752b6b84c78833af44ca5344d2
ms.lasthandoff: 02/21/2017


---
# <a name="configuration-settings-for-standalone-windows-cluster"></a>Windows 独立群集的配置设置
本文介绍如何使用 ***ClusterConfig.JSON*** 文件来配置独立的 Service Fabric 群集。 可以使用此文件指定 Service Fabric 群集的信息，例如 Service Fabric 节点及其 IP 地址、群集上不同类型的节点、安全配置，以及使用独立群集的容错域/升级域定义的网络拓扑。

[下载独立的 Service Fabric 包](service-fabric-cluster-creation-for-windows-server.md#downloadpackage)时，一些 ClusterConfig.JSON 文件示例将下载到你的工作计算机。 名称中包含 *DevCluster* 的示例可帮助你在同一台计算机上创建包含所有三个节点（类似于逻辑节点）的群集。 在这些节点中，必须将一个节点标记为主节点。 此群集可用于开发或测试环境，不支持用作生产群集。 名称中包含 *MultiMachine* 的示例可帮助你创建生产质量群集，其中的每个节点位于不同的计算机上。 这些群集的主节点数将基于[可靠性级别](#reliability)。

1. *ClusterConfig.Unsecure.DevCluster.JSON* 和 *ClusterConfig.Unsecure.MultiMachine.JSON* 分别说明如何创建不安全的测试群集和生产群集。 
2. *ClusterConfig.Windows.DevCluster.JSON* 和 *ClusterConfig.Windows.MultiMachine.JSON* 说明如何创建使用 [Windows 安全性](service-fabric-windows-cluster-windows-security.md)保护的测试群集和生产群集。
3. *ClusterConfig.X509.DevCluster.JSON* 和 *ClusterConfig.X509.MultiMachine.JSON* 说明如何创建使用[基于 X509 证书的安全性](service-fabric-windows-cluster-x509-security.md)保护的测试群集和生产群集。 

现在，我们查看 ***ClusterConfig.JSON*** 文件的以下各个节。

## <a name="general-cluster-configurations"></a>常规群集配置
此部分介绍各种特定于群集的配置，如下面的 JSON 代码段所示。

    "name": "SampleCluster",
    "clusterConfigurationVersion": "1.0.0",
    "apiVersion": "2016-09-26",

可为 Service Fabric 群集指定任何友好名称，只需将该名称分配到 **name** 变量即可。 **clusterConfigurationVersion** 是群集的版本号；每次升级 Service Fabric 群集时，都应该递增该号码。 不过，应该将 **apiVersion** 保留为默认值。

<a id="clusternodes"></a>

## <a name="nodes-on-the-cluster"></a>群集上的节点
可以使用 **nodes** 节配置 Service Fabric 群集上的节点，如以下代码片段所示。

    "nodes": [{
        "nodeName": "vm0",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType0",
        "faultDomain": "fd:/dc1/r0",
        "upgradeDomain": "UD0"
    }, {
        "nodeName": "vm1",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType1",
        "faultDomain": "fd:/dc1/r1",
        "upgradeDomain": "UD1"
    }, {
        "nodeName": "vm2",
        "iPAddress": "localhost",
        "nodeTypeRef": "NodeType2",
        "faultDomain": "fd:/dc1/r2",
        "upgradeDomain": "UD2"
    }],

一个 Service Fabric 群集必须至少包含 3 个节点。 你可以根据设置向此部分添加更多节点。 下表说明了每个节点的配置设置。

| **节点配置** | **说明** |
| --- | --- |
| nodeName |你可以为节点提供任何友好名称。 |
| iPAddress |打开命令窗口并键入 `ipconfig`，找出节点的 IP 地址。 记下 IPV4 地址，并将其分配给 **iPAddress** 变量。 |
| nodeTypeRef |可以为每个节点分配不同的节点类型。 [节点类型](#nodetypes)在以下节中定义。 |
| faultDomain |容错域可让群集管理员定义可能因共享的物理依赖项而同时发生故障的物理节点。 |
| upgradeDomain |升级域描述几乎在相同时间关闭以进行 Service Fabric 升级的节点集。 你可以选择将哪些节点分配到哪些升级域，因为这不受任何物理要求的限制。 |

## <a name="cluster-properties"></a>群集属性
ClusterConfig.JSON 中的 **properties** 节用于配置群集，如下所示。

<a id="reliability"></a>

### <a name="reliability"></a>可靠性
**reliabilityLevel** 节定义可在群集的主节点上运行的系统服务副本数。 此项可提高这些服务以及此群集的可靠性。 可以将此变量设置为 *Bronze*、*Silver*、*Gold* 或 *Platinum*，这样就可以分别运行这些服务的 3、5、7、9 个副本。 请参阅以下示例。

    "reliabilityLevel": "Bronze",

请注意，由于一个主节点运行系统服务的一个副本，因此，实现 *Bronze* 可靠性级别，至少需要 3 个主节点；实现 *Silver* 可靠性级别，至少需要 5 个主节点；实现 *Gold* 可靠性级别，至少需要 7 个主节点；实现 *Platinum* 可靠性级别，至少需要 9 个主节点。

### <a name="diagnostics"></a>诊断
可以使用 **diagnosticsStore** 节来配置参数，以便能够诊断和排查节点或群集故障，如以下代码片段中所示。 

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "FileShare",
        "IsEncrypted": "false",
        "connectionstring": "c:\\ProgramData\\SF\\DiagnosticsStore"
    }

**metadata** 用于描述群集诊断，可以根据具体的情况进行设置。 这些变量用于收集 ETW 跟踪日志、故障转储和性能计数器。 有关 ETW 跟踪日志的详细信息，请阅读 [Tracelog](https://msdn.microsoft.com/library/windows/hardware/ff552994.aspx) 和 [ETW 跟踪](https://msdn.microsoft.com/library/ms751538.aspx)。 可将包含[故障转储](https://blogs.technet.microsoft.com/askperf/2008/01/08/understanding-crash-dump-files/)和[性能计数器](https://msdn.microsoft.com/library/windows/desktop/aa373083.aspx)的所有日志定向到计算机上的 **connectionString** 文件夹。 还可以使用 *AzureStorage* 来存储诊断信息。 请参阅下面的示例代码片段。

    "diagnosticsStore": {
        "metadata":  "Please replace the diagnostics store with an actual file share accessible from all cluster machines.",
        "dataDeletionAgeInDays": "7",
        "storeType": "AzureStorage",
        "IsEncrypted": "false",
        "connectionstring": "xstore:DefaultEndpointsProtocol=https;AccountName=[AzureAccountName];AccountKey=[AzureAccountKey]"
    }

### <a name="security"></a>“安全”
对于安全的 Service Fabric 独立群集，必须使用 **security** 节。 以下代码段显示了该部分的一部分内容。

    "security": {
        "metadata": "This cluster is secured using X509 certificates.",
        "ClusterCredentialType": "X509",
        "ServerCredentialType": "X509",
        . . .
    }

**metadata** 用于描述安全群集，可根据具体的情况进行设置。 **ClusterCredentialType** 和 **ServerCredentialType** 确定群集与节点将要实现的安全类型。 可以将这两项设置为 *X509* 来实现基于证书的安全性，或者设置为 *Windows* 来实现基于 Azure Active Directory 的安全性。 **security** 节的余下设置基于安全类型。 若要了解如何填充 **security** 节的余下设置，请参阅[独立群集中基于证书的安全性](service-fabric-windows-cluster-x509-security.md)，或 [独立群集中的 Windows 安全性](service-fabric-windows-cluster-windows-security.md)。

<a id="nodetypes"></a>

### <a name="node-types"></a>节点类型
**nodeTypes** 节描述群集中的节点类型。 一个群集必须指定至少一个节点类型，如以下代码段所示。 

    "nodeTypes": [{
        "name": "NodeType0",
        "clientConnectionEndpointPort": "19000",
        "clusterConnectionEndpointPort": "19001",
        "leaseDriverEndpointPort": "19002"
        "serviceConnectionEndpointPort": "19003",
        "httpGatewayEndpointPort": "19080",
        "reverseProxyEndpointPort": "19081",
        "applicationPorts": {
            "startPort": "20575",
            "endPort": "20605"
        },
        "ephemeralPorts": {
            "startPort": "20606",
            "endPort": "20861"
        },
        "isPrimary": true
    }]

**name** 是此特定节点类型的友好名称。 若要创建这种类型的节点，请[如上所述](#clusternodes)，将该节点的友好名称分配到其 **nodeTypeRef** 变量。 对于每个节点类型，请定义要使用的连接终结点。 你可以为这些连接终结点选择任意端口号，只要不与此群集中的任何其他终结点冲突即可。 在多节点群集中，根据 [**reliabilityLevel**](#reliability)，将有一个或多个主节点（即，**isPrimary** 设置为 *true*）。 请参阅 [Service Fabric 群集容量规划注意事项](service-fabric-cluster-capacity.md)，了解有关 **nodeTypes** 和 **reliabilityLevel** 的信息，以及什么是主节点类型和非主节点类型。 

#### <a name="endpoints-used-to-configure-the-node-types"></a>用于配置节点类型的终结点
* *clientConnectionEndpointPort* 是使用客户端 API 时，客户端用来连接群集的端口。 
* *clusterConnectionEndpointPort* 是节点相互通信时使用的端口。
* *leaseDriverEndpointPort* 是群集租用驱动程序用来判断节点是否仍处于活动状态的端口。 
* *serviceConnectionEndpointPort* 是节点上部署的应用程序和服务用来与该特定节点上的 Service Fabric 客户端通信的端口。
* *httpGatewayEndpointPort* 是 Service Fabric Explorer 用来连接群集的端口。
* *ephemeralPorts* 替代 [OS 使用的动态端口](https://support.microsoft.com/kb/929851)。 Service Fabric 将使用其中的一部分端口作为应用程序端口，剩余的端口供 OS 使用。 它还会将此范围映射到 OS 中的现有范围，因此，无论何时，你都可以使用示例 JSON 文件中指定的范围。 需要确保起始端口与结束端口至少相差 255。 如果此差异过低，则可能会遇到冲突，因为此范围与操作系统共享。 通过运行 `netsh int ipv4 show dynamicport tcp` 来查看配置的动态端口范围。
* *applicationPorts* 是 Service Fabric 应用程序使用的端口。 这些端口应是 *ephemeralPorts* 的子集，其范围足以满足应用程序的终结点要求。 每当需要新端口时，Service Fabric 将使用这些端口，并负责为这些端口打开防火墙。 
* *reverseProxyEndpointPort* 是可选的反向代理终结点。 有关详细信息，请参阅 [Service Fabric 反向代理](service-fabric-reverseproxy.md)。 

### <a name="log-settings"></a>日志设置
使用 **fabricSettings** 节可以设置 Service Fabric 数据和日志的根目录。 只能在初次创建群集的时候自定义这些设置。 下面是此部分的代码段示例。

    "fabricSettings": [{
        "name": "Setup",
        "parameters": [{
            "name": "FabricDataRoot",
            "value": "C:\\ProgramData\\SF"
        }, {
            "name": "FabricLogRoot",
            "value": "C:\\ProgramData\\SF\\Log"
    }]

建议使用非 OS 驱动器作为 FabricDataRoot 和 FabricLogRoot，因为它能够更可靠地防范 OS 崩溃。 请注意，如果只自定义数据根目录，则会将日志根目录放置在比数据根目录低一级的位置。

### <a name="stateful-reliable-service-settings"></a>有状态的 Reliable Service 设置
**KtlLogger** 部分允许设置 Reliable Services 的全局配置设置。 有关这些设置的详细信息，请阅读[配置有状态 Reliable Services](service-fabric-reliable-services-configuration.md)。
下面的示例演示如何更改创建的共享事务日志，以备份有状态服务的任何可靠集合。

    "fabricSettings": [{
        "name": "KtlLogger",
        "parameters": [{
            "name": "SharedLogSizeInMB",
            "value": "4096"
        }]
    }]

## <a name="next-steps"></a>后续步骤
根据独立群集设置配置一个完整的 ClusterConfig.JSON 文件后，可以遵循[创建独立 Service Fabric 群集](service-fabric-cluster-creation-for-windows-server.md)一文中所述步骤部署群集，然后继续[使用 Service Fabric Explorer 可视化群集](service-fabric-visualizing-your-cluster.md)。


