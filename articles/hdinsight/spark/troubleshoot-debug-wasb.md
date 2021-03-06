---
title: 在 Azure HDInsight 中调试 WASB 文件操作
description: 介绍了与 Azure HDInsight 群集交互时出现的问题的故障排除步骤和可能的解决方法。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
origin.date: 02/18/2020
ms.date: 03/23/2020
ms.openlocfilehash: 98ad33b7c5cda47bda89375fff2af0f48bfb08ae
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79295935"
---
# <a name="debug-wasb-file-operations-in-azure-hdinsight"></a>在 Azure HDInsight 中调试 WASB 文件操作

有时候，你可能想了解 WASB 驱动程序从 Azure 存储启动了哪些操作。 对于客户端，WASB 驱动程序会在 **DEBUG** 级别为每个文件系统操作生成日志。 WASB 驱动程序使用 log4j 来控制日志记录级别，默认值为 **INFO** 级别。 有关 Azure 存储服务器端分析日志，请参阅 [Azure 存储分析日志记录](../../storage/common/storage-analytics-logging.md)。

生成的日志将如下所示：

```log
18/05/13 04:15:55 DEBUG NativeAzureFileSystem: Moving wasb://xxx@yyy.blob.core.chinacloudapi.cn/user/livy/ulysses.txt/_temporary/0/_temporary/attempt_20180513041552_0000_m_000000_0/part-00000 to wasb://xxx@yyy.blob.core.chinacloudapi.cn/user/livy/ulysses.txt/part-00000
```

## <a name="turn-on-wasb-debug-log-for-file-operations"></a>为文件操作启用 WASB 调试日志

1. 在 Web 浏览器中，导航到 `https://CLUSTERNAME.azurehdinsight.net/#/main/services/SPARK2/configs`，其中 `CLUSTERNAME` 是你的 Spark 群集的名称。

1. 导航到“advanced spark2-log4j-properties”  。

1. 将 `log4j.appender.console.Threshold=INFO` 修改为 `log4j.appender.console.Threshold=DEBUG`。

    1. 添加 `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`。

1. 导航到 **Advanced livy2-log4j-properties**。

    添加 `log4j.logger.org.apache.hadoop.fs.azure.NativeAzureFileSystem=DEBUG`。

1. 保存更改。

## <a name="additional-logging"></a>其他日志记录

上述日志应提供对文件系统操作的概要理解。 如果上述日志仍未提供有用的信息，或者要调查 blob 存储 API 调用，请将 `fs.azure.storage.client.logging=true` 添加到 `core-site` 中。 此设置将为 wasb 存储驱动程序启用 java sdk 日志，并打印对 blob 存储服务器的每次调用。 在调查后删除该设置，因为它可能会很快填满磁盘，并可能减慢进程。

如果后端是基于 Azure Data Lake 的，请为组件（例如 spark/tez/hdfs）使用以下 log4j 设置：

```
log4j.logger.com.microsoft.azure.datalake.store=ALL,adlsFile
log4j.additivity.com.microsoft.azure.datalake.store=true
log4j.appender.adlsFile=org.apache.log4j.FileAppender
log4j.appender.adlsFile.File=/var/log/adl/adl.log
log4j.appender.adlsFile.layout=org.apache.log4j.PatternLayout
log4j.appender.adlsFile.layout.ConversionPattern=%p\t%d{ISO8601}\t%r\t%c\t[%t]\t%m%n
```

在 `/var/log/adl/adl.log` 中的日志中查找日志。

## <a name="next-steps"></a>后续步骤

如果你的问题未在本文中列出，或者无法解决问题，请访问以下渠道以获取更多支持：


* 如果需要更多帮助，可以从 [Azure 门户](https://portal.azure.cn/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/)提交支持请求。
