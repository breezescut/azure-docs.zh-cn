---
title: "将 Desired State Configuration 与虚拟机规模集配合使用 | Microsoft Docs"
description: "将虚拟机缩放集与 Azure DSC 扩展配合使用"
services: virtual-machine-scale-sets
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: c0d9ea4f506371ca21a08f6e1d6128c00123cbac


---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>将虚拟机缩放集与 Azure DSC 扩展配合使用
[虚拟机规模集 (VMSS)](virtual-machine-scale-sets-overview.md) 可与 [Azure 期望状态配置 (DSC)](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 扩展处理程序配合使用。 VMSS 提供部署和管理大量虚拟机的方法，并且可根据负载情况实现弹性扩大和缩小。 VM 联机时，DSC 用于配置 VM，使它们能够运行生产软件。

## <a name="differences-between-deploying-to-vm-and-vmss"></a>部署到 VM 和部署到 VMSS 之间的差异
VMSS 的基础模板结构与单一 VM 略有不同。 具体而言，单一 VM 将扩展部署在“virtualMachines”节点下面。 有一个“extensions”类型的入口，DSC 将通过此处添加到模板中

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

VMSS 节点有一个“properties”节，其中包含“VirtualMachineProfile”和“extensionProfile”属性。 DSC 添加在“extensions”下面

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-vmss"></a>VMSS 的行为
VMSS 的行为与单一 VM 的行为相同。 创建新 VM 后，会自动使用 DSC 扩展对其进行预配。 如果扩展需要更新的 WMF 版本，则 VM 会重新启动，然后联机。 VM 联机后，将下载 DSC 配置 .zip 文件，并在 VM 上预配该文件。 在 [Azure DSC 扩展概述](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)中可以找到详细信息。

## <a name="next-steps"></a>后续步骤
检查[适用于 DSC 扩展的 Azure Resource Manager 模板](../virtual-machines/virtual-machines-windows-extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

了解[DSC 扩展安全处理凭据](../virtual-machines/virtual-machines-windows-extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)的方法。 

有关 Azure DSC 扩展处理程序的详细信息，请参阅 [Azure Desired State Configuration 扩展处理程序简介](../virtual-machines/virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。 

有关 PowerShell DSC 的详细信息，请[访问 PowerShell 文档中心](https://msdn.microsoft.com/powershell/dsc/overview)。 




<!--HONumber=Nov16_HO3-->


