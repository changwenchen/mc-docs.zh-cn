---
title: 模板中的参数
description: 介绍如何在 Azure 资源管理器模板中定义参数。
ms.topic: conceptual
origin.date: 09/05/2019
ms.author: v-yeche
ms.date: 01/06/2020
ms.openlocfilehash: ecd03dc2475237da8ca866df02a5a06d62b162c9
ms.sourcegitcommit: 6fb55092f9e99cf7b27324c61f5fab7f579c37dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/03/2020
ms.locfileid: "75631629"
---
# <a name="parameters-in-azure-resource-manager-templates"></a>Azure 资源管理器模板中的参数

本文介绍如何在 Azure 资源管理器模板中定义和使用参数。 为参数提供不同的值即可针对不同环境重复使用模板。

资源管理器会在启动部署操作之前解析参数值。 只要在模板中使用参数，资源管理器就会将其替换为解析的值。

## <a name="define-parameter"></a>定义参数

以下示例展示了一个简单的参数定义。 它定义名为 **storageSKU** 的参数。 此参数为字符串值，仅接受适合其预期用途的值。 如果在部署过程中未提供任何值时，则此参数使用默认值。

<!--Not Available on "Standard_ZRS"-->

```json
"parameters": {
  "storageSKU": {
    "type": "string",
    "allowedValues": [
      "Standard_LRS",
      "Standard_GRS",
      "Standard_RAGRS",
      "Premium_LRS"
    ],
    "defaultValue": "Standard_LRS",
    "metadata": {
      "description": "The type of replication to use for the storage account."
    }
  }
}
```

## <a name="use-parameter"></a>使用参数

在模板中，可以使用 [parameters](template-functions-deployment.md#parameters) 函数引用参数值。 在以下示例中，参数值用于设置存储帐户的 SKU。

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="template-functions"></a>模板函数

为参数指定默认值时，可以使用大多数模板函数。 可以使用另一个参数值生成默认值。 以下模板演示了如何以默认值的方式使用函数。 如果没有为站点提供名称，它会创建唯一的字符串值并将其追加到 **site**。 如果没有为主机计划提供名称，它会采用站点的值，并追加 **-plan**。

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

不能在 parameters 节中使用 [reference](template-functions-resource.md#reference) 函数或任何 [list](template-functions-resource.md#list) 函数。 在解析参数时，这些函数获取资源的运行时状态，不能在部署之前执行。

## <a name="objects-as-parameters"></a>用作参数的对象

将相关值作为对象传入可以更轻松地对其进行组织。 此方法还可以减少模板中的参数数目。

以下示例显示的参数是一个对象。 默认值显示对象的预期属性。

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "location": "chinaeast",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

可以使用点运算符引用对象的属性。

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location": "[parameters('VNetSettings').location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="example-templates"></a>示例模板

以下示例演示了使用参数的方案。

|模板  |说明  |
|---------|---------|
|[用于默认值的参数与函数](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | 演示在定义参数的默认值时如何使用模板函数。 该模板不部署任何资源。 它构造参数值并返回这些值。 |
|[参数对象](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | 演示如何使用参数的对象。 该模板不部署任何资源。 它构造参数值并返回这些值。 |

## <a name="next-steps"></a>后续步骤

* 若要了解参数的可用属性，请参阅[了解 Azure 资源管理器模板的结构和语法](template-syntax.md)。
* 若要了解如何以文件形式传入参数值，请参阅[创建资源管理器参数文件](parameter-files.md)。
* 有关创建参数的建议，请参阅[最佳做法 - 参数](template-best-practices.md#parameters)。

<!-- Update_Description: update meta properties, wording update, update link -->