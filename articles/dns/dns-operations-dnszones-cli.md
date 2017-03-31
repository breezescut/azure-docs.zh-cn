---
title: "在 Azure DNS 中管理 DNS 区域 - Azure CLI 2.0 | Microsoft 文档"
description: "可以使用 Azure CLI 2.0 管理 DNS 区域。 本文介绍如何在 Azure DNS 上更新、删除和创建 DNS 区域。"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: ca6347e079c0e85d1e9583f77b2e43632bedb206
ms.lasthandoff: 03/01/2017

---

# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a>如何使用 Azure CLI 2.0 管理 Azure DNS 中的 DNS 区域

> [!div class="op_single_selector"]
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

本指南介绍如何通过使用适用于 Windows、Mac 和 Linux 的跨平台 Azure CLI 管理 DNS 区域。 也可以使用 [Azure PowerShell](dns-operations-dnszones.md) 或 Azure 门户管理 DNS 记区域。

## <a name="cli-versions-to-complete-the-task"></a>用于完成任务的 CLI 版本

可以使用以下 CLI 版本之一完成任务：

* [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md) - 适用于经典部署模型和资源管理部署模型的 CLI。
* [Azure CLI 2.0](dns-operations-dnszones-cli.md) - 适用于资源管理部署模型的下一代 CLI。

## <a name="introduction"></a>介绍

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>为 Azure DNS 设置 Azure CLI 2.0

### <a name="before-you-begin"></a>开始之前

在开始配置之前，请确认你具有以下各项。

* Azure 订阅。 如果你还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)或注册获取[免费帐户](https://azure.microsoft.com/pricing/free-trial/)。

* 安装最新版本的 Azure CLI 2.0（在 Windows、Linux 或 MAC 中可用）。 有关详细信息，请参阅 [安装 Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2)。

### <a name="sign-in-to-your-azure-account"></a>登录到 Azure 帐户

打开控制台窗口并使用你的凭据进行身份验证。 有关详细信息，请参见“从 Azure CLI 登录 Azure”

```
az login
```

### <a name="select-the-subscription"></a>选择订阅

检查该帐户的订阅。

```
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>选择要使用的 Azure 订阅。

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>创建资源组

Azure 资源管理器要求所有资源组指定一个位置。 此位置将用作该资源组中的资源的默认位置。 但是，由于所有 DNS 资源都是全局性而非区域性的，因此资源组位置的选择不会影响 Azure DNS。

如果使用现有资源组，可跳过此步骤。

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>获取帮助

与 Azure DNS 相关的所有 CLI 2.0 命令都以 `az network dns` 开头。 可使用 `--help` 选项（缩写形式 `-h`）获取有关每个命令的帮助。  例如：

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>创建 DNS 区域

使用 `az network dns zone create` 命令创建 DNS 区域。 有关帮助，请参阅 `az network dns zone create -h`。

以下示例在名为 *MyResourceGroup* 的资源组中创建名为 *contoso.com* 的 DNS 区域：

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>使用标记创建 DNS 区域。

下面的示例演示如何通过 `--tags` 参数（缩写形式 `-t`）使用两个 [Azure Resource Manager 标记](dns-zones-records.md#tags)、project = demo 和 env = test创建 DNS 区域：

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>获取 DNS 区域

若要检索 DNS 区域，请使用 `az network dns zone show`。 有关帮助，请参阅 `az network dns zone show --help`。

以下示例从资源组 *MyResourceGroup* 返回 DNS 区域 *contoso.com* 及其关联数据。 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

以下示例是响应。

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

请注意，`az network dns zone show` 不返回 DNS 记录。 若要列出 DNS 记录，请使用 `az network dns record-set list`。


## <a name="list-dns-zones"></a>列出 DNS 区域

若要枚举 DNS 区域，请使用 `az network dns zone list`。 有关帮助，请参阅 `az network dns zone list --help`。

指定资源组仅列出资源组内的区域：

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

省略资源组可列出订阅中的所有区域：

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>更新 DNS 区域

可以使用 `az network dns zone update` 对 DNS 区域资源进行更改。 有关帮助，请参阅 `az network dns zone update --help`。

此命令不会更新区域中的任何 DNS 记录集（请参阅[如何管理 DNS 记录](dns-operations-recordsets-cli.md)）。 它仅用于更新区域资源本身的属性。 这些属性当前仅限于区域资源的 [Azure Resource Manager“标记”](dns-zones-records.md#tags)。

以下示例演示如何更新 DNS 区域上的标记。 现有标记替换为指定值。

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>删除 DNS 区域

可以使用 `az network dns zone delete` 删除 DNS 区域。 有关帮助，请参阅 `az network dns zone delete --help`。

> [!NOTE]
> 删除 DNS 区域也会删除该区域中的所有 DNS 记录。 此操作不可撤消。 如果 DNS 区域在使用中，则使用该区域的服务在区域删除后将无效。
>
>若要防止意外删除区域，请参阅[如何保护 DNS 区域和记录](dns-protect-zones-recordsets.md)。

此命令提示用户进行确认。 可选的 `--yes` 开关禁止显示此提示。

以下示例演示如何从资源组 *MyResourceGroup* 中删除区域 *contoso.com*。

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>后续步骤

了解如何在 DNS 区域中[管理记录集和记录](dns-getstarted-create-recordset-cli.md)。

了解如何[将域委派给 Azure DNS](dns-domain-delegation.md)。

