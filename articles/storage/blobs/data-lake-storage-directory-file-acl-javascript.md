---
title: 对 Azure Data Lake Storage Gen2（预览版）中的文件和 ACL 使用 JavaScript
description: 使用适用于 JavaScript 的 Azure Storage Data Lake 客户端库在启用了分层命名空间 (HNS) 的存储帐户中管理目录和文件以及目录访问控制列表 (ACL)。
author: WenJason
ms.service: storage
origin.date: 12/18/2019
ms.date: 03/09/2020
ms.author: v-jay
ms.topic: conceptual
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 94812a468011f3eb0121813e89116421f42ee721
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78412527"
---
# <a name="use-javascript-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2-preview"></a>使用 JavaScript 管理 Azure Data Lake Storage Gen2（预览版）中的目录、文件和 ACL

本文介绍了如何使用 JavaScript 在启用了分层命名空间 (HNS) 的存储帐户中创建和管理目录、文件与权限。 

> [!IMPORTANT]
> 本文中所述的 JavaScript 库目前以公共预览版提供。

“[包(节点包管理器)](https://www.npmjs.com/package/@azure/storage-file-datalake)” | ”[示例](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)” | ”[提供反馈](https://github.com/Azure/azure-sdk-for-java/issues)”

## <a name="prerequisites"></a>必备条件

> [!div class="checklist"]
> * Azure 订阅。 请参阅[获取 Azure 1 元人民币的试用订阅](https://wd.azure.cn/zh-cn/pricing/1rmb-trial-full)。
> * 一个已启用分层命名空间 (HNS) 的存储帐户。 按[这些](data-lake-storage-quickstart-create-account.md)说明创建一个。
> * 如果在 Node.js 应用程序中使用此包，则需要 Node.js 8.0.0 或更高版本。

## <a name="set-up-your-project"></a>设置项目

打开一个终端窗口，然后键入以下命令来安装适用于 JavaScript 的 Data Lake 客户端库。

```javascript
npm install @azure/storage-file-datalake
```

将此语句放置在代码文件的顶部，以导入 `storage-file-datalake` 包。 

```javascript
const AzureStorageDataLake = require("@azure/storage-file-datalake");
```

## <a name="connect-to-the-account"></a>连接到帐户 

若要使用本文中的代码片段，需创建一个表示存储帐户的 **DataLakeServiceClient** 实例。 若要获取一个，最简单的方法是使用帐户密钥。 

此示例使用帐户密钥创建 **DataLakeServiceClient** 的实例。

```javascript

function GetDataLakeServiceClient(accountName, accountKey) {

  const sharedKeyCredential = 
     new StorageSharedKeyCredential(accountName, accountKey);
  
  const datalakeServiceClient = new DataLakeServiceClient(
      `https://${accountName}.dfs.core.chinacloudapi.cn`, sharedKeyCredential);

  return datalakeServiceClient;             
}      

```
> [!NOTE]
> 此授权方法仅适用于 Node.js 应用程序。 如果打算在浏览器中运行代码，则可以使用 Azure Active Directory (AD) 进行授权。 有关如何执行该操作的指南，请参阅[适用于 JavaScript 的 Azure Storage File Data Lake 客户端库](https://www.npmjs.com/package/@azure/storage-file-datalake)自述文件。 

## <a name="create-a-file-system"></a>创建文件系统

文件系统充当文件的容器。 可以通过获取 **FileSystemClient** 实例，然后调用 **FileSystemClient.Create** 方法来创建一个。

此示例创建名为 `my-file-system` 的文件系统。 

```javascript
async function CreateFileSystem(datalakeServiceClient) {

  const fileSystemName = "my-file-system";
  
  const fileSystemClient = datalakeServiceClient.getFileSystemClient(fileSystemName);

  const createResponse = await fileSystemClient.create();
        
}
```

## <a name="create-a-directory"></a>创建目录

可以通过获取 **DirectoryClient** 实例，然后调用 **DirectoryClient.create** 方法来创建目录引用。

此示例将名为 `my-directory` 的目录添加到文件系统。 

```javascript
async function CreateDirectory(fileSystemClient) {
   
  const directoryClient = fileSystemClient.getDirectoryClient("my-directory");
  
  await directoryClient.create();

}
```

## <a name="rename-or-move-a-directory"></a>重命名或移动目录

可以通过调用 **DirectoryClient.rename** 方法来重命名或移动目录。 以参数形式传递所需目录的路径。 

此示例将子目录重命名为 `my-directory-renamed` 的名称。

```javascript
async function RenameDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.move("my-directory-renamed");

}
```

此示例将名为 `my-directory-renamed` 的目录移到名为 `my-directory-2` 的目录的子目录中。 

```javascript
async function MoveDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory-renamed"); 
  await directoryClient.move("my-directory-2/my-directory-renamed");      

}
```

## <a name="delete-a-directory"></a>删除目录

可以通过调用 **DirectoryClient.delete** 方法来删除目录。

此示例删除名为 `my-directory` 的目录。   

```javascript
async function DeleteDirectory(fileSystemClient) {

  const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
  await directoryClient.delete();

}
```

## <a name="manage-a-directory-acl"></a>管理目录 ACL

此示例获取并设置名为 `my-directory` 的目录的 ACL。 此示例为拥有用户提供读取、写入和执行权限，为拥有组授予读取和执行权限，并为所有其他用户提供读取访问权限。

> [!NOTE]
> 如果你的应用程序通过使用 Azure Active Directory (Azure AD) 来授予访问权限，请确保已向应用程序用来授权访问的安全主体分配了[存储 Blob 数据所有者角色](/role-based-access-control/built-in-roles#storage-blob-data-owner)。 若要详细了解如何应用 ACL 权限以及更改它们所带来的影响，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](/storage/blobs/data-lake-storage-access-control)。

```javascript
async function ManageDirectoryACLs(fileSystemClient) {

    const directoryClient = fileSystemClient.getDirectoryClient("my-directory"); 
    const permissions = await directoryClient.getAccessControl();

    console.log(permissions.acl);

    const acl = [
    {
      accessControlType: "user",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: true
      }
    },
    {
      accessControlType: "group",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: false,
        execute: true
      }
    },
    {
      accessControlType: "other",
      entityId: "",
      defaultScope: false,
      permissions: {
        read: true,
        write: true,
        execute: false
      }

    }

  ];

  await directoryClient.setAccessControl(acl);
}
```

## <a name="upload-a-file-to-a-directory"></a>将文件上传到目录

首先，读取文件。 此示例使用 Node.js `fs` 模块。 然后，通过创建一个 **FileClient** 实例并调用 **FileClient.create** 方法，在目标目录中创建文件引用。 通过调用 **FileClient.append** 方法来上传文件。 请确保通过调用 **FileClient.flush** 方法完成上传。

此示例将文本文件上传到名为 `my-directory` 的目录。

```javascript
async function UploadFile(fileSystemClient) {

  const fs = require('fs') 

  var content = "";
  
  fs.readFile('mytestfile.txt', (err, data) => { 
      if (err) throw err; 

      content = data.toString();

  }) 
  
  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");
  await fileClient.create();
  await fileClient.append(content, 0, content.length);
  await fileClient.flush(content.length);

}
```

## <a name="manage-a-file-acl"></a>管理文件 ACL

此示例获取并设置名为 `upload-file.txt` 的文件的 ACL。 此示例为拥有用户提供读取、写入和执行权限，为拥有组授予读取和执行权限，并为所有其他用户提供读取访问权限。

> [!NOTE]
> 如果你的应用程序通过使用 Azure Active Directory (Azure AD) 来授予访问权限，请确保已向应用程序用来授权访问的安全主体分配了[存储 Blob 数据所有者角色](/role-based-access-control/built-in-roles#storage-blob-data-owner)。 若要详细了解如何应用 ACL 权限以及更改它们所带来的影响，请参阅 [Azure Data Lake Storage Gen2 中的访问控制](/storage/blobs/data-lake-storage-access-control)。

```javascript
async function ManageFileACLs(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt"); 
  const permissions = await fileClient.getAccessControl();

  console.log(permissions.acl);

  const acl = [
  {
    accessControlType: "user",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: true
    }
  },
  {
    accessControlType: "group",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: false,
      execute: true
    }
  },
  {
    accessControlType: "other",
    entityId: "",
    defaultScope: false,
    permissions: {
      read: true,
      write: true,
      execute: false
    }

  }

];

await fileClient.setAccessControl(acl);        
}
```

## <a name="download-from-a-directory"></a>从目录下载

首先，创建表示要下载的文件的一个 **FileSystemClient** 实例。 使用 **FileSystemClient.read** 方法读取该文件。 然后，写入该文件。 此示例使用 Node.js `fs` 模块来执行该操作。 

> [!NOTE]
> 此文件下载方法仅适用于 Node.js 应用程序。 如果打算在浏览器中运行代码，请参阅[适用于 JavaScript 的 Azure Storage File Data Lake 客户端库](https://www.npmjs.com/package/@azure/storage-file-datalake)自述文件来查看有关如何在浏览器中执行此操作的示例。 

```javascript
async function DownloadFile(fileSystemClient) {

  const fileClient = fileSystemClient.getFileClient("my-directory/uploaded-file.txt");

  const downloadResponse = await fileClient.read();

  const downloaded = await streamToString(downloadResponse.readableStreamBody);
 
  async function streamToString(readableStream) {
    return new Promise((resolve, reject) => {
      const chunks = [];
      readableStream.on("data", (data) => {
        chunks.push(data.toString());
      });
      readableStream.on("end", () => {
        resolve(chunks.join(""));
      });
      readableStream.on("error", reject);
    });
  }   
  
  const fs = require('fs');

  fs.writeFile('mytestfiledownloaded.txt', downloaded, (err) => {
    if (err) throw err;
  });
}

```

## <a name="list-directory-contents"></a>列出目录内容

此示例输出名为 `my-directory` 的目录中的每个目录和文件的名称。

```javascript
async function ListFilesInDirectory(fileSystemClient) {
  
  let i = 1;

  let iter = await fileSystemClient.listPaths({path: "my-directory", recursive: true});

  for await (const path of iter) {
    
    console.log(`Path ${i++}: ${path.name}, is directory: ${path.isDirectory}`);
  }

}
```

## <a name="see-also"></a>另请参阅

* [包(节点包管理器)](https://www.npmjs.com/package/@azure/storage-file-datalake)
* [示例](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-file-datalake/samples)
* [提供反馈](https://github.com/Azure/azure-sdk-for-java/issues)