---
title: 将数据从 Azure Blob 存储复制到 Azure SQL 数据库
description: 本教程提供有关将数据从 Azure Blob 存储复制到 Azure SQL 数据库的分步说明。
services: data-factory
documentationcenter: ''
author: WenJason
manager: digimobile
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: tutorial
origin.date: 11/08/2019
ms.date: 05/11/2020
ms.author: v-jay
ms.openlocfilehash: a13899bfa8d6a5fb56e37b4229bbc354499e7878
ms.sourcegitcommit: f8d6fa25642171d406a1a6ad6e72159810187933
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/28/2020
ms.locfileid: "82198175"
---
# <a name="copy-data-from-azure-blob-to-azure-sql-database-using-azure-data-factory"></a>使用 Azure 数据工厂将数据从 Azure Blob 复制到 Azure SQL 数据库

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

本教程介绍如何创建一个将数据从 Azure Blob 存储复制到 Azure SQL 数据库的数据工厂管道。 本教程中的配置模式适用于从基于文件的数据存储复制到关系数据存储。 如需支持作为源和接收器的数据存储的列表，请参阅[支持的数据存储和格式](copy-activity-overview.md#supported-data-stores-and-formats)。

在本教程中，你将执行以下步骤：

> [!div class="checklist"]
> * 创建数据工厂。
> * 创建 Azure 存储和 Azure SQL 数据库链接服务。
> * 创建 Azure BLob 和 Azure SQL 数据库数据集。
> * 创建包含复制活动的管道。
> * 启动管道运行。
> * 监视管道和活动运行。

本教程使用 .NET SDK。 可以使用其他机制与 Azure 数据工厂交互，请参阅“快速入门”下的示例。 

如果没有 Azure 订阅，可在开始前创建一个 [1 元人民币试用](https://www.azure.cn/zh-cn/pricing/1rmb-trial-full/?form-type=identityauth)帐户。

## <a name="prerequisites"></a>先决条件

* *Azure 存储帐户*。 可将 Blob 存储用作*源*数据存储。 如果没有 Azure 存储帐户，请参阅[创建常规用途存储帐户](../storage/common/storage-account-create.md)。
* *Azure SQL 数据库*。 将数据库用作*接收器*数据存储。 如果你没有 Azure SQL 数据库，请参阅[创建 Azure SQL 数据库](../sql-database/sql-database-single-database-get-started.md)。
* *Visual Studio*。 本文中的演练使用 Visual Studio 2019。
* *[适用于 .NET 的 Azure SDK](https://docs.microsoft.com/dotnet/azure/dotnet-tools)* 。
* *Azure Active Directory 应用程序*。 如果没有 Azure Active Directory 应用程序，请参阅以下文章的[创建 Azure Active Directory 应用程序](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)部分：[如何：使用门户创建 Azure AD 应用程序](../active-directory/develop/howto-create-service-principal-portal.md)。 复制以下值，以便在后续步骤中使用：“应用程序(客户端) ID”、“身份验证密钥”和“目录(租户) ID”。    按照同一文章中的以下说明将应用程序分配到“参与者”角色。 

### <a name="create-a-blob-and-a-sql-table"></a>创建 blob 和 SQL 表

现在，通过创建源 Blob 和接收器 SQL 表来为教程准备 Azure Blob 和 Azure SQL 数据库。

#### <a name="create-a-source-blob"></a>创建源 blob

首先，通过创建容器并向其上传输入文本文件来创建源 Blob：

1. 打开记事本。 复制以下文本，并在本地将其保存到名为 *inputEmp.txt* 的文件中。

    ```inputEmp.txt
    John|Doe
    Jane|Doe
    ```

2. 使用 [Azure 存储资源管理器](https://azure.microsoft.com/features/storage-explorer/)等工具创建 *adfv2tutorial* 容器，并将 *inputEmp.txt* 文件上传到该容器。

#### <a name="create-a-sink-sql-table"></a>创建接收器 SQL 表

接下来，创建接收器 SQL 表：

1. 使用以下 SQL 脚本在 Azure SQL 数据库中创建 *dbo.emp* 表。

    ```sql
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50)
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

2. 允许 Azure 服务访问 SQL Server。 确保允许访问 Azure SQL 服务器中的 Azure 服务，以便数据工厂服务可以将数据写入 Azure SQL 服务器。 若要验证并启用此设置，请执行以下步骤：

    1. 转到 [Azure 门户](https://portal.azure.cn)来管理 SQL 服务器。 搜索并选择“SQL 服务器”  。

    2. 选择你的服务器。

    3. 在 SQL 服务器菜单的“安全性”标题下，选择“防火墙和虚拟网络”。  

    4. 在“防火墙和虚拟网络”页中的“允许 Azure 服务和资源访问此服务器”下，选择“打开”。   

## <a name="create-a-visual-studio-project"></a>创建 Visual Studio 项目

使用 Visual Studio 创建 C# .NET 控制台应用程序。

1. 打开 Visual Studio。
2. 在“开始”窗口中，选择“创建新项目”   。
3. 在“创建新项目”窗口中，从项目类型列表中选择 C# 版本的“控制台应用(.NET Framework)”。   然后，选择“下一步”  。
4. 在“配置新项目”窗口中，输入 *ADFv2Tutorial* 作为**项目名称**。  对于“位置”，请浏览到并/或创建用于保存项目的目录。  然后选择“创建”  。 新项目将显示在 Visual Studio IDE 中。

## <a name="install-nuget-packages"></a>安装 NuGet 包

接下来，使用 NuGet 包管理器安装所需的库包。

1. 在菜单栏中，选择“工具” > “NuGet 包管理器” > “包管理器控制台”。   
2. 在“包管理器控制台”窗格中，运行以下命令来安装包  。 有关 Azure 数据工厂 NuGet 包的信息，请参阅 [Microsoft.Azure.Management.DataFactory](https://www.nuget.org/packages/Microsoft.Azure.Management.DataFactory/)。

    ```package manager console
    Install-Package Microsoft.Azure.Management.DataFactory
    Install-Package Microsoft.Azure.Management.ResourceManager -PreRelease
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

## <a name="create-a-data-factory-client"></a>创建数据工厂客户端

遵循以下步骤创建数据工厂客户端。

1. 打开 *Program.cs*，然后使用以下代码覆盖现有的 `using` 语句，以添加对命名空间的引用。

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Rest;
    using Microsoft.Rest.Serialization;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.DataFactory;
    using Microsoft.Azure.Management.DataFactory.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```

2. 将以下用于设置变量的代码添加到 `Main` 方法。 请将 14 个占位符替换为自己的值。

    若要查看当前可以使用数据工厂的 Azure 区域列表，请参阅[各区域的产品可用性](https://azure.microsoft.com/global-infrastructure/services/?regions=china-non-regional,china-east,china-east-2,china-north,china-north-2&products=all)。 在“产品”下拉列表中，选择“浏览” > “分析” > “数据工厂”。     然后在“区域”下拉列表中，选择所需的区域。  此时会显示一个网格，其中包含适用于所选区域的数据工厂产品的可用性状态。

    > [!NOTE]
    > 数据工厂使用的数据存储（例如 Azure 存储和 Azure SQL 数据库）以及计算资源（例如 HDInsight）可能位于其他区域，而不是为数据工厂选择的区域。

    ```csharp
    // Set variables
    string tenantID = "<your tenant ID>";
    string applicationId = "<your application ID>";
    string authenticationKey = "<your authentication key for the application>";
    string subscriptionId = "<your subscription ID to create the factory>";
    string resourceGroup = "<your resource group to create the factory>";

    string region = "<location to create the data factory in, such as China East 2>";
    string dataFactoryName = "<name of data factory to create (must be globally unique)>";

    // Specify the source Azure Blob information
    string storageAccount = "<your storage account name to copy data>";
    string storageKey = "<your storage account key>";
    string inputBlobPath = "adfv2tutorial/";
    string inputBlobName = "inputEmp.txt";

    // Specify the sink Azure SQL Database information
    string azureSqlConnString =
        "Server=tcp:<your server name>.database.chinacloudapi.cn,1433;" +
        "Database=<your database name>;" +
        "User ID=<your username>@<your server name>;" +
        "Password=<your password>;" +
        "Trusted_Connection=False;Encrypt=True;Connection Timeout=30";
    string azureSqlTableName = "dbo.emp";

    string storageLinkedServiceName = "AzureStorageLinkedService";
    string sqlDbLinkedServiceName = "AzureSqlDbLinkedService";
    string blobDatasetName = "BlobDataset";
    string sqlDatasetName = "SqlDataset";
    string pipelineName = "Adfv2TutorialBlobToSqlCopy";
    ```

3. 在 `Main` 方法中添加用于创建 `DataFactoryManagementClient` 类的实例的以下代码。 将使用此对象来创建数据工厂、链接服务、数据集和管道。 还将使用此对象来监视管道运行详细信息。

    ```csharp
    // Authenticate and create a data factory management client
    var context = new AuthenticationContext("https://login.chinacloudapi.cn/" + tenantID);
    ClientCredential cc = new ClientCredential(applicationId, authenticationKey);
    AuthenticationResult result = context.AcquireTokenAsync(
        "https://management.chinacloudapi.cn/", cc
    ).Result;
    ServiceClientCredentials cred = new TokenCredentials(result.AccessToken);
    var client = new DataFactoryManagementClient(cred) { SubscriptionId = subscriptionId , BaseUri = new Uri("https://management.chinacloudapi.cn/")};
    ```

## <a name="create-a-data-factory"></a>创建数据工厂

在 `Main` 方法中添加用于创建数据工厂的以下代码。 

```csharp
// Create a data factory
Console.WriteLine("Creating a data factory " + dataFactoryName + "...");
Factory dataFactory = new Factory
{
    Location = region,
    Identity = new FactoryIdentity()
};

client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(dataFactory, client.SerializationSettings)
);

while (
    client.Factories.Get(
        resourceGroup, dataFactoryName
    ).ProvisioningState == "PendingCreation"
)
{
    System.Threading.Thread.Sleep(1000);
}
```

## <a name="create-linked-services"></a>创建链接服务

在本教程中，你将创建两个链接服务，分别用于源和接收器。

### <a name="create-an-azure-storage-linked-service"></a>创建 Azure 存储链接服务

在 `Main` 方法中添加用于创建 Azure 存储链接服务的以下代码。  有关支持的属性和详细信息，请参阅 [Azure Blob 链接服务属性](connector-azure-blob-storage.md#linked-service-properties)。

```csharp
// Create an Azure Storage linked service
Console.WriteLine("Creating linked service " + storageLinkedServiceName + "...");

LinkedServiceResource storageLinkedService = new LinkedServiceResource(
    new AzureStorageLinkedService
    {
        ConnectionString = new SecureString(
            "DefaultEndpointsProtocol=https;AccountName=" + storageAccount +
            ";AccountKey=" + storageKey +
        ";EndpointSuffix=core.chinacloudapi.cn"
        )
    }
);

client.LinkedServices.CreateOrUpdate(
    resourceGroup, dataFactoryName, storageLinkedServiceName, storageLinkedService
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(storageLinkedService, client.SerializationSettings)
);
```

### <a name="create-an-azure-sql-database-linked-service"></a>创建 Azure SQL 数据库链接服务

在 `Main` 方法中添加用于创建 Azure SQL 数据库链接服务的以下代码。  有关支持的属性和详细信息，请参阅 [Azure SQL 数据库链接服务属性](connector-azure-sql-database.md#linked-service-properties)。

```csharp
// Create an Azure SQL Database linked service
Console.WriteLine("Creating linked service " + sqlDbLinkedServiceName + "...");

LinkedServiceResource sqlDbLinkedService = new LinkedServiceResource(
    new AzureSqlDatabaseLinkedService
    {
        ConnectionString = new SecureString(azureSqlConnString)
    }
);

client.LinkedServices.CreateOrUpdate(
    resourceGroup, dataFactoryName, sqlDbLinkedServiceName, sqlDbLinkedService
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(sqlDbLinkedService, client.SerializationSettings)
);
```

## <a name="create-datasets"></a>创建数据集

在本部分中创建两个数据集：一个用于源，另一个用于接收器。

### <a name="create-a-dataset-for-source-azure-blob"></a>为源 Azure Blob 创建数据集

在 `Main` 方法中添加用于创建 Azure Blob 数据集的以下代码。  有关支持的属性和详细信息，请参阅 [Azure Blob 数据集属性](connector-azure-blob-storage.md#dataset-properties)。

在 Azure Blob 中定义表示源数据的数据集。 此 Blob 数据集引用在上一步中创建的 Azure 存储链接服务，并说明以下信息：

- 要从中复制的 Blob 的位置：`FolderPath` 和 `FileName`
- 指示分析内容方式的 Blob 格式：`TextFormat` 及其设置（例如列分隔符）
- 数据结构，包括列名称和数据类型（本示例中映射到接收器 SQL 表）

```csharp
// Create an Azure Blob dataset
Console.WriteLine("Creating dataset " + blobDatasetName + "...");
DatasetResource blobDataset = new DatasetResource(
    new AzureBlobDataset
    {
        LinkedServiceName = new LinkedServiceReference {
            ReferenceName = storageLinkedServiceName
        },
        FolderPath = inputBlobPath,
        FileName = inputBlobName,
        Format = new TextFormat { ColumnDelimiter = "|" },
        Structure = new List<DatasetDataElement>
        {
            new DatasetDataElement { Name = "FirstName", Type = "String" },
            new DatasetDataElement { Name = "LastName", Type = "String" }
        }
    }
);

client.Datasets.CreateOrUpdate(
    resourceGroup, dataFactoryName, blobDatasetName, blobDataset
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(blobDataset, client.SerializationSettings)
);
```

### <a name="create-a-dataset-for-sink-azure-sql-database"></a>为接收器 Azure SQL 数据库创建数据集

在 `Main` 方法中添加用于创建 Azure SQL 数据库数据集的以下代码。  有关支持的属性和详细信息，请参阅 [Azure SQL 数据库数据集属性](connector-azure-sql-database.md#dataset-properties)。

在 Azure SQL 数据库中定义表示接收器数据的数据集。 此数据集引用在上一步中创建的 Azure SQL 数据库链接服务。 它还指定用于保存所复制数据的 SQL 表。

```csharp
// Create an Azure SQL Database dataset
Console.WriteLine("Creating dataset " + sqlDatasetName + "...");
DatasetResource sqlDataset = new DatasetResource(
    new AzureSqlTableDataset
    {
        LinkedServiceName = new LinkedServiceReference
        {
            ReferenceName = sqlDbLinkedServiceName
        },
        TableName = azureSqlTableName
    }
);

client.Datasets.CreateOrUpdate(
    resourceGroup, dataFactoryName, sqlDatasetName, sqlDataset
);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(sqlDataset, client.SerializationSettings)
);
```

## <a name="create-a-pipeline"></a>创建管道

在 `Main` 方法中添加用于创建包含复制活动的管道的以下代码。  在本教程中，此管道包含一个活动：`CopyActivity`，它接受 Blob 数据集作为源，接受 SQL 数据集作为接收器。 有关复制活动的详细信息，请参阅 [Azure 数据工厂中的复制活动](copy-activity-overview.md)。

```csharp
// Create a pipeline with copy activity
Console.WriteLine("Creating pipeline " + pipelineName + "...");
PipelineResource pipeline = new PipelineResource
{
    Activities = new List<Activity>
    {
        new CopyActivity
        {
            Name = "CopyFromBlobToSQL",
            Inputs = new List<DatasetReference>
            {
                new DatasetReference() { ReferenceName = blobDatasetName }
            },
            Outputs = new List<DatasetReference>
            {
                new DatasetReference { ReferenceName = sqlDatasetName }
            },
            Source = new BlobSource { },
            Sink = new SqlSink { }
        }
    }
};

client.Pipelines.CreateOrUpdate(resourceGroup, dataFactoryName, pipelineName, pipeline);
Console.WriteLine(
    SafeJsonConvert.SerializeObject(pipeline, client.SerializationSettings)
);
```

## <a name="create-a-pipeline-run"></a>创建管道运行

在 `Main` 方法中添加用于触发管道运行的以下代码。 

```csharp
// Create a pipeline run
Console.WriteLine("Creating pipeline run...");
CreateRunResponse runResponse = client.Pipelines.CreateRunWithHttpMessagesAsync(
    resourceGroup, dataFactoryName, pipelineName
).Result.Body;
Console.WriteLine("Pipeline run ID: " + runResponse.RunId);
```

## <a name="monitor-a-pipeline-run"></a>监视管道运行

现在，插入代码以检查管道运行状态，并获取有关复制活动运行的详细信息。

1. 在 `Main` 方法中添加以下代码用于持续检查管道运行状态，直到它完成数据复制为止。

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(
            resourceGroup, dataFactoryName, runResponse.RunId
        );
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. 在 `Main` 方法中添加以下代码用于检索复制活动运行详细信息，例如，读取/写入的数据大小。

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");

    RunFilterParameters filterParams = new RunFilterParameters(
        DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)
    );

    ActivityRunsQueryResponse queryResponse = client.ActivityRuns.QueryByPipelineRun(
        resourceGroup, dataFactoryName, runResponse.RunId, filterParams
    );

    if (pipelineRun.Status == "Succeeded")
    {
        Console.WriteLine(queryResponse.Value.First().Output);
    }
    else
        Console.WriteLine(queryResponse.Value.First().Error);

    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

## <a name="run-the-code"></a>运行代码

选择“生成” > “生成解决方案”来生成应用程序。   然后选择“调试” > “开始调试”来启动应用程序，并验证管道执行。  

控制台会输出数据工厂、链接服务、数据集、管道和管道运行的创建进度。 然后，检查管道运行状态。 请等到出现包含数据读取/写入大小的复制活动运行详细信息。 然后，可以使用 SQL Server Management Studio (SSMS) 或 Visual Studio 之类工具连接到目标 Azure SQL 数据库，并检查指定的目标表是否包含复制的数据。

### <a name="sample-output"></a>示例输出

```json
Creating a data factory AdfV2Tutorial...
{
  "identity": {
    "type": "SystemAssigned"
  },
  "location": "China East 2"
}
Creating linked service AzureStorageLinkedService...
{
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": {
        "type": "SecureString",
        "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>;EndpointSuffix=core.chinacloudapi.cn"
      }
    }
  }
}
Creating linked service AzureSqlDbLinkedService...
{
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": {
        "type": "SecureString",
        "value": "Server=tcp:<servername>.database.chinacloudapi.cn,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
      }
    }
  }
}
Creating dataset BlobDataset...
{
  "properties": {
    "type": "AzureBlob",
    "typeProperties": {
      "folderPath": "adfv2tutorial/",
      "fileName": "inputEmp.txt",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "|"
      }
    },
    "structure": [
      {
        "name": "FirstName",
        "type": "String"
      },
      {
        "name": "LastName",
        "type": "String"
      }
    ],
    "linkedServiceName": {
      "type": "LinkedServiceReference",
      "referenceName": "AzureStorageLinkedService"
    }
  }
}
Creating dataset SqlDataset...
{
  "properties": {
    "type": "AzureSqlTable",
    "typeProperties": {
      "tableName": "dbo.emp"
    },
    "linkedServiceName": {
      "type": "LinkedServiceReference",
      "referenceName": "AzureSqlDbLinkedService"
    }
  }
}
Creating pipeline Adfv2TutorialBlobToSqlCopy...
{
  "properties": {
    "activities": [
      {
        "type": "Copy",
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          }
        },
        "inputs": [
          {
            "type": "DatasetReference",
            "referenceName": "BlobDataset"
          }
        ],
        "outputs": [
          {
            "type": "DatasetReference",
            "referenceName": "SqlDataset"
          }
        ],
        "name": "CopyFromBlobToSQL"
      }
    ]
  }
}
Creating pipeline run...
Pipeline run ID: 1cd03653-88a0-4c90-aabc-ae12d843e252
Checking pipeline run status...
Status: InProgress
Status: InProgress
Status: Succeeded
Checking copy activity run details...
{
  "dataRead": 18,
  "dataWritten": 28,
  "rowsCopied": 2,
  "copyDuration": 2,
  "throughput": 0.01,
  "errors": [],
  "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (China East 2)",
  "usedDataIntegrationUnits": 2,
  "billedDuration": 2
}

Press any key to exit...
```

## <a name="next-steps"></a>后续步骤

此示例中的管道将数据从 Azure Blob 存储中的一个位置复制到另一个位置。 你已了解如何：

> [!div class="checklist"]
> * 创建数据工厂。
> * 创建 Azure 存储和 Azure SQL 数据库链接服务。
> * 创建 Azure BLob 和 Azure SQL 数据库数据集。
> * 创建包含复制活动的管道。
> * 启动管道运行。
> * 监视管道和活动运行。

若要了解如何将数据从本地复制到云，请转到以下教程：

> [!div class="nextstepaction"]
>[将数据从本地复制到云](tutorial-hybrid-copy-powershell.md)