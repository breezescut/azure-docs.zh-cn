---
title: "在 Azure 中不到&5; 分钟创建你的第一个 Java Web 应用 | Microsoft 文档"
description: "了解如何通过部署一个示例应用，轻松地在应用服务中运行 Web 应用。"
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: ab9b49cd86f37499ebf8fd8162779be305019f36
ms.lasthandoff: 03/18/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>在 Azure 中不到&5; 分钟创建你的第一个 Java Web 应用
[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)]

本快速入门帮助你在数分钟内将你的第一个 Java Web 应用部署到 [Azure 应用服务](../app-service/app-service-value-prop-what-is.md)。

在开始之前，请确保已安装 Azure CLI。 有关详细信息，请参阅 [Azure CLI 安装指南](https://docs.microsoft.com/cli/azure/install-azure-cli)。

## <a name="log-in-to-azure"></a>登录 Azure
运行 `az login` 并按屏幕说明进行操作，以便登录到 Azure。
   
```azurecli
az login
```
   
## <a name="create-a-resource-group"></a>创建资源组   
创建[资源组](../azure-resource-manager/resource-group-overview.md)。 这是放置所有 Azure 资源（例如 Web 应用及其 SQL 数据库后端）的地方，这些资源需要集中进行管理。

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

若要查看适用于 `---location` 的可能值，请使用 `az appservice list-locations` Azure CLI 命令。

## <a name="create-an-app-service-plan"></a>创建应用服务计划
创建“免费”[应用服务计划](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。 

```azurecli
az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
```

## <a name="create-a-web-app"></a>创建 Web 应用
使用 `<app_name>` 中的唯一名称创建 Web 应用。

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
```

## <a name="deploy-sample-application"></a>部署示例应用程序
从 GitHub 部署示例 Java 应用。

```azurecli
az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
--repo-url "https://github.com/azure-appservice-samples/JavaCoffeeShopTemplate.git" --branch master --manual-integration 
```

## <a name="browse-to-web-app"></a>浏览到 Web 应用
若要查看应用在 Azure 中的实时运行情况，请运行此命令。

```azurecli
az appservice web browse --name <app_name> --resource-group myResourceGroup
```

恭喜，你的第一个 Java Web 应用已在 Azure 应用服务中实时运行！

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>后续步骤

浏览预先创建的 [Web 应用 CLI 脚本](app-service-cli-samples.md)。

