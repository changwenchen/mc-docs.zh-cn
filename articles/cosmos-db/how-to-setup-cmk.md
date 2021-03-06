---
title: 为 Azure Cosmos DB 帐户配置客户管理的密钥
description: 了解如何使用 Azure Key Vault 为 Azure Cosmos DB 帐户配置客户管理的密钥
author: rockboyfor
ms.service: cosmos-db
ms.topic: conceptual
origin.date: 03/19/2020
ms.date: 04/27/2020
ms.author: v-yeche
ROBOTS: noindex, nofollow
ms.openlocfilehash: f7179bd410bcb1e5defcdd0dbc82b8555a192d7c
ms.sourcegitcommit: f9c242ce5df12e1cd85471adae52530c4de4c7d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82134653"
---
# <a name="configure-customer-managed-keys-for-your-azure-cosmos-account-with-azure-key-vault"></a>使用 Azure Key Vault 为 Azure Cosmos 帐户配置客户管理的密钥

> [!NOTE]
> 目前，必须请求访问权限才能使用此功能。 为此，请联系 [azurecosmosdbcmk@service.microsoft.com](mailto:azurecosmosdbcmk@service.microsoft.com)。

存储在 Azure Cosmos 帐户中的数据会使用 Azure 托管密钥（服务托管密钥  ）自动无缝地进行加密。 还可以选择使用你自己托管的密钥（客户托管密钥  ）来添加另一个加密层。

![围绕客户数据的加密层](./media/how-to-setup-cmk/cmk-intro.png)

必须将客户托管密钥存储在 [Azure Key Vault](../key-vault/key-vault-overview.md) 中，并为每个启用了客户托管密钥的 Azure Cosmos 帐户提供一个密钥。 此密钥用于加密该帐户中存储的所有数据。

> [!NOTE]
> 当前，客户托管密钥仅适用于新的 Azure Cosmos 帐户。 应在创建帐户期间配置这些密钥。

<a name="register-resource-provider"></a>
## <a name="register-the-azure-cosmos-db-resource-provider-for-your-azure-subscription"></a>为 Azure 订阅注册 Azure Cosmos DB 资源提供程序

1. 登录到 [Azure 门户](https://portal.azure.cn/)，转到你的 Azure 订阅，然后在“设置”选项卡下选择“资源提供程序”：  

    ![左侧菜单中的“资源提供程序”项](./media/how-to-setup-cmk/portal-rp.png)

1. 搜索 **Microsoft.DocumentDB** 资源提供程序。 确认该资源提供程序是否标记为已注册。 如果不是，请选择该资源提供程序，然后选择“注册”： 

    ![注册 Microsoft.DocumentDB 资源提供程序](./media/how-to-setup-cmk/portal-rp-register.png)

## <a name="configure-your-azure-key-vault-instance"></a>配置 Azure Key Vault 实例

对 Azure Cosmos DB 使用客户管理的密钥需要在你打算用来托管加密密钥的 Azure Key Vault 实例上设置两个属性。 这些属性包括“软删除”和“不清除”。   默认情况下未启用这些属性。 可以使用 PowerShell 或 Azure CLI 来启用它们。

若要了解如何在现有 Azure Key Vault 实例上启用这些属性，请参阅以下文章之一中的“启用软删除”和“启用清除保护”部分：

- [如何通过 PowerShell 使用软删除](../key-vault/key-vault-soft-delete-powershell.md)
- [如何通过 Azure CLI 使用软删除](../key-vault/key-vault-soft-delete-cli.md)

## <a name="add-an-access-policy-to-your-azure-key-vault-instance"></a>将访问策略添加到 Azure Key Vault 实例

1. 在 Azure 门户中，转到你打算用来托管加密密钥的 Azure Key Vault 实例。 在左侧菜单中选择“访问策略”： 

    ![左侧菜单中的“访问策略”](./media/how-to-setup-cmk/portal-akv-ap.png)

1. 选择“+ 添加访问策略”  。

1. 在“密钥权限”下拉菜单中，选择“获取”、“解包密钥”和“包装密钥”权限：    

    ![选择适当的权限](./media/how-to-setup-cmk/portal-akv-add-ap-perm2.png)

1. 在“选择主体”下，选择“未选择任何项”。   然后，搜索“Azure Cosmos DB”  主体并将其选中（为了便于查找，对于其中的主体 ID 为 `57506a73-e302-42a9-b869-6f12d9ec29e9` 的任何 Azure 中国区域，还可以按主体 ID `a232010e-820c-4083-83bb-3ace5fc29d0b` 进行搜索）。 最后，选择底部的“选择”  。 如果列表中没有“Azure Cosmos DB”  主体，可能需要根据本文的[注册资源提供程序](#register-resource-provider)部分所述，重新注册 Microsoft.DocumentDB  资源提供程序）。

    <!--MOONCAKE: CORRECT ON Azure China regions,  REMOVE except US goverment regions-->
    
    ![选择 Azure Cosmos DB 主体](./media/how-to-setup-cmk/portal-akv-add-ap.png)

1. 选择“添加”以添加新的访问策略  。

## <a name="generate-a-key-in-azure-key-vault"></a>在 Azure Key Vault 中生成密钥

1. 在 Azure 门户中，转到你打算用来托管加密密钥的 Azure Key Vault 实例。 然后在左侧菜单中选择“密钥”： 

    ![左侧菜单中的“密钥”项](./media/how-to-setup-cmk/portal-akv-keys.png)

1. 选择“生成/导入”  ，为新密钥提供名称，并选择一个 RSA 密钥大小。 建议至少使用 3072，以获得最佳安全性。 然后选择“创建”  ：

   ![新建密钥](./media/how-to-setup-cmk/portal-akv-gen.png)

1. 创建密钥后，选择新创建的密钥，然后选择其当前版本。

1. 复制密钥的“密钥标识符”  （但不要复制最后一个正斜杠后面的部分）：

    ![复制密钥的密钥标识符](./media/how-to-setup-cmk/portal-akv-keyid.png)

## <a name="create-a-new-azure-cosmos-account"></a>创建新的 Azure Cosmos 帐户

<!--Not Available on #### Using the Azure portal-->

### <a name="using-azure-powershell"></a>使用 Azure PowerShell

使用 PowerShell 创建新的 Azure Cosmos DB 帐户时：

- 请在 **PropertyObject** 中的 **keyVaultKeyUri** 属性下传递前面复制的 Azure Key Vault 密钥 URI。

- 使用 **2019-12-12** 作为 API 版本。

> [!IMPORTANT]
> 必须显式设置 `Location` 参数，才能使用客户管理的密钥成功创建帐户。

```powershell
$resourceGroupName = "myResourceGroup"
$accountLocation = "China North 2"
$accountName = "mycosmosaccount"

$failoverLocations = @(
    @{ "locationName"="China North 2"; "failoverPriority"=0 }
)

$CosmosDBProperties = @{
    "databaseAccountOfferType"="Standard";
    "locations"=$failoverLocations;
    "keyVaultKeyUri" = "https://<my-vault>.vault.azure.cn/keys/<my-key>";
}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
    -ApiVersion "2019-12-12" -ResourceGroupName $resourceGroupName `
    -Location $accountLocation -Name $accountName -PropertyObject $CosmosDBProperties
```

### <a name="using-an-azure-resource-manager-template"></a>使用 Azure 资源管理器模板

通过 Azure 资源管理器模板创建新的 Azure Cosmos 帐户时：

- 在 **properties** 对象中的 **keyVaultKeyUri** 属性下，传递前面复制的 Azure Key Vault 密钥 URI。

- 使用 **2019-12-12** 作为 API 版本。

> [!IMPORTANT]
> 必须显式设置 `Location` 参数，才能使用客户管理的密钥成功创建帐户。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "accountName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "keyVaultKeyUri": {
            "type": "string"
        }
    },
    "resources": 
    [
        {
            "type": "Microsoft.DocumentDB/databaseAccounts",
            "name": "[parameters('accountName')]",
            "apiVersion": "2019-12-12",
            "kind": "GlobalDocumentDB",
            "location": "[parameters('location')]",
            "properties": {
                "locations": [ 
                    {
                        "locationName": "[parameters('location')]",
                        "failoverPriority": 0,
                        "isZoneRedundant": false
                    }
                ],
                "databaseAccountOfferType": "Standard",
                "keyVaultKeyUri": "[parameters('keyVaultKeyUri')]"
            }
        }
    ]
}

```

使用以下 PowerShell 脚本部署模板：

```powershell
$resourceGroupName = "myResourceGroup"
$accountName = "mycosmosaccount"
$accountLocation = "China North 2"
$keyVaultKeyUri = "https://<my-vault>.vault.azure.cn/keys/<my-key>"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "deploy.json" `
    -accountName $accountName `
    -location $accountLocation `
    -keyVaultKeyUri $keyVaultKeyUri
```

### <a name="using-the-azure-cli"></a>使用 Azure CLI

通过 Azure CLI 创建新的 Azure Cosmos 帐户时，请在 --key-uri  参数下传递你前面复制的 Azure Key Vault 密钥的 URI。

```azurecli
resourceGroupName='myResourceGroup'
accountName='mycosmosaccount'
keyVaultKeyUri = 'https://<my-vault>.vault.azure.cn/keys/<my-key>'

az cosmosdb create \
    -n $accountName \
    -g $resourceGroupName \
    --locations regionName='China North 2' failoverPriority=0 isZoneRedundant=False \
    --key-uri $keyVaultKeyUri
```

## <a name="frequently-asked-questions"></a>常见问题

### <a name="is-there-any-additional-charge-for-using-customer-managed-keys"></a>使用客户托管密钥是否需要支付额外的费用？

是的。 由于使用客户托管密钥管理数据加密和解密需要增大计算负载，针对 Azure Cosmos 帐户执行的所有操作消耗的[请求单位](./request-units.md)数会增加 25%。

### <a name="what-data-gets-encrypted-with-the-customer-managed-keys"></a>哪些数据是使用客户管理的密钥加密的？

Azure Cosmos 帐户中存储的所有数据都将通过客户托管密钥加密，但以下元数据除外：

- Azure Cosmos DB [帐户、数据库和容器](./account-overview.md#elements-in-an-azure-cosmos-account)的名称

- [存储过程](./stored-procedures-triggers-udfs.md)的名称

- [索引策略](./index-policy.md)中声明的属性路径

- 容器的[分区键](./partitioning-overview.md)的值

### <a name="are-customer-managed-keys-supported-for-existing-azure-cosmos-accounts"></a>现有的 Azure Cosmos 帐户是否支持客户管理的密钥？

此功能目前仅适用于新帐户。

### <a name="is-there-a-plan-to-support-finer-granularity-than-account-level-keys"></a>你们是否有计划支持比帐户级密钥更精细的粒度？

目前无此计划，但我们正在考虑容器级密钥。

### <a name="how-do-customer-managed-keys-affect-a-backup"></a>客户托管密钥对备份造成怎样的影响？

Azure Cosmos DB [定期自动备份](./online-backup-and-restore.md)帐户中存储的数据。 此操作会备份已加密的数据。 若要使用还原的备份，需要提供备份时使用的加密密钥。 这意味着，没有执行任何吊销操作，并且备份时使用的密钥版本仍将保持启用状态。

### <a name="how-do-i-revoke-an-encryption-key"></a>如何吊销加密密钥？

可以通过禁用密钥的最新版本来吊销密钥：

![禁用密钥的版本](./media/how-to-setup-cmk/portal-akv-rev2.png)

或者，若要从 Azure Key Vault 实例中吊销所有密钥，可以删除授予 Azure Cosmos DB 主体的访问策略：

![删除 Azure Cosmos DB 主体的访问策略](./media/how-to-setup-cmk/portal-akv-rev.png)

### <a name="what-operations-are-available-after-a-customer-managed-key-is-revoked"></a>吊销客户管理的密钥后可执行哪些操作？

吊销加密密钥后，删除帐户是唯一可执行的操作。

## <a name="next-steps"></a>后续步骤

- 详细了解 [Azure Cosmos DB 中的数据加密](./database-encryption-at-rest.md)。
- 大概了解如何[安全访问 Cosmos DB 中的数据](secure-access-to-data.md)。

<!-- Update_Description: update meta properties, wording update, update link -->