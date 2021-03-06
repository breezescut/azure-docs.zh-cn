---
title: "Service Fabric 项目创建后续步骤 | Microsoft 文档"
description: "本文包含针对 Service Fabric 执行的一组核心开发任务的链接"
services: service-fabric
documentationcenter: .net
author: seanmck
manager: timlt
editor: 
ms.assetid: 299d1f97-1ca9-440d-9f81-d1d0dd2bf4df
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2016
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b4b2424e5efe3392b08e58ceb05ec63f15c7ad32


---
# <a name="your-service-fabric-application-and-next-steps"></a>Service Fabric 应用程序和后续步骤
已创建你的 Azure Service Fabric 应用程序。 本文说明项目的构成以及有可能要执行的一些后续步骤。

## <a name="your-application"></a>你的应用程序
每个新应用程序都包含一个应用程序项目。 根据选择的服务类型，可能有一个或两个附加的项目。

### <a name="the-application-project"></a>应用程序项目
应用程序项目包括：

* 对构成应用程序的服务的引用集。
* 三个发布配置文件（1-本地节点、5-本地节点和云），可用于维护首选项以适应不同的环境，诸如与群集终结点相关的首选项，以及是否按默认执行升级部署。
* 三个应用程序参数文件（同上），可用于维护环境特定的应用程序配置，例如，要为服务创建的分区数目。
* 可使用部署脚本从命令行部署应用程序，或者通过自动持续集成和部署管道来部署应用程序。
* 用于描述应用程序的应用程序清单。 可以在 ApplicationPackageRoot 文件夹下查找清单。

### <a name="stateless-service"></a>无状态服务
当添加新的无状态服务时，Visual Studio 将在解决方案中添加一个服务项目，其中包含继承自 `StatelessService` 的类型。 该服务将递增计数器中的本地变量。

### <a name="stateful-service"></a>有状态服务
当添加新的有状态服务时，Visual Studio 将在解决方案中添加一个服务项目，其中包含继承自 `StatefulService` 的类型。 该服务将在其 `RunAsync` 方法中递增计数器，并将结果存储在 `ReliableDictionary` 中。

### <a name="actor-service"></a>执行组件服务
当你添加新的可靠执行组件时，Visual Studio 会将两个项目添加到你的解决方案：执行组件项目和接口项目。

执行组件项目提供所需的方法用于设置和获取可靠保存在执行组件状态中的计数器的值。 接口项目提供其他服务可用来调用执行组件的接口。

### <a name="stateless-web-api"></a>无状态 Web API
无状态 Web API 项目提供一个基本 Web 服务，可用于向外部客户端打开你的应用程序。 有关如何构建该项目的信息，请参阅 [Service Fabric Web API 服务与 OWIN 自托管](service-fabric-reliable-services-communication-webapi.md)。

### <a name="aspnet-core"></a>ASP.NET core
Service Fabric SDK 提供相同的一组可用于独立 ASP.NET Core 项目的 ASP.NET Core 模板：空的，[Web API][aspnet-webapi] 和 [Web 应用程序][aspnet-webapp]。

## <a name="next-steps"></a>后续步骤
### <a name="create-an-azure-cluster"></a>创建 Azure 群集
Service Fabric SDK 提供一个用于开发和测试的本地群集。 若要在 Azure 中创建群集，请参阅[从 Azure 门户设置 Service Fabric 群集][create-cluster-in-portal]。

### <a name="try-deploying-to-azure-for-free-with-party-clusters"></a>尝试使用合作群集免费部署到 Azure
如果要尝试在 Azure 中部署和管理应用程序但不设置自己的群集，可以使用免费的[合作群集服务](http://aka.ms/tryservicefabric)。

### <a name="publish-your-application-to-azure"></a>将应用程序发布到 Azure
可以直接从 Visual Studio 将应用程序发布到 Azure 群集。 若要了解操作方法，请参阅 [将应用程序发布到 Azure][publish-app-to-azure]。

### <a name="use-service-fabric-explorer-to-visualize-your-cluster"></a>使用 Service Fabric Explorer可视化群集
Service Fabric Explorer 提供一种用于可视化群集（包括已部署的应用程序和物理布局）的简易方法。 有关详细信息，请参阅[使用 Service Fabric Explorer 可视化群集][visualize-with-sfx]。

### <a name="version-and-upgrade-your-services"></a>对服务进行版本控制和升级
Service Fabric 支持单独对应用程序中的独立服务进行版本控制和升级。 若要了解详细信息，请参阅[对服务进行版本控制和升级][app-upgrade-tutorial]。

### <a name="configure-continuous-integration-with-visual-studio-team-services"></a>配置与 Visual Studio Team Services 的持续集成
要了解如何为 Service Fabric 应用程序设置持续集成过程，请参阅[使用 Visual Studio Team Services 配置持续集成][ci-with-vso]。

<!-- Links -->
[add-web-frontend]: service-fabric-add-a-web-frontend.md
[create-cluster-in-portal]: service-fabric-cluster-creation-via-portal.md
[publish-app-to-azure]: service-fabric-publish-app-remote-cluster.md
[visualize-with-sfx]: service-fabric-visualizing-your-cluster.md
[ci-with-vso]: service-fabric-set-up-continuous-integration.md
[reliable-services-webapi]: service-fabric-reliable-services-communication-webapi.md
[app-upgrade-tutorial]: service-fabric-application-upgrade-tutorial.md
[aspnet-webapi]: https://docs.asp.net/en/latest/tutorials/first-web-api.html
[aspnet-webapp]: https://docs.asp.net/en/latest/tutorials/first-mvc-app/index.html



<!--HONumber=Nov16_HO3-->


