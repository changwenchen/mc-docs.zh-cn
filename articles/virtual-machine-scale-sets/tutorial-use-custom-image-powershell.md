---
title: 教程 - 通过 Azure PowerShell 使用规模集中的自定义 VM 映像
description: 了解如何使用 Azure PowerShell 来创建可用于部署虚拟机规模集的自定义 VM 映像
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: v-junlch
ms.custom: mvc
ms.openlocfilehash: 84417eebabce7c6dd3710bf0e761d54fade6f494
ms.sourcegitcommit: b80d236ce3c706abc25bbaa41b0ccddd896e48fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873145"
---
# <a name="tutorial-create-and-use-a-custom-image-for-virtual-machine-scale-sets-with-azure-powershell"></a>教程：通过 Azure PowerShell 创建和使用虚拟机规模集的自定义映像

创建规模集时，需指定部署 VM 实例时要使用的映像。 若要在部署 VM 实例之后减少任务数目，可以使用自定义 VM 映像。 在此自定义 VM 映像中可以完成全部所需的应用程序安装或配置步骤。 在规模集中创建的任何 VM 实例使用自定义 VM 映像，并随时可为应用程序流量提供服务。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 创建和自定义 VM
> * 取消预配和通用化 VM
> * 从源 VM 创建自定义 VM 映像
> * 部署使用自定义 VM 映像的规模集

如果没有 Azure 订阅，可在开始前创建一个[试用帐户](https://www.azure.cn/pricing/1rmb-trial/?WT.mc_id=A261C142F)。

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]


## <a name="create-and-configure-a-source-vm"></a>创建并配置源 VM

>[!NOTE]
> 本教程介绍创建和使用通用化 VM 映像的过程。 不支持从专用 VHD 创建规模集。

首先使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 创建资源组，然后使用 [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) 创建 VM。 此 VM 可用作自定义 VM 映像的源。 以下示例在名为 *myResourceGroup* 的资源组中创建名为 *myCustomVM* 的 VM。 系统提示时，请输入用作 VM 登录凭据的用户名和密码：

```azurepowershell
# Create a resource a group
New-AzResourceGroup -Name "myResourceGroup" -Location "ChinaNorth"

# Create a Windows Server 2016 Datacenter VM
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Name "myCustomVM" `
  -ImageName "Win2016Datacenter" `
  -OpenPorts 3389
```

若要连接到 VM，请使用 [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) 列出公共 IP 地址，如下所示：

```azurepowershell
Get-AzPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

创建与 VM 的远程连接。 提供你自己在前一命令中使用的 IP 地址。 出现提示时，请输入在第一步创建 VM 时使用的凭据：

```powershell
mstsc /v:<IpAddress>
```

为了自定义 VM，让我们安装一个基本的 Web 服务器。 部署规模集中的 VM 实例时，该实例会包含运行 Web 应用程序的全部所需包。 在 VM 上打开本地 PowerShell 提示符，然后使用 [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature) 安装 IIS Web 服务器，如下所示：

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

准备将 VM 用作自定义映像的最后一步是使 VM 通用化。 Sysprep 会删除所有个人帐户信息和配置，并将 VM 重置为干净状态，以便将来进行部署。 有关详细信息，请参阅[如何使用 Sysprep：简介](https://technet.microsoft.com/library/bb457073.aspx)。

若要使 VM 通用化，请运行 Sysprep 并针对开箱即用体验来设置 VM。 完成后，指示 Sysprep 关闭 VM：

```powershell
C:\Windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
```

当 Sysprep 完成相关过程并关闭 VM 后，到 VM 的远程连接会自动关闭。


## <a name="create-a-custom-vm-image-from-the-source-vm"></a>从源 VM 创建自定义 VM 映像
现已自定义源 VM，其上已安装 IIS Web 服务器。 让我们创建与规模集配合使用的自定义 VM 映像。

若要创建映像，需要解除分配 VM。 通过 [Stop-AzVm](https://docs.microsoft.com/powershell/module/az.compute/stop-azvm) 解除分配 VM。 然后，使用 [Set-AzVm](https://docs.microsoft.com/powershell/module/az.compute/set-azvm) 将 VM 的状态设置为已通用化，以便 Azure 平台知道该 VM 可使用自定义映像。 只能从通用化 VM 创建映像：

```azurepowershell
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Force
Set-AzVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Generalized
```

解除分配和通用化 VM 可能需要花费几分钟时间。

现在，请使用 [New-AzImageConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azimageconfig) 和 [New-AzImage](https://docs.microsoft.com/powershell/module/az.compute/new-azimage) 来创建 VM 的映像。 以下示例从 VM 创建名为 *myImage* 的映像：

```azurepowershell
# Get VM object
$vm = Get-AzVM -Name "myCustomVM" -ResourceGroupName "myResourceGroup"

# Create the VM image configuration based on the source VM
$image = New-AzImageConfig -Location "ChinaNorth" -SourceVirtualMachineId $vm.ID 

# Create the custom VM image
New-AzImage -Image $image -ImageName "myImage" -ResourceGroupName "myResourceGroup"
```

## <a name="configure-the-network-security-group-rules"></a>配置网络安全组规则
在创建规模集之前，需要配置关联的网络安全组规则，以允许访问 HTTP、RDP 和远程处理 

```azurepowershell
$rule1 = New-AzNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389

$rule3 = New-AzNetworkSecurityRuleConfig -Name remoting-rule -Description "Allow PS Remoting" -Access Allow -Protocol Tcp -Direction Inbound -Priority 120 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 5985

New-AzNetworkSecurityGroup -Name "myNSG" -ResourceGroupName "myResourceGroup" -Location "ChinaNorth" -SecurityRules $rule1,$rule2,$rule3
```

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>从自定义 VM 映像创建规模集
现在，请使用 [New-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) 来创建规模集，前者使用 `-ImageName` 参数来定义在上一步创建的自定义 VM 映像。 若要将流量分配到单独的 VM 实例，则还要创建负载均衡器。 负载均衡器包含的规则可在 TCP 端口 80 上分配流量，并允许 TCP 端口 3389 上的远程桌面流量，以及 TCP 端口 5985 上的 PowerShell 远程流量。 出现提示时，请针对规模集中的 VM 实例提供自己的所需管理凭据：

```azurepowershell
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "ChinaNorth" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNSG"
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ImageName "myImage"
```

创建和配置所有的规模集资源和 VM 需要几分钟时间。


## <a name="test-your-scale-set"></a>测试规模集
若要查看运行中的规模集，请使用 [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/Get-AzPublicIpAddress) 获取负载均衡器的公共 IP 地址，如下所示：


```azurepowershell
Get-AzPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Name "myPublicIPAddress" | Select IpAddress
```

在 Web 浏览器中键入公共 IP 地址。 随后会显示默认的 IIS 网页，如以下示例所示：

![从自定义 VM 映像运行的 IIS](./media/tutorial-use-custom-image-powershell/default-iis-website.png)


## <a name="clean-up-resources"></a>清理资源
若要删除规模集和其他资源，请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 删除资源组及其所有资源。 `-Force` 参数将确认是否希望删除资源，而不会有额外提示。 `-AsJob` 参数会使光标返回提示符处，无需等待操作完成。

```azurepowershell
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>后续步骤
本教程介绍了如何通过 Azure PowerShell 创建和使用规模集的自定义 VM 映像：

> [!div class="checklist"]
> * 创建和自定义 VM
> * 取消预配和通用化 VM
> * 创建自定义 VM 映像
> * 部署使用自定义 VM 映像的规模集

请继续学习下一教程，了解如何将应用程序部署到规模集。

> [!div class="nextstepaction"]
> [将应用程序部署到规模集](tutorial-install-apps-powershell.md)

<!-- Update_Description: update metedata properties -->