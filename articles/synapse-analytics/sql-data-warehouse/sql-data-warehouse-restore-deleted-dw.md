---
title: 还原已删除的 SQL 池
description: 有关还原已删除的 SQL 池的指南。
services: synapse-analytics
author: WenJason
manager: digimobile
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
origin.date: 08/29/2018
ms.date: 05/11/2020
ms.author: v-jay
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 84500634d053cded3dcb516488a0b90f9c0a4904
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198760"
---
# <a name="restore-a-deleted-sql-pool-using-azure-synapse-analytics"></a>使用 Azure Synapse Analytics 还原已删除的 SQL 池

本文介绍了如何使用 Azure 门户或 PowerShell 还原 SQL。

## <a name="before-you-begin"></a>准备阶段

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

**验证 DTU 容量。** 每个 SQL 池都由一个具有默认 DTU 配额的 SQL 服务器（例如 myserver.database.chinacloudapi.cn）托管。  验证 SQL Server 的剩余 DTU 配额是否足够进行数据库还原。

## <a name="restore-a-deleted-data-warehouse-through-powershell"></a>通过 PowerShell 还原已删除的数据仓库

若要还原已删除的 SQL 池，请使用 [Restore-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) cmdlet。 如果相应的逻辑服务器也被删除，则不能还原该数据仓库。

1. 开始之前，请确保[安装 Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json)。
2. 打开 PowerShell。
3. 连接到 Azure 帐户，并列出与帐户关联的所有订阅。
4. 选择包含要还原的已删除数据仓库的订阅。
5. 获取特定的已删除数据仓库。
6. 还原已删除的数据仓库
    * 若要将已删除的 SQL 数据仓库还原到另一逻辑服务器，请确保指定另一逻辑服务器名称。  该逻辑服务器也可以位于另一资源组和区域中。
7. 验证已还原的数据仓库是否处于联机状态。
8. 完成还原后，可以按[在恢复后配置数据库](../../sql-database/sql-database-disaster-recovery.md?toc=/synapse-analytics/sql-data-warehouse/toc.json&bc=/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json#configure-your-database-after-recovery)中的说明配置恢复后的数据仓库。

```Powershell
$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.chinacloudapi.cn
#$TargetResourceGroupName="<YourTargetResourceGroupName>" # uncomment to restore to a different logical server.
#$TargetServerName="<YourtargetServerNameWithoutURLSuffixSeeNote>"
$DatabaseName="<YourDatabaseName>"
$NewDatabaseName="<YourDatabaseName>"

Connect-AzAccount -Environment AzureChinaCloud
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Get the deleted database to restore
$DeletedDatabase = Get-AzSqlDeletedDatabaseBackup -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName

# Restore deleted database
$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $DeletedDatabase.ResourceGroupName -ServerName $DeletedDatabase.ServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Use the following command to restore deleted data warehouse to a different logical server
#$RestoredDatabase = Restore-AzSqlDatabase –FromDeletedDatabaseBackup –DeletionDate $DeletedDatabase.DeletionDate -ResourceGroupName $TargetResourceGroupName -ServerName $TargetServerName -TargetDatabaseName $NewDatabaseName –ResourceId $DeletedDatabase.ResourceID

# Verify the status of restored database
$RestoredDatabase.status
```

## <a name="restore-a-deleted-database-using-the-azure-portal"></a>通过 Azure 门户还原已删除的数据库

1. 登录到 [Azure 门户](https://portal.azure.cn/)。
2. 导航到承载着已删除数据仓库的 SQL Server。
3. 在目录中选择“已删除的数据库”图标。 

    ![已删除的数据库](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-01.png)

4. 选择要还原的已删除 SQL 数据仓库。

    ![选择“已删除的数据库”](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-11.png)

5. 指定新的**数据库名称**，并单击“确定” 

    ![指定数据库名称](./media/sql-data-warehouse-restore-deleted-dw/restoring-deleted-21.png)

## <a name="next-steps"></a>后续步骤

- [还原现有的 SQL 池](sql-data-warehouse-restore-active-paused-dw.md)
- [从异地备份 SQL 池进行还原](sql-data-warehouse-restore-from-geo-backup.md)