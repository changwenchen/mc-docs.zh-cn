---
title: 创建高级 Azure 文件共享
description: 本文介绍如何创建高级 Azure 文件共享。
author: WenJason
ms.service: storage
ms.topic: conceptual
origin.date: 05/05/2019
ms.date: 03/30/2020
ms.author: v-jay
ms.subservice: files
ms.openlocfilehash: 323776756fd34ae711873d41d2ed320c14e80d61
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80290460"
---
# <a name="how-to-create-an-premium-azure-file-share"></a>如何创建高级 Azure 文件共享
高级文件共享在固态磁盘 (SSD) 存储介质上提供，可用于 IO 密集型工作负荷，包括宿主数据库和高性能计算 (HPC)。 高级文件共享托管在特殊用途的存储帐户类型（称为 FileStorage 帐户）中。 高级文件共享旨在用于高性能和企业级应用程序，可稳定提供低延迟、高 IOPS 和高吞吐量的共享。

本文介绍如何使用 [Azure 门户](https://portal.azure.cn/)、Azure PowerShell 和 Azure CLI 创建此新帐户类型。

## <a name="prerequisites"></a>必备条件

若要访问包含高级 Azure 文件共享的 Azure 资源，需要一个 Azure 订阅。 如果还没有订阅，则在开始前创建一个 [1 元人民币的试用帐户](https://www.azure.cn/pricing/1rmb-trial-full/?form-type=identityauth)。

## <a name="create-a-premium-file-share-using-the-azure-portal"></a>使用 Azure 门户创建高级文件共享

### <a name="sign-in-to-azure"></a>登录 Azure

登录 [Azure 门户](https://portal.azure.cn/)。

### <a name="create-a-filestorage-storage-account"></a>创建 filestorage 存储帐户

现在可以创建存储帐户。

每个存储帐户都必须属于 Azure 资源组。 资源组是对 Azure 资源进行分组的逻辑容器。 在创建存储帐户时，可以选择创建新的资源组，也可以使用现有资源组。 本文介绍如何创建新资源组。

1. 在 Azure 门户中，选择左侧菜单中的“存储帐户”。 

    ![Azure 门户主页 - 选择存储帐户](media/storage-how-to-create-premium-fileshare/azure-portal-storage-accounts.png)

1. 在显示的“存储帐户”窗口中，选择“添加”。  
1. 选择要在其中创建存储帐户的订阅。
1. 在“资源组”  字段下，选择“新建”  。 输入新资源组的名称，如下图中所示。

1. 然后，输入存储帐户的名称。 所选名称在 Azure 中必须唯一。 该名称还必须为 3 到 24 个字符，并且只能包含数字和小写字母。
1. 选择存储帐户的位置或使用默认位置。
1. 对于“性能”，请选择“高级”。  
1. 依次选择“帐户类型”、“FileStorage”。  
1. 将“复制”保留设置为默认值“本地冗余存储(LRS)”。  

    ![如何创建高级文件共享的存储帐户](media/storage-how-to-create-premium-fileshare/create-filestorage-account.png)

1. 选择“查看+创建”  可查看存储帐户设置并创建帐户。
1. 选择“创建”  。

创建存储帐户资源后，请导航到该资源。

### <a name="create-a-premium-file-share"></a>创建高级文件共享

1. 在存储帐户的左侧菜单中滚动到“文件服务”部分，然后选择“文件”。  
1. 选择“文件共享”以创建高级文件共享。 
1. 输入文件共享的名称和所需配额，然后选择“创建”。 

> [!NOTE]
> 预配的共享大小按共享配额指定，文件共享按预配大小计费。有关更多详细信息，请参阅[定价页](https://azure.cn/pricing/details/storage/files/)。

   ![创建高级文件共享](media/storage-how-to-create-premium-fileshare/create-premium-file-share.png)

### <a name="clean-up-resources"></a>清理资源

若要清理本文创建的资源，可以直接删除资源组。 删除资源组也会删除相关联的存储帐户，以及与资源组相关联的任何其他资源。

## <a name="create-a-premium-file-share-using-powershell"></a>使用 PowerShell 创建高级文件共享

### <a name="create-an-account-using-powershell"></a>使用 PowerShell 创建帐户

首先，安装最新版本的 [PowerShellGet](https://docs.microsoft.com/powershell/scripting/gallery/installing-psget) 模块。

然后，升级 PowerShell 模块，登录到 Azure 订阅，创建资源组，然后创建存储帐户。

### <a name="upgrade-your-powershell-module"></a>升级 PowerShell 模块

若要在 PowerShell 中与高级文件共享交互，需要安装 Az.Storage 模块版本1.4.0，或最新的 Az.Storage 模块。

首先使用提升的权限打开 PowerShell 会话。

安装 Az.Storage 模块：

```powershell
Install-Module Az.Storage -Repository PSGallery -AllowClobber -Force
```

### <a name="sign-in-to-your-azure-subscription"></a>登录到 Azure 订阅

使用 `Connect-AzAccount` 命令并按照屏幕上的说明进行身份验证。

```powershell
Connect-AzAccount -Environment AzureChinaCloud
```

### <a name="create-a-resource-group"></a>创建资源组

若要通过 PowerShell 创建新的资源组，请使用 [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) 命令：

```powershell
# put resource group in a variable so you can use the same group name going forward,
# without hardcoding it repeatedly
$resourceGroup = "storage-how-to-resource-group"
$location = "chinaeast2"
New-AzResourceGroup -Name $resourceGroup -Location $location
```

### <a name="create-a-filestorage-storage-account"></a>创建 FileStorage 存储帐户

若要通过 PowerShell 创建 filestorage 存储帐户，请使用 [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount) 命令：

```powershell
$storageAcct = New-AzStorageAccount -ResourceGroupName $resourceGroup -Name "fileshowto" -SkuName "Premium_LRS" -Location "chinaeast2" -Kind "FileStorage"
```

### <a name="create-a-premium-file-share"></a>创建高级文件共享

创建 FileStorage 帐户后，接下来可以创建高级文件共享。 可使用 [New-AzStorageShare](https://docs.microsoft.com/powershell/module/az.storage/New-AzStorageShare) cmdlet 来创建。

> [!NOTE]
> 预配的共享大小按共享配额指定，文件共享按预配大小计费。有关更多详细信息，请参阅[定价页](https://azure.cn/pricing/details/storage/files/)。

```powershell
New-AzStorageShare `
   -Name myshare `
   -Context $storageAcct.Context
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) 命令： 

```powershell
Remove-AzResourceGroup -Name $resourceGroup
```

## <a name="create-a-premium-file-share-using-azure-cli"></a>使用 Azure CLI 创建高级文件共享

登录到 CLI 的本地安装，首先确保安装了最新版本，然后运行 login 命令：

```cli
az login
```

### <a name="create-a-resource-group"></a>创建资源组

若要通过 Azure CLI 创建新的资源组，请使用 [az group create](/cli/group) 命令。

```azurecli
az group create `
    --name files-howto-resource-group `
    --location chinaeast2
```

### <a name="create-a-filestorage-storage-account"></a>创建 FileStorage 存储帐户

若要通过 Azure CLI 创建 FileStorage 存储帐户，请使用 [az storage account create](/cli/storage/account) 命令。

```azurecli
az storage account create `
    --name fileshowto `
    --resource-group files-howto-resource-group `
    --location chinaeast2 `
    --sku Premium_LRS `
    --kind FileStorage
```

### <a name="get-the-storage-account-key"></a>获取存储帐户密钥

存储帐户密钥控制对存储帐户中的资源的访问，本文使用密钥来创建高级文件共享。 这些密钥是在创建存储帐户时自动创建的。 可以使用 [az storage account keys list](/cli/storage/account/keys) 命令获取存储帐户的存储帐户密钥：

```azurecli 
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

### <a name="create-a-premium-file-share"></a>创建高级文件共享

创建 filestorage 帐户后，接下来可以创建高级文件共享。 可使用 [az storage share create](/cli/storage/share) 命令来创建。

> [!NOTE]
> 预配的共享大小按共享配额指定，文件共享按预配大小计费。有关更多详细信息，请参阅[定价页](https://azure.cn/pricing/details/storage/files/)。

```azurecli
az storage share create \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY \
    --name "myshare" 
```

### <a name="clean-up-resources"></a>清理资源

若要删除资源组及其关联的资源（包括新的存储帐户），请使用 [az group delete](/cli/group) 命令。

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>后续步骤

在本文中，你已创建一个高级文件共享。 若要了解此帐户提供的性能，请转到规划指南的“性能层”部分。

> [!div class="nextstepaction"]
> [文件共享层](storage-files-planning.md#storage-tiers)