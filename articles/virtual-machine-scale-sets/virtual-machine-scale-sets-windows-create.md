---
title: "使用 PowerShell 创建 Azure 虚拟机规模集 | Microsoft Docs"
description: "使用 PowerShell 创建 Azure 虚拟机规模集"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/21/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 1f8e66fac5b82698525794f0486dd0432c7421a7
ms.openlocfilehash: 7286fed39839675eb960b749f3235f83e36c5e9a
ms.lasthandoff: 02/22/2017


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>使用 Azure PowerShell 创建 Windows 虚拟机规模集
这些步骤采用填空方法来创建 Azure 虚拟机规模集。 有关规模集的详细信息，请参阅[虚拟机规模集概述](virtual-machine-scale-sets-overview.md)。

执行本文中的步骤大约需要 30 分钟时间。

## <a name="step-1-install-azure-powershell"></a>步骤 1：安装 Azure PowerShell
有关安装最新版 Azure PowerShell、选择订阅和登录到帐户的信息，请参阅[如何安装和配置 Azure PowerShell](/powershell/azureps-cmdlets-docs)。

## <a name="step-2-create-resources"></a>步骤 2：创建资源
创建新规模集所需的资源。

### <a name="resource-group"></a>资源组
虚拟机规模集必须包含在资源组中。

1. 获取可以放置资源的可用位置列表：
   
        Get-AzureLocation | Sort Name | Select Name
2. 选择最适合自己的位置，将 **$locName** 的值替换为该位置名称，然后创建变量：
   
        $locName = "location name from the list, such as Central US"
3. 将 **$rgName** 的值替换为新资源组要使用的名称，然后创建变量： 
   
        $rgName = "resource group name"
4. 创建资源组：
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    用户应看到与此示例类似的内容：
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="virtual-network"></a>虚拟网络
规模集中的虚拟机必须具有虚拟网络。

1. 将“$subnetName”的值替换为虚拟网络中子网要使用的名称，然后创建变量： 
   
        $subnetName = "subnet name"
2. 创建子网配置：
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    地址前缀在虚拟网络中可能会有所不同。
3. 将 **$netName** 的值替换为虚拟网络要使用的名称，然后创建变量： 
   
        $netName = "virtual network name"
4. 创建虚拟网络：
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>规模集的配置
您已拥有了规模集配置需要的所有资源，因此，让我们一起来创建它。  

1. 将 **$ipName** 的值替换为 IP 配置要使用的名称，然后创建变量： 
   
        $ipName = "IP configuration name"
2. 创建 IP 配置：
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. 将 **$vmssConfig** 的值替换为规模集配置要使用的名称，然后创建变量：   
   
        $vmssConfig = "Scale set configuration name"
4. 创建规模集的配置：
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    此示例演示了使用三台虚拟机创建规模集。 有关规模集容量的详细信息，请参阅[虚拟机规模集概述](virtual-machine-scale-sets-overview.md)。 此步骤还包括在规模集中设置虚拟机的大小（也称为 SkuName）。 若要查找符合需要的大小，请查看[虚拟机的大小](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。
5. 将网络接口配置添加到规模集配置中：
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    用户应看到与此示例类似的内容：
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>操作系统配置文件
1. 将 **$computerName** 的值替换为要使用的计算机名称前缀，然后创建变量： 
   
        $computerName = "computer name prefix"
2. 替换虚拟机上管理员帐户的 **$adminName** 名称的值，然后创建变量：
   
        $adminName = "administrator account name"
3. 将 **$adminPassword** 的值替换为帐户密码，然后创建变量：
   
        $adminPassword = "password for administrator accounts"
4. 创建操作系统配置文件：
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>存储配置文件
1. 将 **$storageProfile** 的值替换为存储配置文件要使用的名称，然后创建变量：  
   
        $storageProfile = "storage profile name"
2. 创建定义要使用的映像的变量：  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    若要查找其他要使用的映像的相关信息，请参阅[使用 Windows PowerShell 和 Azure CLI 来导航和选择 Azure 虚拟机映像](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)。

3. 创建存储帐户配置文件：
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>虚拟机规模集
最后，可以创建规模集。

1. 将 **$vmssName** 的值替换为虚拟机规模集的名称，然后创建变量：
   
        $vmssName = "scale set name"
2. 创建规模集：
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    用户应看到与此示例类似的内容，这则表示部署成功：
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>步骤 3：浏览资源
使用这些资源来浏览已创建的虚拟机规模集：

* Azure 门户 - 使用门户可以获取有限数量的信息。
* [Azure 资源浏览器](https://resources.azure.com/) - 要浏览规模集的当前状态，这是最好的工具。
* Azure PowerShell - 使用此命令可获取信息：
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>后续步骤
* 使用[在虚拟机规模集中管理虚拟机](virtual-machine-scale-sets-windows-manage.md)中的信息管理刚刚创建的规模集
* 请考虑使用[自动缩放和虚拟机规模集](virtual-machine-scale-sets-autoscale-overview.md)中的信息设置规模集的自动缩放
* 若要了解有关垂直缩放的详细信息，请参阅[虚拟机规模集垂直自动缩放](virtual-machine-scale-sets-vertical-scale-reprovision.md)


