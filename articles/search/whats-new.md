---
title: 新功能公告
titleSuffix: Azure Cognitive Search
description: 公告新的和增强的功能，包括将 Azure 搜索这项服务重命名为 Azure 认知搜索。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: overview
origin.date: 03/24/2020
ms.date: 04/20/2020
ms.openlocfilehash: 6889dce5ddb90951c4c82b9217fae1c0fad637e0
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588735"
---
# <a name="whats-new-in-azure-cognitive-search"></a>Azure 认知搜索中的新增功能

了解该服务中的新增功能。 请将本页加入书签，以随时了解该服务的最新信息。

<a name="new-service-name"></a>

## <a name="new-service-name"></a>新服务名称

Azure 搜索现已重命名为 **Azure 认知搜索**，这样可以反映认知技能的扩展（仍为可选）使用以及在核心操作中进行的 AI 处理。 API 版本、NuGet 包、命名空间和终结点未更改。 新的和现有的搜索解决方案不受服务名称更改的影响。

## <a name="feature-announcements"></a>功能公告

### <a name="march-2020"></a>2020 年 3 月

+ [本机 blob 软删除（预览版）](search-howto-indexing-azure-blob-storage.md#incremental-indexing-and-deletion-detection)意味着 Azure 认知搜索中的 Azure blob 存储索引器会识别处于软删除状态的 blob，并在编制索引过程中删除相应的搜索文档。

+ 新的稳定版[管理 REST API (2020-03-13)](https://docs.microsoft.com/rest/api/searchmanagement/management-api-versions) 现已可用。 

### <a name="february-2020"></a>2020 年 2 月

+ [PII 检测（预览版）](cognitive-search-skill-pii-detection.md)是编制索引期间使用的一项认知技能，它可以从输入文本中提取个人身份信息，并可让你通过多种方式在该文本中屏蔽此类信息。

+ [自定义实体查找（预览版）](cognitive-search-skill-custom-entity-lookup.md )可在用户自定义的单词和短语列表中查找文本。 它使用此列表为包含任何匹配实体的所有文档加上标签。 该技能还支持一定程度的模糊匹配，应用此匹配方法可以查找类似但不完全相同的匹配项。 

### <a name="january-2020"></a>2020 年 1 月

+ [客户托管的加密密钥](search-security-manage-encryption-keys.md)现已正式发布。 如果使用 REST，则可使用 `api-version=2019-05-06` 访问该功能。 对于托管代码，正确的包仍为 [.NET SDK 8.0-preview 版](search-dotnet-sdk-migration-version-9.md)，即使该功能不是预览版。 

+ 可以通过两种机制（当前均为预览版）提供对搜索服务的专用访问权限：

  + 可以使用管理 REST API `api-version=2019-10-01-Preview` 来创建服务，限制对特定 IP 地址的访问。 预览版 API 在 [CreateOrUpdate API](https://docs.microsoft.com/rest/api/searchmanagement/2019-10-01-preview/createorupdate-service) 中有新的 **IpRule** 和 **NetworkRuleSet** 属性。 此预览功能在所选区域中提供。 有关详细信息，请参阅[如何使用管理 REST API](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api)。

  <!-- + Currently available through a limited-access preview, you can provision an Azure Search service that supports Azure Private Endpoint for connections from clients on the same virtual network. For more information, see [Create a Private Endpoint for a secure connection](service-create-private-endpoint.md). -->

### <a name="december-2019"></a>2019 年 12 月

+ [创建应用（预览版）](search-create-app-portal.md)是门户中用于生成可下载 HTML 文件的新向导。 该文件附带嵌入式脚本，此脚本呈现可运行的“localhost”样式 Web 应用（已绑定到搜索服务中的索引）。 页面在向导中可配置，并且可以包含搜索栏、结果区域、边栏导航和自动提示查询支持。 可以脱机修改 HTML，以便扩展或自定义工作流或外观。

<!-- + [Create a private endpoint for secure connections (preview)](service-create-private-endpoint.md) explains how to set up a Private Link for secure connections to your search service. This preview feature is available upon request and uses [Azure Private Link](../private-link/private-link-overview.md) and [Azure Virtual Network](../virtual-network/virtual-networks-overview.md) as part of the solution. -->

### <a name="november-2019---ignite-conference"></a>2019 年 11 月 - Ignite 大会

+ [增量扩充（预览版）](cognitive-search-incremental-indexing-conceptual.md)将缓存和有状态性添加到了扩充管道，使你能够处理特定的步骤或阶段，且不丢失已处理的内容。 以前，对扩充管道进行任何更改都需要重新生成整个管道。 使用增量扩充时，会保留高开销分析（尤其是图像分析）的输出。

<!-- 
+ Custom Entity Lookup is a cognitive skill used during indexing that allows you to provide a list of custom entities (such as part numbers, diseases, or names of locations you care about) that should be found within the text. It supports fuzzy matching, case-insensitive matching, and entity synonyms. -->

+ [文档提取（预览版）](cognitive-search-skill-document-extraction.md)是编制索引期间使用的一项认知技能，用于从技能集中提取文件内容。 以前，文档破解只能在技能集执行之前发生。 使用此项新增技能，还可以在技能集的执行过程中执行此操作。

+ [文本翻译](cognitive-search-skill-text-translation.md)是编制索引期间使用的一项认知技能，它可以评估文本，并为每个记录返回翻译成指定目标语言的文本。

+ [Power BI 模板](https://github.com/Azure-Samples/cognitive-search-templates/blob/master/README.md)可以在 Power BI Desktop 中立即开始可视化和分析知识存储中的扩充内容。 此模板适用于通过[导入数据向导](knowledge-store-create-portal.md)创建的 Azure 表投影。

+ 索引器现在支持 [Azure Data Lake Storage Gen2（预览版）](search-howto-index-azure-data-lake-storage.md)、[Cosmos DB Gremlin API（预览版）](search-howto-index-cosmosdb.md)和 [Cosmos DB Cassandra API（预览版）](search-howto-index-cosmosdb.md)。 可以使用[此窗体](https://aka.ms/azure-cognitive-search/indexer-preview)登录。 获允加入预览计划以后，你会收到确认电子邮件。

<!-- ### July 2019 -->

<!-- + Generally available in [Azure Government Cloud](https://docs.microsoft.com/azure/azure-government/documentation-government-services-webandmobile#azure-cognitive-search). -->

## <a name="service-updates"></a>服务更新

Azure 认知搜索的[服务更新公告](https://updates.azure.cn)可以在 Azure 网站上找到。