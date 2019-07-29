---
title: 使用 Azure 数据工厂复制多个容器中的文件 | Microsoft Docs
description: 了解如何使用解决方案模板通过 Azure 数据工厂复制多个容器中的文件。
services: data-factory
documentationcenter: ''
author: WenJason
ms.author: v-jay
ms.reviewer: douglasl
manager: digimobile
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 11/1/2018
ms.date: 07/08/2019
ms.openlocfilehash: ed828505bbc4987a651215107862300849f56c22
ms.sourcegitcommit: 5191c30e72cbbfc65a27af7b6251f7e076ba9c88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2019
ms.locfileid: "67570068"
---
# <a name="copy-files-from-multiple-containers-with-azure-data-factory"></a>使用 Azure 数据工厂复制多个容器中的文件

本文介绍一种解决方案模板，可用于从文件存储之间的多个容器复制文件。 例如，可以使用模板将一个 Azure Blob 存储帐户中的所有内容复制到另一个 Azure Blob 存储帐户。

> [!NOTE]
> 若要复制单个容器中的文件，使用[复制数据工具](copy-data-tool.md)通过单个复制活动创建管道的做法会更有效。 本文中的模板超出了你对该简单方案的需求。

## <a name="about-this-solution-template"></a>关于此解决方案模板

此模板枚举源存储中的容器。 然后将这些容器复制到目标存储。

该模板包含三个活动：
- **GetMetadata** 扫描源存储并获取容器列表。
- **ForEach** 获取 **GetMetadata** 活动提供的容器列表，然后循环访问该列表并将每个容器传递到 Copy 活动。
- **Copy** 将源存储中的每个容器复制到目标存储。

该模板定义两个参数：
- *SourceFilePath* 是数据源存储的路径，在其中可以获取容器的列表。 在大多数情况下，该路径是包含多个容器文件夹的根目录。 此参数的默认值为 `/`。
- *DestinationFilePath* 是文件将复制到目标存储中的路径。 此参数的默认值为 `/`。

## <a name="how-to-use-this-solution-template"></a>如何使用此解决方案模板

1. 转到“在文件存储之间复制多个文件容器”  模板。 创建与源存储的**新**连接。 源存储是你要从多个容器复制文件的位置。

    ![与源建立新的连接](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image1.png)

2. 创建与目标存储的**新**连接。

    ![与目标建立新的连接](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image2.png)

3. 选择“使用此模板”  。

    ![使用此模板](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image3.png)
    
4. 你将看到管道，如以下示例所示：

    ![显示管道](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image4.png)

5. 选择“调试”，输入**参数**，然后选择“完成”。  

    ![运行管道](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image5.png)

6. 查看结果。

    ![查看结果](media/solution-template-copy-files-multiple-containers/copy-files-multiple-containers-image6.png)

## <a name="next-steps"></a>后续步骤

- [使用 Azure 数据工厂通过控制表从数据库进行大容量复制](solution-template-bulk-copy-with-control-table.md)

- [使用 Azure 数据工厂复制多个容器中的文件](solution-template-copy-files-multiple-containers.md)