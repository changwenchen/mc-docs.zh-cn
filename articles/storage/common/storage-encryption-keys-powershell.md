---
title: 使用 PowerShell 配置客户管理的密钥
titleSuffix: Azure Storage
description: 了解如何使用 PowerShell 来配置客户管理的密钥用于 Azure 存储加密。 使用客户管理的密钥可以创建、轮换、禁用和撤销访问控制。
services: storage
author: WenJason
ms.service: storage
ms.topic: how-to
origin.date: 03/10/2020
ms.date: 03/30/2020
ms.author: v-jay
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 78df450086bb99af07b4ec273ef48e2f641b4dbe
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80290389"
---
# <a name="configure-customer-managed-keys-with-azure-key-vault-by-using-powershell"></a>通过 PowerShell 使用 Azure Key Vault 配置客户管理的密钥

[!INCLUDE [storage-encryption-configure-keys-include](../../../includes/storage-encryption-configure-keys-include.md)]

本文介绍如何使用 PowerShell 配置包含客户管理的密钥的 Azure Key Vault。 要了解如何使用 Azure CLI 创建密钥保管库，请参阅[快速入门：使用 PowerShell 在 Azure Key Vault 中设置和检索机密](../../key-vault/quick-create-powershell.md)。

## <a name="assign-an-identity-to-the-storage-account"></a>将标识分配到存储帐户

若要为存储帐户启用客户管理的密钥，请先将一个系统分配的托管标识分配到该存储帐户。 将使用此托管标识授予存储帐户访问 Key Vault 的权限。

若要使用 PowerShell 分配托管标识，请调用 [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)。 请记得将括号中的占位符值替换为你自己的值。

```powershell
$storageAccount = Set-AzStorageAccount -ResourceGroupName <resource_group> `
    -Name <storage-account> `
    -AssignIdentity
```

有关使用 PowerShell 配置系统分配的托管标识的详细信息，请参阅[使用 PowerShell 在 Azure VM 上配置 Azure 资源的托管标识](../../active-directory/managed-identities-azure-resources/qs-configure-powershell-windows-vm.md)。

## <a name="create-a-new-key-vault"></a>创建新的 Key Vault

若要使用 PowerShell 创建新的 Key Vault，请调用 [New-AzKeyVault](https://docs.microsoft.com/powershell/module/az.keyvault/new-azkeyvault)。 必须为用来存储客户管理的密钥（用于 Azure 存储加密）的 Key Vault 启用两项密钥保护设置：“软删除”和“不要清除”。   

请记得将括号中的占位符值替换为你自己的值。

```powershell
$keyVault = New-AzKeyVault -Name <key-vault> `
    -ResourceGroupName <resource_group> `
    -Location <location> `
    -EnableSoftDelete `
    -EnablePurgeProtection
```

若要了解如何使用 PowerShell 在现有密钥保管库上启用“软删除”  和“请勿清除”  ，请参阅[如何在 PowerShell 中使用软删除](../../key-vault/key-vault-soft-delete-powershell.md)中标题为“启用软删除”  和“启用清除保护”  的部分。

## <a name="configure-the-key-vault-access-policy"></a>配置 Key Vault 访问策略

接下来，配置 Key Vault 的访问策略，使存储帐户有权访问 Key Vault。 此步骤使用前面分配给存储帐户的托管标识。

若要设置 Key Vault 的访问策略，请调用 [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy)。 请记得将括号中的占位符值替换为自己的值，并使用前面示例中定义的变量。

```powershell
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get,recover
```

## <a name="create-a-new-key"></a>新建密钥

接下来，在 Key Vault 中创建新密钥。 若要创建新密钥，请调用 [Add-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/add-azkeyvaultkey)。 请记得将括号中的占位符值替换为自己的值，并使用前面示例中定义的变量。

```powershell
$key = Add-AzKeyVaultKey -VaultName $keyVault.VaultName -Name <key> -Destination 'Software'
```

## <a name="configure-encryption-with-customer-managed-keys"></a>配置使用客户管理的密钥进行加密

Azure 存储加密默认使用 Microsoft 托管的密钥。 此步骤将 Azure 存储帐户配置为使用客户管理的密钥，并指定要与存储帐户关联的密钥。

调用 [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) 以更新存储帐户的加密设置，如以下示例所示。 包括 **-KeyvaultEncryption** 选项，以便为存储帐户启用客户管理的密钥。 请记得将括号中的占位符值替换为自己的值，并使用前面示例中定义的变量。

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -KeyvaultEncryption `
    -KeyName $key.Name `
    -KeyVersion $key.Version `
    -KeyVaultUri $keyVault.VaultUri
```

## <a name="update-the-key-version"></a>更新密钥版本

创建密钥的新版本时，需将存储帐户更新为使用新版本。 首先调用 [Get-AzKeyVaultKey](https://docs.microsoft.com/powershell/module/az.keyvault/get-azkeyvaultkey) 以获取最新密钥版本。 然后调用 [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) 更新存储帐户的加密设置，以使用新的密钥版本，如上一部分中所示。

## <a name="use-a-different-key"></a>使用其他密钥

若要更改用于 Azure 存储加密的密钥，请调用 [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)（如[使用客户管理的密钥配置加密](#configure-encryption-with-customer-managed-keys)中所示），并提供新的密钥名称和版本。 如果新密钥位于不同的密钥保管库中，还需要更新密钥保管库 URI。

## <a name="revoke-customer-managed-keys"></a>撤销客户托管密钥

如果你认为密钥可能已泄露，则可以通过删除密钥保管库访问策略来撤销客户托管密钥。 若要撤销客户托管密钥，请调用 [Remove-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/remove-azkeyvaultaccesspolicy) 命令，如下例所示。 请记得将括号中的占位符值替换为自己的值，并使用前面示例中定义的变量。

```powershell
Remove-AzKeyVaultAccessPolicy -VaultName $keyVault.VaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
```

## <a name="disable-customer-managed-keys"></a>禁用客户托管密钥

禁用客户托管密钥时，将再次使用 Microsoft 托管密钥对存储帐户进行加密。 若要禁用客户管理的密钥，请使用 `-StorageEncryption` 选项调用 [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount)，如以下示例所示。 请记得将括号中的占位符值替换为自己的值，并使用前面示例中定义的变量。

```powershell
Set-AzStorageAccount -ResourceGroupName $storageAccount.ResourceGroupName `
    -AccountName $storageAccount.StorageAccountName `
    -StorageEncryption  
```

## <a name="next-steps"></a>后续步骤

- [静态数据的 Azure 存储加密](storage-service-encryption.md)
- [什么是 Azure Key Vault？](/key-vault/key-vault-overview)
