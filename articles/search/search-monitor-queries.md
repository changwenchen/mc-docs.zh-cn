---
title: 监视查询
titleSuffix: Azure Cognitive Search
description: 监视性能和吞吐量的查询指标。 收集并分析诊断日志中的查询字符串输入。
manager: nitinme
author: HeidiSteen
ms.author: v-tawe
ms.service: cognitive-search
ms.topic: conceptual
origin.date: 02/18/2020
ms.date: 03/16/2020
ms.openlocfilehash: 9f59735ad1423206b2abdb5e98204e4819466492
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78850510"
---
# <a name="monitor-query-requests-in-azure-cognitive-search"></a>监视 Azure 认知搜索中的查询请求

本文介绍如何使用指标与诊断日志来度量查询性能和查询量。 此外，介绍如何收集查询中使用的输入字词 - 评估搜索集的实用性和有效性时必须提供这些信息。

馈送到指标中的历史数据将保留 30 天。 若要保留更长时间，或者要报告操作数据和查询字符串，请务必启用一项[诊断设置](search-monitor-logs.md)，该设置指定用于保存所记录的事件和指标的存储选项。

可最大程度地提高数据度量完整性的条件包括：

+ 使用可计费服务（在“基本”或“标准”层创建的服务）。 免费服务由多个订阅者共享，当负载变化时，这会造成某种程度的波动。

+ 如果可能，请使用单个副本和分区创建包容性的隔离环境。 如果使用多个副本，将会计算多个节点中的查询指标的平均值，这可能会降低结果的精确度。 同样，使用多个分区意味着数据将被分割，如果同时正在进行索引编制，则某些分区有可能包含不同的数据。 优化查询性能时，单个节点和分区就能提供更稳定的环境用于测试。

> [!Tip]
> 使用附加的客户端代码和 Application Insights，还可以捕获点击率数据，以更深入地了解哪些内容引起了应用程序用户的兴趣。 有关详细信息，请参阅[搜索流量分析](search-traffic-analytics.md)。

## <a name="query-volume-qps"></a>查询量 (QPS)

查询量以**每秒搜索查询数** (QPS) 度量。QPS 是一个内置指标，可按一分钟时间范围内执行的查询数的平均、计数、最小或最大值来报告。 指标的一分钟间隔 (TimeGrain = "PT1M") 在系统中是固定的。

查询往往在若干毫秒内即可完成执行，因此，指标中仅显示以秒度量的查询。

| 聚合类型 | 说明 |
|------------------|-------------|
| 平均值 | 执行查询的某一分钟内的平均秒数。|
| 计数 | 在一分钟间隔内发出到日志的指标数。 |
| 最大值 | 在一分钟内每秒注册的最大搜索查询数。 |
| 最小值 | 在一分钟内每秒注册的最小搜索查询数。  |
| Sum | 在一分钟内执行的所有查询数之和。  |

例如，在一分钟内可能出现如下所述的模式：有 1 秒出现高负载（这是 SearchQueriesPerSecond 的最大值），紧接着有 58 秒的平均负载，最后 1 秒只有 1 个查询（这是最小值）。

另举一例：如果某个节点发出 100 个指标，其中每个指标的值为 40，那么，“计数”为 100，“总和”为 4000，“平均值”为 40，“最大值”为 40。

## <a name="query-performance"></a>查询性能

服务范围的查询性能以搜索延迟（完成查询花费的时间）以及由于资源争用而丢弃的受限制查询度量。

### <a name="search-latency"></a>搜索延迟

| 聚合类型 | 延迟 | 
|------------------|---------|
| 平均值 | 平均查询持续时间（毫秒）。 | 
| 计数 | 在一分钟间隔内发出到日志的指标数。 |
| 最大值 | 样本中运行时间最长的查询。 | 
| 最小值 | 样本中运行时间最短的查询。  | 
| 总计 | 样本中在时间间隔（一分钟）内执行的所有查询的总执行时间。  |

考虑以下“搜索延迟”指标示例：  已采样 86 个查询，平均持续时间为 23.26 毫秒。 最小值 0 表示丢弃了某些查询。 运行时间最长的查询花费了 1000 毫秒才完成。 总执行时间为 2 秒。

![延迟聚合](./media/search-monitor-usage/metrics-latency.png "延迟聚合")

### <a name="throttled-queries"></a>受限制的查询

受限制的查询是指已丢弃而未处理的查询。 在大多数情况下，限制是运行服务的正常组成部分。  发生限制并不一定表示出现了问题。

如果当前处理的请求数超出了可用资源，则会发生限制。 排除某个副本的轮换或者在编制索引期间，可能会看到受限制请求的数量增多。 查询和索引编制请求由同一组资源处理。

服务根据资源消耗量确定是否丢弃请求。 内存、CPU 和磁盘 IO 资源消耗百分比是一段时间的平均值。 如果此百分比超过阈值，则所有索引请求将受到限制，直到请求量减少。 

根据所用的客户端，可通过以下方式指示受限制的请求：

+ 服务返回错误“发送的请求过多”。 请稍后再试。” 
+ 服务返回 503 错误代码，指示服务当前不可用。 
+ 如果使用的是门户（例如搜索资源管理器），则会以静默方式丢弃查询，你需要再次单击“搜索”。

若要确认受限制的查询，请使用“受限制的搜索查询”指标。  可以在门户中浏览指标，或根据本文中所述创建一个警报指标。 对于在采样间隔内丢弃的查询，请使用“总计”来获取未执行的查询百分比。 

| 聚合类型 | 限制 |
|------------------|-----------|
| 平均值 | 间隔内丢弃的查询百分比。 |
| 计数 | 在一分钟间隔内发出到日志的指标数。 |
| 最大值 | 间隔内丢弃的查询百分比。|
| 最小值 | 间隔内丢弃的查询百分比。 |
| 总计 | 间隔内丢弃的查询百分比。 |

对于“受限制的搜索查询百分比”、最小值、最大值、平均值和总计，全都具有相同的值：在一分钟内的搜索查询总数中，已限制搜索查询百分比  。

在以下屏幕截图中，第一个数字是计数（或发送到日志的指标数）。 显示在顶部或者将鼠标悬停在指标上显示的其他聚合包括平均值、最大值和总计。 在此示例中，未丢弃任何请求。

![受限制聚合](./media/search-monitor-usage/metrics-throttle.png "受限制聚合")

## <a name="explore-metrics-in-the-portal"></a>在门户中浏览指标

为了让用户快速查看当前数字，服务“概述”页上的“监视”选项卡会显示三个按固定间隔以小时、天和周度量的指标（“搜索延迟”、“每秒搜索查询数(每搜索单位)”和“受限制的搜索查询百分比”），并提供用于更改聚合类型的选项。    

若要进行更深入的浏览，请从“监视”菜单中打开指标资源管理器，以便可以分层、放大和可视化数据，从而浏览趋势或异常情况。  在这篇[有关创建指标图表的教程](https://docs.azure.cn/azure-monitor/learn/tutorial-metrics-explorer)中详细了解指标资源管理器。

1. 在“监视”部分下，选择“指标”打开指标资源管理器，其中的数据范围是根据搜索服务设置的。 

1. 在“指标”下，从下拉列表中选择一个指标，并查看偏好类型的可用聚合列表。 聚合定义在每个时间间隔如何对收集的值采样。

   ![QPS 指标的指标资源管理器](./media/search-monitor-usage/metrics-explorer-qps.png "QPS 指标的指标资源管理器")

1. 在右上角设置时间间隔。

1. 选择可视化效果。 默认设置为折线图。

1. 选择“添加指标”并选择不同的聚合来叠加更多聚合。 

1. 在折线图上放大感兴趣的区域。 将鼠标指针放在区域的开头位置，单击并按住鼠标左键，拖动到区域的另一侧，然后松开按钮。 图表将放大该时间范围。

## <a name="identify-strings-used-in-queries"></a>识别查询中使用的字符串

启用诊断日志记录时，系统将捕获 **AzureDiagnostics** 表中的查询请求。 作为先决条件，必须已启用[诊断日志记录](search-monitor-logs.md)，并指定 Log Analytics 工作区或其他存储选项。

1. 在“监视”部分下，选择“日志”在 Log Analytics 中打开一个空查询窗口。 

1. 运行以下表达式来搜索 Query.Search 操作，这会返回表格格式的结果集，其中包含操作名称、查询字符串、查询的索引以及找到的文档数。 最后两条语句排除针对样本索引运行的、包含空的或未指定的搜索的查询字符串，这可以减少结果中的干扰信息。

   ```
   AzureDiagnostics
   | project OperationName, Query_s, IndexName_s, Documents_d
   | where OperationName == "Query.Search"
   | where Query_s != "?api-version=2019-05-06&search=*"
   | where IndexName_s != "realestate-us-sample-index"
   ```

1. （可选）在 *Query_s* 中设置列筛选器，以基于特定的语法或字符串进行搜索。 例如，可以基于“等于”进行筛选`?api-version=2019-05-06&search=*&%24filter=HotelName`。 

   ![记录的查询字符串](./media/search-monitor-usage/log-query-strings.png "记录的查询字符串")

尽管此方法适用于临时调查，但生成报告可以在更有利于分析的布局中合并和呈现查询字符串。

## <a name="identify-long-running-queries"></a>识别长时间运行的查询

添加持续时间列以获取所有查询的数量，而不仅仅是作为指标选取的查询的数量。 将此数据排序可以显示完成哪些查询所花费的时间最长。

1. 在“监视”部分下，选择“日志”以查询日志信息。 

1. 运行以下查询以返回按持续时间（以毫秒为单位）排序的查询。 运行时间最长的查询列在最前面。

   ```
   AzureDiagnostics
   | project OperationName, resultSignature_d, DurationMs, Query_s, Documents_d, IndexName_s
   | where OperationName == "Query.Search"
   | sort by DurationMs
   ```

   ![按持续时间对查询排序](./media/search-monitor-usage/azurediagnostics-table-sortby-duration.png "按持续时间对查询排序")

## <a name="create-a-metric-alert"></a>创建指标警报

指标警报会建立一个阈值，达到该阈值时，将会发出通知，或者触发预先定义的纠正措施。 

对于搜索服务，通常会针对搜索延迟和受限制的查询创建指标警报。 如果你知道何时丢弃了查询，可以查看降低负载或增加容量的补救措施。 例如，如果受限制的查询在索引编制期间增加，可将其推迟到查询活动减少时。

推送特定副本分区配置的限制时，针对查询量阈值 (QPS) 设置警报也很有帮助。

1. 在“监视”部分下选择“警报”，然后单击“+ 新建警报规则”。   确保选择你的搜索服务作为资源。

1. 在“条件”下单击“添加”。 

1. 配置信号逻辑。 对于信号类型，请选择“指标”，然后选择信号。 

1. 选择信号后，可以使用图表来可视化历史数据，以便在如何继续设置条件方面做出明智的决策。

1. 接下来，向下滚动到“警报逻辑”。 对于概念证明，可以人为地指定一个较小值进行测试。

   ![警报逻辑](./media/search-monitor-usage/alert-logic-qps.png "警报逻辑")

1. 接下来，指定或创建操作组。 这是在达到阈值时要调用的响应措施。 该措施可以是推送通知或自动响应。

1. 最后，指定警报详细信息。 命名并描述警报，分配严重性值，并指定是要以启用还是禁用状态创建规则。

   ![警报详细信息](./media/search-monitor-usage/alert-details.png "警报详细信息")

如果指定了电子邮件通知，将会收到来自“Microsoft Azure”的、主题行为“Azure:已激活，严重性:3 `<your rule name>`”的电子邮件。

<!-- ## Report query data

Power BI is an analytical reporting tool useful for visualizing data, including log information. If you are collecting data in Blob storage, a Power BI template makes it easy to spot anomalies or trends. Use this link to download the template. -->

## <a name="next-steps"></a>后续步骤

如果尚未这样做，请查看搜索服务监视基础知识，以全方面地了解监督功能。

> [!div class="nextstepaction"]
> [监视 Azure 认知搜索中的操作和活动](search-monitor-usage.md)