---
title: "自动缩放 Windows 虚拟机规模集 | Microsoft Docs"
description: "使用 Azure PowerShell 为 Windows 虚拟机规模集设置自动缩放"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 4fa0cdc2ee71d4e499dfc45bac7b690d728b626a
ms.openlocfilehash: 2649edd4af67e25ce49bf0ced858b7c7e4633290
ms.lasthandoff: 02/09/2017


---
# <a name="automatically-scale-machines-in-a-virtual-machine-scale-set"></a>自动缩放虚拟机规模集中的虚拟机
使用虚拟机规模集可以轻松地将相同的虚拟机作为集来进行部署和管理。 缩放集为超大规模应用程序提供高度可缩放且可自定义的计算层，并且它们支持 Windows 平台映像、Linux 平台映像、自定义映像和扩展。 有关规模集的详细信息，请参阅[虚拟机规模集](virtual-machine-scale-sets-overview.md)。

本教程演示如何创建 Windows 虚拟机的规模集，并自动缩放集中的虚拟机。 通过使用 Azure PowerShell 创建和部署 Azure Resource Manager 模板，可创建规模集并设置缩放。 有关模板的详细信息，请参阅[创作 Azure Resource Manager 模板](../azure-resource-manager/resource-group-authoring-templates.md)。 若要了解有关规模集自动缩放的详细信息，请参阅[自动缩放和虚拟机规模集](virtual-machine-scale-sets-autoscale-overview.md)。

在本文章中，将部署以下资源和扩展：

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

有关 Resource Manager 资源的详细信息，请参阅 [Azure Resource Manager 与经典部署](../azure-resource-manager/resource-manager-deployment-model.md)。

## <a name="step-1-install-azure-powershell"></a>步骤 1：安装 Azure PowerShell
有关安装最新版 Azure PowerShell、选择订阅和登录到 Azure 的信息，请参阅[如何安装和配置 Azure PowerShell](/powershell/azureps-cmdlets-docs)。

## <a name="step-2-create-a-resource-group-and-a-storage-account"></a>步骤 2：创建资源组和存储帐户
1. **创建资源组** - 所有资源都必须部署到资源组。 使用 [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx) 创建名为 **vmsstestrg1** 的资源组。
2. **创建存储帐户** - 此存储帐户是存储模板的位置。 使用 [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) 创建名为 **vmsstestsa** 的存储帐户。

## <a name="step-3-create-the-template"></a>步骤 3：创建模板
借助 Azure Resource Manager 模板，你可以使用资源和关联部署参数的 JSON 描述来统一部署和管理 Azure 资源。

1. 在常用的编辑器中，创建文件 C:\VMSSTemplate.json 并添加初始 JSON 结构，以支持模板。

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. 参数并不总是必需的，但在部署模板时它们提供了输入值的方法。 将这些参数添加到已添加到模板中的参数父元素下。

    ```json   
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
    * 用于访问规模集中虚拟机的单独虚拟机的名称。
    * 存储模板的存储帐户名称。
    * 最初在规模集中创建的虚拟机数。
    * 虚拟机上的管理员帐户的名称和密码。
    * 创建资源的名称前缀以支持规模集。

3. 可以在模板中使用变量指定可能经常发生变化的值或需要通过组合参数值创建的值。 将这些变量添加到已添加到模板中的变量父元素下。

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
      "wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\% Processor Time\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU utilization\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```
   
    * 网络接口所用的 DNS 名称。

        * 虚拟网络和子网的 IP 地址名称和前缀。
        * 虚拟网络、负载平衡器和网络接口的名称和标识符。
        * 与缩放集中虚拟机关联的帐户的存储帐户名称。
        * 已安装在虚拟机上的诊断扩展的设置。 有关诊断扩展的详细信息，请参阅[使用 Azure Resource Manager 模板创建具有监视和诊断功能的 Windows 虚拟机](../virtual-machines/virtual-machines-windows-extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

4. 将存储帐户资源添加到已添加到模板中的资源父元素下。 此模板使用一个循环来创建建议的&5; 个存储帐户，其中将存储操作系统磁盘和诊断数据。 这组帐户可在一个缩放集中最多支持 100 个虚拟机，这是当前的最大值。 每个存储帐户通过将变量中定义的字母指示符与模板的参数中提供的前缀组合来命名。

    ```json
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
      "apiVersion": "2015-06-15",
      "copy": {
        "name": "storageLoop",
        "count": 5
      },
      "location": "[resourceGroup().location]",
      "properties": { "accountType": "Standard_LRS" }
    },
    ```

5. 添加虚拟网络资源。 有关详细信息，请参阅[网络资源提供程序](../virtual-network/resource-groups-networking.md)。

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. 添加负载平衡器和网络接口所使用的公共 IP 地址资源。

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. 添加缩放集使用的负载平衡器资源。 有关详细信息，请参阅 [Azure Resource Manager 对负载平衡器的支持](../load-balancer/load-balancer-arm.md)。

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 3389
            }
          }
        ]
      }
    },
    ```

8. 添加单独虚拟机使用的网络接口资源。 由于规模集中的虚拟机不可使用公共 IP 地址访问，因此将在相同的虚拟网络中创建单独虚拟机，并使用它来远程访问虚拟机。

    ```json  
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. 在规模集所在的同一网络中添加单独的虚拟机。

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"        
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. 添加虚拟机规模集资源，并指定将在规模集中的所有虚拟机上安装的诊断扩展。 此资源的许多设置都与虚拟机资源相似。 主要区别在于指定规模集中虚拟机数量的容量元素和指定虚拟机更新方式的 upgradePolicy。 在所有存储帐户都根据 dependsOn 元素的指定创建之前，不会创建规模集。

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3], '.blob.core.windows.net/vhds')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4], '.blob.core.windows.net/vhds')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "MicrosoftWindowsServer",
              "offer": "WindowsServer",
              "sku": "2012-R2-Datacenter",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name": "Microsoft.Insights.VMDiagnosticsSettings",
                "properties": {
                  "publisher": "Microsoft.Azure.Diagnostics",
                  "type": "IaaSDiagnostics",
                  "typeHandlerVersion": "1.5",
                  "autoUpgradeMinorVersion": true,
                  "settings": {
                    "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount": "[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey": "[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint": "https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. 添加 autoscaleSettings 资源，该资源定义规模集如何根据规模集中虚拟机上的处理器使用率进行调整。

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor(_Total)\\% Processor Time",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```

    对于本教程，以下值非常重要：
    
    * **metricName**  
    此值与在 wadperfcounter 变量中定义的性能计数器相同。 使用该变量，诊断扩展可收集 **Processor(_Total)\% Processor Time** 计数器。
    
    * **metricResourceUri**  
    此值是虚拟机规模集的资源标识符。
    
    * **timeGrain**  
    此值是收集的指标的粒度。 在此模板中，它设置为&1; 分钟。
    
    * **statistic**  
    此值可确定如何组合指标以调节自动缩放操作。 可能的值包括：Average、Min、Max。 在此模板中，已收集虚拟机的平均总 CPU 使用率。
    
    * **timeWindow**  
    此值是收集实例数据的时间范围。 它必须介于 5 分钟和 12 个小时之间。
    
    * **timeAggregation**  
    此值可确定应如何随着时间的推移组合收集的数据。 默认值为 Average。 可能的值包括：Average、Minimum、Maximum、Last、Total、Count。
    
    * **operator**  
    此值是用于比较指标数据和阈值的运算符。 可能的值包括：Equals、NotEquals、GreaterThan、GreaterThanOrEqual、LessThan、LessThanOrEqual。
    
    * **threshold**  
    此值是触发缩放操作的值。 在此模板中，当缩放集中的虚拟机的平均 CPU 使用率超过 50% 时，会将虚拟机添加到缩放集。
    
    * **direction**  
    此值可确定在达到阈值时执行的操作。 可能的值为 Increase 或 Decrease。 在此模板中，如果在定义的时间窗口中阈值超过 50%，则将增加缩放集中的虚拟机数。
    
    * **类型**  
    此值是应发生的操作类型，且必须设置为 ChangeCount。
    
    * **value**  
    此值是已在规模集中添加或删除的虚拟机数。 此值必须大于或等于 1。 默认值为 1。 在此模板中，当达到该阈值时，缩放集中的虚拟机数将递增 1。
    
    * **cooldown**  
    此值是自上次缩放操作后，至下次操作发生之前要等待的时间。 此值必须介于一分钟到一周之间。

12. 保存模板文件。    

## <a name="step-4-upload-the-template-to-storage"></a>步骤 4：将模板上传到存储空间
只要知道在步骤 1 中创建的存储帐户的名称和主密钥，就可以上传模板。

1. 在 Microsoft Azure PowerShell 窗口中，设置变量以指定在步骤 1 中创建的存储帐户的名称。
   
    ```powershell
    $storageAccountName = "vmstestsa"
    ```

2. 设置用于指定存储帐户的主密钥的变量。
   
    ```powershell
    $storageAccountKey = "<primary-account-key>"
    ```
   
   可以通过在 Azure 门户中查看存储帐户资源时单击密钥图标来获取此密钥。
3. 创建用于使用存储帐户验证操作的存储帐户上下文对象。
   
    ```powershell
    $ctx = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
    ```

4. 创建用于存储模板的容器。

    ```powershell
    $containerName = "templates"
    New-AzureStorageContainer -Name $containerName -Context $ctx  -Permission Blob
    ```

5. 将模板文件上载到新容器。

    ```powershell   
    $blobName = "VMSSTemplate.json"
    $fileName = "C:\" + $BlobName
    Set-AzureStorageBlobContent -File $fileName -Container $containerName -Blob  $blobName -Context $ctx
    ```

## <a name="step-5-deploy-the-template"></a>步骤 5：部署模板
创建模板后，便可以开始部署资源了。 使用以下命令启动相关进程：

```powershell
New-AzureRmResourceGroupDeployment -Name "vmsstestdp1" -ResourceGroupName "vmsstestrg1" -TemplateUri "https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json"
```

当你按 Enter 时，系统将提示你为所指定的变量提供值。 提供以下值：

```powershell
vmName: vmsstestvm1
  vmSSName: vmsstest1
  instanceCount: 5
  adminUserName: vmadmin1
  adminPassword: VMpass1
  resourcePrefix: vmsstest
```

成功部署所有资源应大约需要 15 分钟。

> [!NOTE]
> 还可以使用门户的功能来部署资源。 请使用以下链接：“https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>”
> 
> 

## <a name="step-6-monitor-resources"></a>步骤 6：监视资源
你可以使用以下方法获取有关虚拟机缩放集的一些信息：

* Azure 门户 - 当前使用门户可以获取有限数量的信息。
* [Azure 资源浏览器](https://resources.azure.com/) - 要浏览规模集的当前状态，这是最好的工具。 遵循此路径，你应该看到所创建的缩放集的实例视图：
  
    订阅 > {你的订阅} > resourceGroups > vmsstestrg1 > 提供程序 > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines

* Azure PowerShell - 使用此命令可获取一些信息：

  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  ```
  
  或
  
  ```powershell
  Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceView
  ```

* 就像连接任何其他虚拟机一样连接到单独虚拟机，然后可以远程访问规模集中的虚拟机，以监视单个进程。

> [!NOTE]
> 用于获取有关规模集的信息的完整 REST API 可在[虚拟机规模集](https://msdn.microsoft.com/library/mt589023.aspx)中找到

## <a name="step-7-remove-the-resources"></a>步骤 7：删除资源
由于你需要为 Azure 中使用的资源付费，因此，删除不再需要的资源总是一种良好的做法。 你不需要单独从资源组中删除每个资源， 删除资源组，其中包含的所有资源即会自动删除。

  ```powershell
  Remove-AzureRmResourceGroup -Name vmsstestrg1
  ```

如果要保留资源组，可以仅删除缩放集。

  ```powershell
  Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name"
  ```

## <a name="next-steps"></a>后续步骤
* 使用[在虚拟机规模集中管理虚拟机](virtual-machine-scale-sets-windows-manage.md)中的信息管理刚刚创建的规模集。
* 若要了解有关垂直缩放的详细信息，请参阅[虚拟机规模集垂直自动缩放](virtual-machine-scale-sets-vertical-scale-reprovision.md)
* 在 [Azure 监视器 PowerShell 快速入门示例](../monitoring-and-diagnostics/insights-powershell-samples.md)中查找 Azure 监视器监视功能的示例
* 若要了解有关通知功能的相关信息，请参阅[使用自动缩放操作在 Azure 监视器中发送电子邮件和 webhook 警报通知](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* 了解如何[使用审核日志在 Azure 监视器中发送电子邮件和 webhook 警报通知](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)


