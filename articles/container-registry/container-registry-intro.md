---
title: "Azure 中的专用 Docker 容器注册表 | Microsoft 文档"
description: "介绍 Azure 容器注册表服务，该服务提供基于云的托管专用 Docker 注册表。"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 0433e22dc7722ef9c8edfaf949dbd9a9d8645e67
ms.openlocfilehash: 69d1750f13b5507268229b29a392c38662c0f5f0
ms.lasthandoff: 03/02/2017

---
# <a name="introduction-to-private-docker-container-registries"></a>专用 Docker 容器注册表简介
> [!NOTE]
> 容器注册表目前以预览版提供。


Azure 容器注册表是基于开源 Docker 注册表 v2 的托管 [Docker 注册表](https://docs.docker.com/registry/)服务。 可以创建和维护 Azure 容器注册表来存储与管理专用的 [Docker 容器](https://www.docker.com/what-docker)映像。 可将 Azure 中的容器注册表与现有的容器开发和部署管道配合使用，现成地吸收 Docker 社区的专业知识。

有关 Docker 和容器的背景信息，请参阅：

* [Docker user guide](https://docs.docker.com/engine/userguide/)（Docker 用户指南）
* [Azure Container Registry preview announcement](https://azure.microsoft.com/blog/azure-container-registry-preview/)（Azure 容器注册表预览版通告） 

## <a name="key-concepts"></a>关键概念
* **注册表** - 在 Azure 订阅中创建一个或多个容器注册表。 每个注册表由同一位置的标准 Azure [存储帐户](../storage/storage-introduction.md)提供支持。 在与部署相同的 Azure 位置创建注册表可以利用容器映像的本地闭合网络存储。 

  注册表在根域中根据订阅的 [Azure Active Directory 租户](../active-directory/active-directory-howto-tenant.md)命名。 例如，如果你在 Contoso 域中有一个组织帐户，则完全限定的注册表名称采用 `myregistry-contoso.azurecr.io` 格式。 
  
  可以使用使用 Azure Active Directory 支持的 [服务主体](../active-directory/active-directory-application-objects.md)或提供的管理员帐户来[控制访问](container-registry-authentication.md)容器注册表。 运行标准 `docker login` 命令可对注册表进行身份验证。 

* **存储库** - 一个注册表包含一个或多个存储库（容器映像组）。 Azure 容器注册表支持多级存储库命名空间。 使用此功能可将特定应用相关的映像集合分组，或者将特定开发或运营团队的应用集合分组。 例如：
  
  * `myregistry-contoso.azurecr.io/aspnetcore:1.0.1` 表示企业范围的映像
  * `myregistry-contoso.azurecr.io/warrantydept/dotnet-build` 表示用于构建 .NET 应用、在保修部门之间共享的映像
  * `myregistry-contoso.azrecr.io/warrantydept/customersubmissions/web` 表示一个 Web 映像，它已在 constomersubmissions 应用中分组，由保修部门拥有

* **映像** - 存储在存储库中，每个映像是 Docker 容器的只读快照。 Azure 容器注册表可以包含 Windows 和 Linux 映像。 可以控制所有容器部署的映像名称。 使用标准 [Docker 命令](https://docs.docker.com/engine/reference/commandline/)可将映像推送到存储库，或者从存储库中提取映像。 

* **容器** - 容器定义软件应用程序及其在完整文件系统中包装的依赖项，包括代码、运行时、系统工具和库。 可以基于从容器注册表提取的 Windows 或 Linux 映像运行 Docker 容器。 在一台计算机上运行的容器共享操作系统内核。 Docker 容器完全可移植到所有主要 Linux 分发版、Mac 和 Windows。

## <a name="use-cases"></a>用例
将 Azure 容器注册表中的映像提取到各种部署目标：

* 用于跨主机群集管理容器化应用程序的**可缩放协调系统**，包括 [DC/OS](https://docs.mesosphere.com/)、[Docker Swarm](https://docs.docker.com/swarm/) 和 [Kubernetes](http://kubernetes.io/docs/)。
* 支持大规模构建和运行应用程序的 **Azure 服务**，包括[容器服务](../container-service/index.md)、[应用服务](/app-service/index.md)、[批处理](../batch/index.md)和 [Service Fabric](../service-fabric/index.md)。 

开发人员还可以在执行容器开发工作流的过程中将内容推送到容器注册表。 例如，通过连续集成和部署工具（如 [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) 或 [Jenkins](https://jenkins.io/)）将目标设置为容器注册表。





## <a name="next-steps"></a>后续步骤
* [使用 Azure 门户创建容器注册表](container-registry-get-started-portal.md)
* [使用 Azure CLI 创建容器注册表](container-registry-get-started-azure-cli.md)
* [使用 Docker CLI 推送第一个映像](container-registry-get-started-docker-cli.md)
* 若要使用 Visual Studio Team Services、Azure 容器服务和 Azure 容器注册表生成连续集成和部署工作流，请参阅[此教程](../container-service/container-service-setup-ci-cd.md)。
* 如果想要在 Azure 中设置自己的 Docker 专用注册表（不带公共终结点），请参阅 [Deploying Your Own Private Docker Registry on Azure](../virtual-machines/virtual-machines-linux-docker-registry-in-blob-storage.md)（在 Azure 上部署自己的专用 Docker 注册表）。

