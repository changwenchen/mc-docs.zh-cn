---
title: Azure HDInsight：Python 示例
description: 针对用于 Python 的 HDInsight SDK，请在 GitHub 上查找有关使用它的常见任务的 Python 示例。
author: hrasheed-msft
ms.service: hdinsight
ms.topic: sample
origin.date: 11/08/2019
ms.date: 12/23/2019
ms.author: hrasheed
ms.openlocfilehash: aa5f6e31b16b8fbfdb80927b07fbf53e8d25dc06
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75335930"
---
# <a name="azure-hdinsight-python-samples"></a>Azure HDInsight：Python 示例

> [!div class="op_single_selector"]
> * [Python 示例](hdinsight-sdk-python-samples.md)
> * [.NET 示例](hdinsight-sdk-dotnet-samples.md)
> * [Java 示例](hdinsight-sdk-java-samples.md)
<!-- * [Go Examples](hdinsight-sdk-go-samples.md)-->

> [!Important]
> Python 2.7 将于 2020 年 1 月 1 日弃用。 如果你仍在使用 Python 2.7，请升级到 3.7 以使用 HDInsight Python SDK。  

本文提供：

* 有关群集创建任务的示例链接。
* 有关其他管理任务的参考内容链接。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>先决条件

[用于 Python 的 Azure HDInsight SDK](https://docs.microsoft.com/python/api/overview/azure/hdinsight#sdk-installation)

## <a name="cluster-management---creation"></a>群集管理 - 创建

* [创建 Kafka 群集](https://github.com/Azure-Samples/hdinsight-python-sdk-samples/blob/master/samples/create_kafka_cluster_sample.py)
* [创建 Spark 群集](https://github.com/Azure-Samples/hdinsight-python-sdk-samples/blob/master/samples/create_spark_cluster_sample.py)
* [创建包含 Azure Data Lake Storage Gen2 的 Spark 群集](https://github.com/Azure-Samples/hdinsight-python-sdk-samples/blob/master/samples/create_hadoop_cluster_with_adls_gen2_sample.py)
* [创建包含企业安全性套餐 (ESP) 的 Spark 群集](https://github.com/Azure-Samples/hdinsight-python-sdk-samples/blob/master/samples/create_esp_cluster_sample.py)

通过克隆 [hdinsight-python-sdk-samples](https://github.com/Azure-Samples/hdinsight-python-sdk-samples) GitHub 存储库，可获取 Python 的这些示例。

[!INCLUDE [hdinsight-sdk-additional-functionality](../../includes/hdinsight-sdk-additional-functionality.md)]

在[用于 Python 的 HDInsight SDK 参考文档](https://docs.microsoft.com/python/api/overview/azure/hdinsight?view=azure-python)中，可找到这一附加 SDK 功能的代码片段。