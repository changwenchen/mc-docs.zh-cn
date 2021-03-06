---
title: 统一多个 Azure Monitor Application Insights 资源 | Azure Docs
description: 本文详细介绍了如何在 Azure Monitor 日志中使用函数来查询多个 Application Insights 资源并直观呈现这些数据。
author: lingliw
manager: digimobile
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
origin.date: 02/02/2020
ms.date: 2/18/2020
ms.author: v-lingwu
ms.openlocfilehash: f8ef98e499b08cca462de1213983a67d81a1b4b7
ms.sourcegitcommit: b80d236ce3c706abc25bbaa41b0ccddd896e48fc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "81873124"
---
# <a name="unify-multiple-azure-monitor-application-insights-resources"></a>统一多个 Azure Monitor Application Insights 资源 
本文介绍如何在一个位置查询和查看所有 Application Insights 日志数据（即使这些数据位于不同 Azure 订阅），可作为弃用 Application Insights 连接器的替换方式。 可以在单个查询中包含的 Application Insights 资源的数量限制为 100。

## <a name="recommended-approach-to-query-multiple-application-insights-resources"></a>用于查询多个 Application Insights 资源的建议做法 
在查询中列出多个 Application Insights 资源可能很繁琐且难以维护。 可以改为利用函数将查询逻辑从应用程序范围中分离出来。  

本示例演示了如何监视多个 Application Insights 资源，并按应用程序名称显示失败请求的计数。

使用 union 运算符和应用程序列表创建函数，然后在工作区中使用别名 *applicationsScoping* 将该查询保存为函数。 

可以通过导航到工作区中的查询资源管理器，然后选择要编辑的函数并保存，或使用 `SavedSearch` PowerShell cmdlet 来随时修改列出的应用程序。 

>[!NOTE]
>此方法不能用于日志警报，因为警报规则资源（包括工作区和应用程序）的访问验证在创建警报时执行。 不支持在创建警报后将新资源添加到该函数。 如果更喜欢使用函数在日志警报中确定资源范围，则需要在门户中编辑警报规则或使用资源管理器模板来更新范围内的资源。 或者，可以在日志警报查询中包含资源列表。

`withsource= SourceApp` 命令可向结果添加用于指定发送日志的应用程序的列。 本示例中的 parse 运算符为可选项，用于从 SourceApp 属性中提取应用程序名称。 

```
union withsource=SourceApp 
app('Contoso-app1').requests,  
app('Contoso-app2').requests, 
app('Contoso-app3').requests, 
app('Contoso-app4').requests, 
app('Contoso-app5').requests 
| parse SourceApp with * "('" applicationName "')" *  
```

现在便可以在跨资源查询中使用 applicationsScoping 函数：  

```
applicationsScoping 
| where timestamp > ago(12h)
| where success == 'False'
| parse SourceApp with * '(' applicationName ')' * 
| summarize count() by applicationName, bin(timestamp, 1h) 
| render timechart
```

虽然是在工作区中执行查询，但该查询使用 Application Insights 架构，因为 applicationsScoping 函数会返回 Application Insights 数据结构。 函数别名返回来自所有已定义应用程序的请求的并集。 然后，查询筛选失败的请求，并按应用程序显示趋势。

![跨查询结果示例](media/unify-app-resource-data/app-insights-query-results.png)

>[!NOTE]
>新的 [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) 支持日志警报中的[跨资源查询](../log-query/cross-workspace-query.md)。 默认情况下，除非从[旧版日志警报 API](../platform/alerts-log-api-switch.md#process-of-switching-from-legacy-log-alerts-api) 切换，否则 Azure Monitor 会使用[旧版 Log Analytics 警报 API](../platform/api-alerts.md) 从 Azure 门户创建新的日志警报规则。 切换之后，新的 API 成为 Azure 门户中新警报规则的默认设置，借助它可以创建跨资源查询日志警报规则。 可以使用 [scheduledQueryRules API 的 ARM 模板](../platform/alerts-log.md#log-alert-with-cross-resource-query-using-azure-resource-template)创建[跨资源查询](../log-query/cross-workspace-query.md)日志警报规则，而无需进行切换。但是，此警报规则可通过 [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) 进行管理，而不可通过 Azure 门户进行管理。

## <a name="application-insights-and-log-analytics-workspace-schema-differences"></a>Application Insights 和 Log Analytics 工作区的架构差异
下表显示了 Log Analytics 和 Application Insights 之间的架构差异。  

| Log Analytics 工作区属性| Application Insights 资源属性|
|------------|------------| 
| AnonUserId | user_id|
| ApplicationId | appId|
| ApplicationName | appName|
| ApplicationTypeVersion | application_Version |
| AvailabilityCount | itemCount |
| AvailabilityDuration | duration |
| AvailabilityMessage | message |
| AvailabilityRunLocation | location |
| AvailabilityTestId | id |
| AvailabilityTestName | name |
| AvailabilityTimestamp | timestamp |
| 浏览者 | client_browser |
| 城市 | client_city |
| ClientIP | client_IP |
| Computer | cloud_RoleInstance | 
| 国家/地区 | client_CountryOrRegion | 
| CustomEventCount | itemCount | 
| CustomEventDimensions | customDimensions |
| CustomEventName | name | 
| DeviceModel | client_Model | 
| DeviceType | client_type | 
| ExceptionCount | itemCount | 
| ExceptionHandledAt | handledAt |
| ExceptionMessage | message | 
| ExceptionType | type |
| OperationID | operation_id |
| OperationName | operation_Name | 
| 操作系统 | client_OS | 
| PageViewCount | itemCount |
| PageViewDuration | duration | 
| PageViewName | name | 
| ParentOperationID | operation_Id | 
| RequestCount | itemCount | 
| RequestDuration | duration | 
| RequestID | id | 
| RequestName | name | 
| RequestSuccess | success | 
| ResponseCode | resultCode | 
| 角色 | cloud_RoleName |
| RoleInstance | cloud_RoleInstance |
| SessionId | session_Id | 
| SourceSystem | operation_SyntheticSource |
| TelemetryType | type |
| URL | url |
| UserAccountId | user_AccountId |

## <a name="next-steps"></a>后续步骤

使用[日志搜索](../../azure-monitor/log-query/log-query-overview.md)可以查看 Application Insights 应用的详细信息。
