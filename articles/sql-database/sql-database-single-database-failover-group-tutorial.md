---
title: 教程：将单一数据库添加到故障转移组
description: 使用 Azure 门户、PowerShell 或 Azure CLI 将 Azure SQL 数据库单一数据库添加到故障转移组。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: WenJason
ms.author: v-jay
ms.reviewer: sstein, carlrab
origin.date: 06/19/2019
ms.date: 02/17/2020
ms.openlocfilehash: 3c37306153d6c7a3eadab3c6337556def92c444c
ms.sourcegitcommit: a4a2521da9b29714aa6b511fc6ba48279b5777c8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82126727"
---
# <a name="tutorial-add-an-azure-sql-database-single-database-to-a-failover-group"></a>教程：将 Azure SQL 数据库单一数据库添加到故障转移组

[故障转移组](sql-database-auto-failover-group.md)是一种声明性抽象层，可用于对多个异地复制数据库进行分组。 了解如何使用 Azure 门户、PowerShell 或 Azure CLI 为 Azure SQL 数据库单一数据库配置故障转移组并测试故障转移。  在本教程中，您将学习如何执行以下操作：

> [!div class="checklist"]
> - 创建 Azure SQL 数据库单一数据库。
> - 为单一数据库创建两个逻辑 SQL 服务器之间的故障转移组。
> - 测试故障转移。

## <a name="prerequisites"></a>先决条件

# <a name="portal"></a>[Portal](#tab/azure-portal)
若要完成本教程，请确保做好以下准备： 

- Azure 订阅。 如果你没有帐户，请[创建一个 1 元试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。


# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
若要完成本教程，请确保准备好以下各项：

- Azure 订阅。 如果你没有帐户，请[创建一个 1 元试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。
- [Azure PowerShell](https://docs.microsoft.com/powershell/azureps-cmdlets-docs)


# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
若要完成本教程，请确保准备好以下各项：

- Azure 订阅。 如果你没有帐户，请[创建一个 1 元试用帐户](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)。
- 最新版本的 [Azure CLI](/cli/install-azure-cli?view=azure-cli-latest)。 

---

## <a name="1---create-a-single-database"></a>1 - 创建单一数据库 

[!INCLUDE [sql-database-create-single-database](includes/sql-database-create-single-database.md)]

## <a name="2---create-the-failover-group"></a>2 - 创建故障转移组 
此步骤在现有的 Azure SQL 服务器与另一区域中的新 Azure SQL 服务器之间创建一个[故障转移组](sql-database-auto-failover-group.md)。 然后，将示例数据库添加到故障转移组。 

# <a name="portal"></a>[Portal](#tab/azure-portal)
使用 Azure 门户创建故障转移组，并将单一数据库添加到其中。 


1. 在 [Azure 门户](https://portal.azure.cn)的左侧菜单上选择“所有服务”  。 
1. 在搜索框中键入 `sql servers`。 
1. 在**服务器名称**下选择服务器的名称以打开服务器的设置。

   ![打开单一数据库的服务器](media/sql-database-single-database-failover-group-tutorial/open-sql-db-server.png)

1. 在“设置”窗格下选择“故障转移组”，然后选择“添加组”以创建新的故障转移组。    

    ![添加新的故障转移组](media/sql-database-single-database-failover-group-tutorial/sqldb-add-new-failover-group.png)

1. 在“故障转移组”页上输入或选择以下值，然后选择“创建”：  
    - **故障转移组名称**：键入唯一的故障转移组名称，例如 `failovergrouptutorial`。 
    - **辅助服务器**：选择“配置所需设置”选项，然后选择“创建新服务器”。   或者，可以选择现有的服务器作为辅助服务器。 输入以下值后，选择“选择”。  
        - **服务器名称**：键入辅助服务器的唯一名称，例如 `mysqlsecondary`。 
        - **服务器管理员登录名**：键入 `azureuser`
        - **密码**：键入符合密码要求的复杂密码。
        - **位置**：从下拉列表中选择一个位置，例如“中国北部 2”。 此位置不能与主服务器的位置相同。

    > [!NOTE]
    > 服务器登录名和防火墙设置必须与主服务器相匹配。 
    
      ![为故障转移组创建辅助服务器](media/sql-database-single-database-failover-group-tutorial/create-secondary-failover-server.png)

   - **组中的数据库**：选择辅助服务器后，此选项将解除锁定。 使用该选项来**选择要添加的数据库**：请选择在第 1 部分创建的数据库。 将数据库添加到故障转移组的操作会自动启动异地复制过程。 
        
    ![将 SQL 数据库添加到故障转移组](media/sql-database-single-database-failover-group-tutorial/add-sqldb-to-failover-group.png)
        

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
使用 PowerShell 创建故障转移组，并将单一数据库添加到其中。 

   > [!NOTE]
   > 服务器登录名和防火墙设置必须与主服务器相匹配。 

   ```powershell
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "China East 2"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $databaseName = "mySampleDatabase"
   $drLocation = "China North 2"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName 
   Write-host "Failover group name is" $failoverGroupName
   
   # Create a secondary server in the failover region
   Write-host "Creating a secondary logical server in the failover region..."
   $drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   $drServer

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary logical server..."
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   $serverFirewallRule   
   
   # Create a failover group between the servers
   $failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
   New-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      -FailoverGroupName $failoverGroupName `
      -FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   $failovergroup
   
   # Add the database to the failover group
   Write-host "Adding the database to the failover group..." 
   Get-AzSqlDatabase `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -DatabaseName $databaseName | `
   Add-AzSqlDatabaseToFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Successfully added the database to the failover group..." 
   ```

本教程的此部分使用以下 PowerShell cmdlet：

| 命令 | 注释 |
|---|---|
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [New-AzSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserverfirewallrule) | 为逻辑服务器创建防火墙规则。 | 
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 新建 Azure SQL 数据库单一数据库。 | 
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 新建故障转移组。 |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) | 获取一个或更多个 SQL 数据库。 |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 将一个或更多个 Azure SQL 数据库添加到故障转移组。 |

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
使用 AZ CLI 创建故障转移组，并将单一数据库添加到其中。 

   > [!NOTE]
   > 服务器登录名和防火墙设置必须与主服务器相匹配。 

   ```azurecli
   #!/bin/bash
   # set variables
   $failoverLocation = "China East 2"
   $failoverServer = "failoverServer-$randomIdentifier"
   $failoverGroup = "failoverGroup-$randomIdentifier"

   echo "Creating a secondary logical server in the DR region..."
   az sql server create --name $failoverServer --resource-group $resourceGroup --location $failoverLocation --admin-user $login --admin-password $password
   
   echo "Creating a failover group between the two servers..."
   az sql failover-group create --name $failoverGroup --partner-server $failoverServer --resource-group $resourceGroup --server $server --add-db $database --failover-policy Automatic
   ```

本教程的此部分使用以下 Az CLI cmdlet：

| 命令 | 注释 |
|---|---|
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql server firewall-rule create](/cli/sql/server/firewall-rule) | 创建服务器的防火墙规则。 | 
| [az sql failover-group create](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | 创建故障转移组。 | 

---

## <a name="3---test-failover"></a>3 - 测试故障转移 
此步骤将故障转移组故障转移到辅助服务器，然后使用 Azure 门户故障回复。 

# <a name="portal"></a>[Portal](#tab/azure-portal)
使用 Azure 门户测试故障转移。 

1. 在 [Azure 门户中](https://portal.azure.cn)导航到你的“SQL 服务器”服务器。  
1. 在“设置”窗格下选择“故障转移组”，然后选择在第 2 部分创建的故障转移组。   
  
   ![在门户中选择故障转移组](media/sql-database-single-database-failover-group-tutorial/select-failover-group.png)

1. 查看哪个服务器是主服务器，哪个服务器是辅助服务器。 
1. 在“任务”窗格中选择“故障转移”，以故障转移包含示例单一数据库的故障转移组。  
1. 在告知将会断开 TDS 会话连接的警告中选择“是”。  

   ![故障转移包含 SQL 数据库的故障转移组](media/sql-database-single-database-failover-group-tutorial/failover-sql-db.png)

1. 查看哪个服务器现在是主服务器，哪个服务器是辅助服务器。 如果故障转移成功，这两个服务器的角色应会交换。 
1. 再次选择“故障转移”以将服务器故障回复到其初始角色。  

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)
使用 PowerShell 测试故障转移。 


检查辅助副本的角色： 

   ```powershell
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Check role of secondary replica
   Write-host "Confirming the secondary replica is secondary...." 
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole
   ```

故障转移到辅助服务器： 

   ```powershell
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully to" $drServerName 
   ```

将故障转移组还原到主服务器：

   ```powershell
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $serverName = "mysqlserver-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"
   
   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...." 
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failed failover group successfully back to" $serverName
   ```

本教程部分使用以下 PowerShell cmdlet：

| 命令 | 注释 |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 获取或列出 Azure SQL 数据库故障转移组。 |
| [Switch-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 执行 Azure SQL 数据库故障转移组的故障转移。 |



# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)
使用 AZ CLI 测试故障转移。 

确认哪个服务器是辅助服务器：

   
   ```azurecli
   echo "Verifying which server is in the secondary role..."
   az sql failover-group list --server $server --resource-group $resourceGroup
   ```

故障转移到辅助服务器： 

   ```azurecli
   echo "Failing over group to the secondary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $failoverServer
   echo "Successfully failed failover group over to" $failoverServer
   ```

将故障转移组还原到主服务器：

   ```azurecli
   echo "Failing over group back to the primary server..."
   az sql failover-group set-primary --name $failoverGroup --resource-group $resourceGroup --server $server
   echo "Successfully failed failover group back to" $server
   ```

本教程的此部分使用以下 Az CLI cmdlet：

| 命令 | 注释 |
|---|---|
| [az sql failover-group list](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | 列出某个服务器中的故障转移组。 |
| [az sql failover-group set-primary](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | 通过对当前主服务器上的所有数据库进行故障转移来设置故障转移组的主服务器。 | 

---

## <a name="clean-up-resources"></a>清理资源 
通过删除资源组来清理资源。 

# <a name="portal"></a>[Portal](#tab/azure-portal)
使用 Azure 门户删除资源组。 

1. 在 [Azure 门户](https://portal.azure.cn)中导航到你的资源组。
1. 选择“删除资源组”即可删除该资源组中的所有资源以及该组本身。  
1. 在文本框中键入资源组的名称 `myResourceGroup`，然后选择“删除”以删除该资源组。   

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 PowerShell 删除资源组。 

   ```powershell
   # Set variables
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Remove the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

本教程的此部分使用以下 PowerShell cmdlet：

| 命令 | 注释 |
|---|---|
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组 | 

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

使用 AZ CLI 删除资源组。 


   ```azurecli
   echo "Cleaning up resources by removing the resource group..."
   az group delete --name $resourceGroup
   echo "Successfully removed resource group" $resourceGroup
   ```

本教程的此部分使用以下 Az CLI cmdlet：

| 命令 | 注释 |
|---|---|
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

---


> [!IMPORTANT]
> 若要保留资源组但删除辅助数据库，请先将其从故障转移组中移除，然后再将其删除。 如果在从故障转移组中移除辅助数据库之前将其删除，则可能会导致不可预知的行为。 


## <a name="full-scripts"></a>完整脚本

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
# Set variables for your server and database
$subscriptionId = '<SubscriptionID>'
$randomIdentifier = $(Get-Random)
$resourceGroupName = "myResourceGroup-$randomIdentifier"
$location = "China East 2"
$adminLogin = "azureuser"
$password = "PWD27!"+(New-Guid).Guid
$serverName = "mysqlserver-$randomIdentifier"
$databaseName = "mySampleDatabase"
$drLocation = "China North 2"
$drServerName = "mysqlsecondary-$randomIdentifier"
$failoverGroupName = "failovergrouptutorial-$randomIdentifier"


# The ip address range that you want to allow to access your server 
# Leaving at 0.0.0.0 will prevent outside-of-azure connections
$startIp = "0.0.0.0"
$endIp = "0.0.0.0"

# Show randomized variables
Write-host "Resource group name is" $resourceGroupName 
Write-host "Password is" $password  
Write-host "Server name is" $serverName 
Write-host "DR Server name is" $drServerName 
Write-host "Failover group name is" $failoverGroupName

# Set subscription ID
Set-AzContext -SubscriptionId $subscriptionId

# Create a resource group
Write-host "Creating resource group..."
$resourceGroup = New-AzResourceGroup -Name $resourceGroupName -Location $location -Tag @{Owner="SQLDB-Samples"}
$resourceGroup


# Create a server with a system wide unique server name
Write-host "Creating primary logical server..."
$server = New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -Location $location `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
   -ArgumentList $adminLogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$server

# Create a server firewall rule that allows access from the specified IP range
Write-host "Configuring firewall for primary logical server..."
$serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
$serverFirewallRule

# Create General Purpose Gen5 database with 2 vCore
Write-host "Creating a gen5 2 vCore database..."
$database = New-AzSqlDatabase  -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -DatabaseName $databaseName `
   -Edition GeneralPurpose `
   -VCore 2 `
   -ComputeGeneration Gen5 `
   -MinimumCapacity 2 `
   -SampleName "AdventureWorksLT"
$database

# Create a secondary server in the failover region
Write-host "Creating a secondary logical server in the failover region..."
$drServer = New-AzSqlServer -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -Location $drLocation `
   -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
      -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
$drServer


# Create a failover group between the servers
$failovergroup = Write-host "Creating a failover group between the primary and secondary server..."
New-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -PartnerServerName $drServerName  `
   -FailoverGroupName $failoverGroupName `
   -FailoverPolicy Automatic `
   -GracePeriodWithDataLossHours 2
$failovergroup

# Add the database to the failover group
Write-host "Adding the database to the failover group..." 
Get-AzSqlDatabase `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -DatabaseName $databaseName | `
Add-AzSqlDatabaseToFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FailoverGroupName $failoverGroupName
Write-host "Successfully added the database to the failover group..." 

# Check role of secondary replica
Write-host "Confirming the secondary replica is secondary...." 
(Get-AzSqlDatabaseFailoverGroup `
   -FailoverGroupName $failoverGroupName `
   -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName).ReplicationRole

# Failover to secondary server
Write-host "Failing over failover group to the secondary..." 
Switch-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $drServerName `
   -FailoverGroupName $failoverGroupName
Write-host "Failed failover group successfully to" $drServerName 



# Revert failover to primary server
Write-host "Failing over failover group to the primary...." 
Switch-AzSqlDatabaseFailoverGroup `
   -ResourceGroupName $resourceGroupName `
   -ServerName $serverName `
   -FailoverGroupName $failoverGroupName
Write-host "Failed failover group successfully back to" $serverName


# Show randomized variables
Write-host "Resource group name is" $resourceGroupName 
Write-host "Password is" $password  
Write-host "Server name is" $serverName 
Write-host "DR Server name is" $drServerName 
Write-host "Failover group name is" $failoverGroupName

# Clean up resources by removing the resource group
# Write-host "Removing resource group..."
# Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
# Write-host "Resource group removed =" $resourceGroupName
```

此脚本使用以下命令。 表中的每条命令均链接到特定于命令的文档。

| 命令 | 注释 |
|---|---|
| [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) | 创建用于存储所有资源的资源组。 |
| [New-AzSqlServer](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserver) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [New-AzSqlServerFirewallRule](https://docs.microsoft.com/powershell/module/az.sql/new-azsqlserverfirewallrule) | 为逻辑服务器创建防火墙规则。 | 
| [New-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabase) | 新建 Azure SQL 数据库单一数据库。 | 
| [New-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 新建故障转移组。 |
| [Get-AzSqlDatabase](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabase) | 获取一个或更多个 SQL 数据库。 |
| [Add-AzSqlDatabaseToFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 将一个或更多个 Azure SQL 数据库添加到故障转移组。 |
| [Get-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 获取或列出 Azure SQL 数据库故障转移组。 |
| [Switch-AzSqlDatabaseFailoverGroup](https://docs.microsoft.com/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 执行 Azure SQL 数据库故障转移组的故障转移。 |
| [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) | 删除资源组 | 

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```cli
#!/bin/bash

subscriptionID=<subscriptionId>
resourceGroupName=myResourceGroup-$RANDOM
location=China East 2
adminLogin=azureuser
password="PWD27!"+`openssl rand -base64 18`
serverName=mysqlserver-$RANDOM
databaseName=mySampleDatabase
drLocation=China North 2
drServerName=mysqlsecondary-$RANDOM
failoverGroupName=failovergrouptutorial-$RANDOM

# The ip address range that you want to allow access to your DB. 
# Leaving at 0.0.0.0 will prevent outside-of-azure connections
startip=0.0.0.0
endip=0.0.0.0

# print out randomized variables
echo Resource group name is $resourceGroupName
echo Passowrd is $password
echo Servername is $serverName
echo DR Server name $drServerName
echo Failover group name $failoverGroupName

# set the subscription context for the Azure account
az account set -s $subscriptionID

# create a resource group
echo "Creating resource group..."
az group create \
   --name $resourceGroupName \
   --location $location \
   --tags Owner[=SQLDB-Samples]

# create a logical server in the resource group
echo "Creating primary logical server..."
az sql server create \
   --name $serverName \
   --resource-group $resourceGroupName \
   --location $location  \
   --admin-user $adminLogin \
   --admin-password $password

# configure a firewall rule for the server
echo "Configuring firewall..."
az sql server firewall-rule create \
   --resource-group $resourceGroupName \
   --server $serverName \
   -n AllowYourIp \
   --start-ip-address $startip \
   --end-ip-address $endip

# create a gen5 2vCore database in the server 
echo "Creating a gen5 2 vCore database..."
az sql db create \
   --resource-group $resourceGroupName \
   --server $serverName \
   --name $databaseName \
   --sample-name AdventureWorksLT \
   --edition GeneralPurpose \
   --family Gen5 \
   --capacity 2

# create a secondary server in the failover region
echo "Creating a secondary logical server in the DR region..."
az sql server create \
   --name $drServerName \
   --resource-group $resourceGroupName \
   --location $drLocation  \
   --admin-user $adminLogin\
   --admin-password $password

# create a failover group between the servers and add the database
echo "Creating a failover group between the two servers..."
az sql failover-group create \
   --name $failoverGroupName  \
   --partner-server $drServerName \
   --resource-group $resourceGroupName \
   --server $serverName \
   --add-db $databaseName
   --failover-policy Automatic

# verify which server is secondary
echo "Verifying which server is in the secondary role..."
az sql failover-group list \
   --server $serverName \
   --resource-group $resourceGroupName

# failover to the secondary server
echo "Failing over group to the secondary server..."
az sql failover-group set-primary \
   --name $failoverGroupName \
   --resource-group $resourceGroupName \
   --server $drServerName
echo "Successfully failed failover group over to" $drServerName

# revert failover group back to the primary server
echo "Failing over group back to the primary server..."
az sql failover-group set-primary \
   --name $failoverGroupName \
   --resource-group $resourceGroupName \
   --server $serverName
echo "Successfully failed failover group back to" $serverName

# clean up resources by removing the resource group
# echo "Cleaning up resources by removing the resource group..."
# az group delete --name $resourceGroupName 
# echo "Successfully removed resource group" $resourceGroupName
This script uses the following commands. Each command in the table links to command specific documentation.
```

| 命令 | 注释 |
|---|---|
| [az account set](/cli/account?view=azure-cli-latest#az-account-set) | 将订阅设置为当前的活动订阅。 | 
| [az group create](/cli/group#az-group-create) | 创建用于存储所有资源的资源组。 |
| [az sql server create](/cli/sql/server#az-sql-server-create) | 创建托管单一数据库和弹性池的 SQL 数据库服务器。 |
| [az sql server firewall-rule create](/cli/sql/server/firewall-rule) | 创建服务器的防火墙规则。 | 
| [az sql db create](/cli/sql/db?view=azure-cli-latest) | 创建数据库。 | 
| [az sql failover-group create](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-create) | 创建故障转移组。 | 
| [az sql failover-group list](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-list) | 列出某个服务器中的故障转移组。 |
| [az sql failover-group set-primary](/cli/sql/failover-group?view=azure-cli-latest#az-sql-failover-group-set-primary) | 通过对当前主服务器上的所有数据库进行故障转移来设置故障转移组的主服务器。 | 
| [az group delete](/cli/vm/extension#az-vm-extension-set) | 删除资源组，包括所有嵌套的资源。 |

# <a name="portal"></a>[Portal](#tab/azure-portal)
没有适用于 Azure 门户的脚本。 
 
---

可在以下文档中找到其他 Azure SQL 数据库脚本：[Azure PowerShell](sql-database-powershell-samples.md) 和 [Azure CLI](sql-database-cli-samples.md)。 

## <a name="next-steps"></a>后续步骤

在本教程中，你已将一个 Azure SQL 数据库单一数据库添加到故障转移组，并测试了故障转移。 你已了解如何： 

> [!div class="checklist"]
> - 创建 Azure SQL 数据库单一数据库。 
> - 在两个逻辑 SQL 服务器之间创建单一数据库的[故障转移组](sql-database-auto-failover-group.md)。
> - 测试故障转移。

请继续学习下一篇教程，了解如何将弹性池添加到故障转移组。 

> [!div class="nextstepaction"]
> [教程：将 Azure SQL 数据库弹性池添加到故障转移组](sql-database-elastic-pool-failover-group-tutorial.md)
