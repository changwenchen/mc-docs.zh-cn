---
title: 在 HDInsight 中的 Apache Spark 上运行 Azure 机器学习工作负载
description: 了解如何在 Azure HDInsight 中的 Apache Spark 上使用自动化机器学习 (AutoML) 运行 Azure 机器学习工作负荷。
author: hrasheed-msft
ms.author: v-yiso
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
origin.date: 12/13/2019
ms.date: 01/13/2020
ms.openlocfilehash: 0c87767a8a7fb2e320056c84337d3228c2661293
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631507"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-on-apache-spark-in-hdinsight"></a>在 HDInsight 中的 Apache Spark 上使用自动化机器学习运行 Azure 机器学习工作负载

Azure 机器学习可简化和加速机器学习模型的生成、训练和部署。 在自动化机器学习 (AutoML) 中，一开始可以对带有定义的目标特征的数据进行训练，然后以迭代方式将算法和特征选择组合在一起，以便根据训练分数自动为数据选择最佳模型。 HDInsight 允许客户预配具有数百个节点的群集。 在 HDInsight 群集中的 Spark 上运行的 AutoML 允许用户在这些节点上使用计算容量，以横向扩展的方式运行训练作业，以及并行运行多个训练作业。 这样，用户便可以运行 AutoML 试验，同时将计算与其他大数据工作负载共享。

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>在 HDInsight 群集上安装 Azure 机器学习

有关自动化机器学习的一般教程，请参阅[教程：使用自动化机器学习生成回归模型](../../machine-learning/tutorial-auto-train-models.md)。
所有新的 HDInsight-Spark 群集都预装了 AzureML-AutoML SDK。 可以通过此[示例 Jupyter 笔记本](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-hdi)开始使用 AutoML on HDInsight。 此 Jupyter Notebook 演示如何使用自动化机器学习分类器来解决一个简单的分类问题。

> [!Note]
> Azure 机器学习包将安装到 Python3 conda 环境中。 应使用 PySpark3 内核运行安装的 Jupyter Notebook。

还可以通过 Zeppelin 笔记本来使用 AutoML。

> [!Note]
> Zeppelin 有一个[已知问题](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html)，即 PySpark3 不会选取适当版本的 Python。 请使用记录的解决方法。

## <a name="authentication-for-workspace"></a>工作区身份验证

创建工作区和提交试验需要使用身份验证令牌。 可以使用 [Azure AD 应用程序](../../active-directory/develop/app-objects-and-service-principals.md)生成此令牌。 如果未在帐户中启用多重身份验证，则还可以使用 [Azure AD 用户](/azure/python/python-sdk-azure-authenticate)来生成所需的身份验证令牌。  

以下代码片段使用 **Azure AD 应用程序**创建身份验证令牌。

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
    tenant_id='<Azure Tenant ID>',
    service_principal_id='<Azure AD Application ID>',
    service_principal_password='<Azure AD Application Key>'
)
```

以下代码片段使用 **Azure AD 用户**创建身份验证令牌。

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com', 'my_smart_password')
```

## <a name="loading-dataset"></a>加载数据集

Spark 上的自动化机器学习使用**数据流** - 针对数据执行的松散评估且不可变的操作。  数据流使用公共读取访问权限从 Blob 加载数据集，或使用 SAS 令牌从 Blob URL 加载数据集。

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(
    path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(
    path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

还可以使用一次性的注册将数据存储注册到到工作区。

## <a name="experiment-submission"></a>试验提交

在[自动化机器学习配置](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig)中，应设置属性 `spark_context`，使包在分布式模式下运行。 属性 `concurrent_iterations` 表示并行执行的最大迭代数，应设置为小于 Spark 应用的执行器核心数。

## <a name="next-steps"></a>后续步骤

* 有关自动化机器学习的动机详细信息，请参阅[使用 Microsoft 自动化机器学习逐步发布模型！](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/)
* 有关如何使用 Azure ML 自动化 ML 功能的详细信息，请参阅 [Azure 机器学习中的全新自动化机器学习功能](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/)
* [Microsoft Research 提供的 AutoML 项目](https://www.microsoft.com/research/project/automl/)