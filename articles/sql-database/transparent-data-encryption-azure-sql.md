---
title: 透明数据加密
description: Azure Synapse Analytics 中的 SQL 数据库和 Synapse SQL 的透明数据加密概述。 本文档介绍服务托管的透明数据加密和“创建自己的密钥”的优势与配置选项。
services: sql-database
ms.service: sql-database
ms.subservice: security
titleSuffix: Azure SQL Database and Azure Synapse
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: vanto
origin.date: 04/10/2020
ms.date: 04/27/2020
ms.openlocfilehash: aedc4f08aef9e83ba7389b32c5ea1132e76c3573
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126203"
---
# <a name="transparent-data-encryption-for-sql-database-and-azure-synapse"></a>SQL 数据库和 Azure Synapse 的透明数据加密

[透明数据加密 (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption) 通过加密静态数据，帮助保护 Azure Synapse Analytics 中的 Azure SQL 数据库、Azure SQL 托管实例和 Synapse SQL 免受恶意脱机活动的威胁。 它可执行静态数据库、关联备份和事务日志文件的实时加密和解密，无需更改应用程序。 对于所有新部署的 Azure SQL 数据库，默认已启用 TDE；对于 Azure SQL 数据库、Azure SQL 托管实例或 Azure Synapse 的旧版数据库，需要手动启用 TDE。

TDE 对页面级数据执行实时 I/O 加密和解密。 将每个页面读入内存时会将其解密，在写入磁盘之前会将其加密。 TDE 使用称为数据库加密密钥 (DEK) 的对称密钥加密整个数据库的存储。 在数据库启动时，已加密的 DEK 被解密，然后用于解密和重新加密 SQL Server 数据库引擎进程中的数据库文件。 DEK 由 TDE 保护器保护。 TDE 保护器是服务托管的证书（服务托管的透明数据加密）或存储在 [Azure Key Vault](/key-vault/key-vault-secure-your-key-vault) 中的非对称密钥（客户管理的透明数据加密）。 

对于 Azure SQL 数据库和 Azure Synapse，TDE 保护器在逻辑 SQL 服务器级别设置，并由该服务器关联的所有数据库继承。 对于 Azure SQL 托管实例（预览版 BYOK 功能），TDE 保护器是在实例级别设置的，并由该实例上所有加密的数据库继承。 除非另有说明，否则术语“服务器”在整个文档中指的是服务器和实例  。

> [!IMPORTANT]
> 默认情况下，会使用服务托管的透明数据加密将所有新建的 Azure SQL 数据库加密。 默认情况下，2017 年 5 月之前创建的现有 SQL 数据库以及通过还原、异地复制和数据库副本创建的 SQL 数据库均不加密。 默认情况下，2019 年 2 月之前创建的现有托管实例数据库不加密。 通过源提供的还原继承加密状态创建的托管实例数据库。

> [!NOTE]
> 不能使用 TDE 来加密 SQL 数据库中的逻辑 **master** 数据库。  **master** 数据库包含对用户数据库执行 TDE 操作时所需的对象。


## <a name="service-managed-transparent-data-encryption"></a>服务托管的透明数据加密

在 Azure 中，TDE 的默认设置是 DEK 受内置服务器证书保护。 内置服务器证书对于每个服务器都是唯一的，使用的加密算法是 AES 256。 如果某个数据库存在异地复制关系，则主数据库和异地辅助数据库将受主数据库的父服务器密钥保护。 如果两个数据库连接到同一个服务器，则它们也共享相同的内置证书。 Azure 根据内部安全策略自动轮换这些证书，根密钥由 Azure 内部机密存储保护。 客户可以在 [Azure 信任中心](https://www.trustcenter.cn/)提供的独立第三方审核报告中验证 SQL 数据库是否符合内部安全策略。

Azure 还可按需无缝移动和管理密钥，以实现异地复制和还原。


## <a name="customer-managed-transparent-data-encryption---bring-your-own-key"></a>客户管理的透明数据加密 - 创建自己的密钥

客户管理的 TDE 也称为 TDE 的“创建自己的密钥”(BYOK) 支持。 在此方案中，用于加密 DEK 的 TDE 保护器是客户管理的非对称密钥，该密钥存储在客户自有且自行管理的 Azure Key Vault（Azure 的基于云的外部密钥管理系统）中，并且永远不会离开该密钥保管库。 TDE 保护器可[由密钥保管库生成](/key-vault/about-keys-secrets-and-certificates#key-vault-keys)。 需要向 SQL 数据库授予对客户管理的密钥保管库的权限才能对 DEK 进行解密和加密。 如果撤销了逻辑 SQL Server 对该密钥保管库的权限，则数据库将无法访问，并且所有数据都是加密的。

使用集成了 Azure Key Vault 的 TDE，用户可以控制密钥管理任务，包括密钥轮换、密钥保管库权限、密钥备份，以及使用 Azure Key Vault 功能对所有 TDE 保护器启用审核/报告。 Key Vault 提供了集中密钥管理功能，并可在密钥与数据管理之间实现职责分离，以帮助满足安全策略的符合性。
若要详细了解 Azure SQL 数据库和 Azure Synapse 的 BYOK，请参阅[透明数据加密与 Azure Key Vault 的集成](transparent-data-encryption-byok-azure-sql.md)。

若要开始使用 TDE 与 Azure Key Vault 的集成，请参阅操作指南[使用 Key Vault 中的自有密钥启用透明数据加密](transparent-data-encryption-byok-azure-sql-configure.md)。

## <a name="move-a-transparent-data-encryption-protected-database"></a>移动受透明数据加密保护的数据库

对于 Azure 中的操作，不需要解密数据库。 源数据库或主数据库上 TDE 的设置均以透明方式继承在目标系统上。 包括的操作涉及到：

- 异地还原
- 自助时间点还原
- 还原已删除的数据库
- 活动异地复制
- 创建数据库副本
- 将备份文件还原到 Azure SQL 托管实例

> [!IMPORTANT]
> 由于无法访问用于加密的证书，因此不允许在 Azure SQL 托管实例中对由服务托管的 TDE 加密的数据库手动执行仅复制备份。 使用时间点还原功能将此类型的数据库移到另一个托管实例。

在导出受 TDE 保护的数据库时，该数据库的导出内容不会加密。 此导出内容存储在未加密的 BACPAC 文件中。 完成新数据库的导入后，请务必适当保护 BACPAC 文件并启用 TDE。

例如，如果从本地 SQL Server 实例导出 BACPAC 文件，则新数据库的导入内容不会自动加密。 同样，如果将 BACPAC 文件导出到本地 SQL Server 实例，则新数据库也不会自动加密。

向/从 SQL 数据库导出时例外。 在新数据库中启用了 TDE，但 BACPAC 文件本身仍未加密。


## <a name="manage-transparent-data-encryption"></a>管理透明数据加密
# <a name="portal"></a>[Portal](#tab/azure-portal)
在 Azure 门户中管理 TDE。

若要通过 Azure 门户配置 TDE，必须以 Azure 所有者、参与者或 SQL 安全管理员的身份建立连接。

可以在数据库级别启用和禁用 TDE。 若要对数据库启用 TDE，请转到 [Azure 门户](https://portal.azure.cn)并使用 Azure 管理员或参与者帐户登录。 在用户数据库下查找 TDE 设置。 默认会使用服务托管的透明数据加密。 将为包含数据库的服务器自动生成 TDE 证书。  对于 Azure SQL 托管实例，请使用 T-SQL 对数据库启用和禁用 TDE。

![服务托管的透明数据加密](./media/transparent-data-encryption-azure-sql/service-managed-transparent-data-encryption.png)  

在服务器级别设置 TDE 主密钥（称为 TDE 保护器）。 若要使用支持 BYOK 的 TDE，并使用 Key Vault 中的密钥来保护数据库，请在你的服务器下打开 TDE 设置。

![支持“创建自己的密钥”的透明数据加密](./media/transparent-data-encryption-azure-sql/tde-byok-support.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
使用 PowerShell 管理 TDE。

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure 资源管理器模块仍受 Azure SQL 数据库的支持，但所有未来的开发都是针对 Az.Sql 模块的。 若要了解这些 cmdlet，请参阅 [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)。 Az 模块和 AzureRm 模块中的命令参数大体上是相同的。

若要通过 PowerShell 配置 TDE，用户必须作为 Azure 所有者、参与者或 SQL 安全管理员进行连接。

### <a name="cmdlets-for-azure-sql-database-and-azure-synapse"></a>用于 Azure SQL 数据库和 Azure Synapse 的 Cmdlet

对 Azure SQL 数据库和 Azure Synapse 使用以下 cmdlet：

| Cmdlet | 说明 |
| --- | --- |
| [Set-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) |为数据库启用或禁用透明数据加密|
| [Get-AzSqlDatabaseTransparentDataEncryption](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) |获取数据库的透明数据加密状态 |
| [Get-AzSqlDatabaseTransparentDataEncryptionActivity](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) |检查数据库的加密进度 |
| [Add-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/add-azsqlserverkeyvaultkey) |将 Key Vault 密钥添加到 SQL Server 实例 |
| [Get-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverkeyvaultkey) |获取 Azure SQL 数据库服务器的 Key Vault 密钥  |
| [Set-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) |设置 SQL Server 实例的透明数据加密保护器 |
| [Get-AzSqlServerTransparentDataEncryptionProtector](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) |获取透明数据加密保护器 |
| [Remove-AzSqlServerKeyVaultKey](https://docs.microsoft.com/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) |从 SQL Server 实例中删除 Key Vault 密钥 |
|  | |

> [!IMPORTANT]
> 对于 Azure SQL 托管实例，请使用 T-SQL [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) 命令在数据库级别启用和禁用 TDE，并查看[示例 PowerShell 脚本](transparent-data-encryption-byok-azure-sql-configure.md)以在实例级别管理 TDE。

# <a name="transact-sql"></a>[Transact-SQL](#tab/azure-TransactSQL)
使用 Transact-SQL 管理 TDE。

使用 master 数据库中充当管理员或 **dbmanager** 角色成员的登录名连接到数据库。

| 命令 | 说明 |
| --- | --- |
| [ALTER DATABASE（Azure SQL 数据库）](https://docs.microsoft.com/sql/t-sql/statements/alter-database-azure-sql-database) | SET ENCRYPTION ON/OFF 会加密或解密数据库 |
| [sys.dm_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql) |返回有关数据库的加密状态及其关联的数据库加密密钥的信息 |
| [sys.dm_pdw_nodes_database_encryption_keys](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-nodes-database-encryption-keys-transact-sql) |返回有关每个 Azure Synapse 节点的加密状态及其关联的数据库加密密钥的信息 |
|  | |

不能使用 Transact-SQL 将 TDE 保护器切换到 Key Vault 中的密钥。 请使用 PowerShell 或 Azure 门户。

# <a name="rest-api"></a>[REST API](#tab/azure-RESTAPI)
使用 REST API 管理 TDE。

若要通过 REST API 配置 TDE，必须以 Azure 所有者、参与者或 SQL 安全管理员的身份建立连接。
对 Azure SQL 数据库和 Azure Synapse 使用以下命令集：

| 命令 | 说明 |
| --- | --- |
|[创建或更新服务器](https://docs.microsoft.com/rest/api/sql/servers/createorupdate)|将 Azure Active Directory 标识添加到 SQL Server 实例（用于授予 Key Vault 的访问权限）|
|[创建或更新服务器密钥](https://docs.microsoft.com/rest/api/sql/serverkeys/createorupdate)|将 Key Vault 密钥添加到 SQL Server 实例|
|[删除服务器密钥](https://docs.microsoft.com/rest/api/sql/serverkeys/delete)|从 SQL Server 实例中删除 Key Vault 密钥|
|[获取服务器密钥](https://docs.microsoft.com/rest/api/sql/serverkeys/get)|从 SQL Server 实例中获取特定的 Key Vault 密钥|
|[按服务器列出服务器密钥](https://docs.microsoft.com/rest/api/sql/serverkeys/listbyserver)|获取 SQL Server 实例的 Key Vault 密钥 |
|[创建或更新加密保护器](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/createorupdate)|为 SQL Server 实例设置 TDE 保护器|
|[获取加密保护器](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/get)|获取 SQL Server 实例的 TDE 保护器|
|[按服务器列出加密保护器](https://docs.microsoft.com/rest/api/sql/encryptionprotectors/listbyserver)|获取 SQL Server 实例的 TDE 保护器 |
|[创建或更新透明数据加密配置](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/createorupdate)|对数据库启用或禁用 TDE|
|[获取透明数据加密配置](https://docs.microsoft.com/rest/api/sql/transparentdataencryptions/get)|获取数据库的 TDE 配置|
|[列出透明数据加密配置结果](https://docs.microsoft.com/rest/api/sql/transparentdataencryptionactivities/listbyconfiguration)|获取数据库的加密结果|

## <a name="see-also"></a>另请参阅
- 有关 TDE 的一般介绍，请参阅[透明数据加密](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption)。
- 若要详细了解 Azure SQL 数据库、Azure SQL 托管实例和 Azure Synapse 的支持 BYOK 的 TDE，请参阅[支持“创建自己的密钥”的透明数据加密](transparent-data-encryption-byok-azure-sql.md)。
- 若要开始使用支持“创建自己的密钥”的 TDE，请参阅操作指南[使用 Key Vault 中的自有密钥启用透明数据加密](transparent-data-encryption-byok-azure-sql-configure.md)。
- 有关 Key Vault 的详细信息，请参阅[保护对密钥保管库的访问](/key-vault/key-vault-secure-your-key-vault)。
