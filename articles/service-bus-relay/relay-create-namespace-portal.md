---
title: 使用 Azure 门户创建中继命名空间 | Microsoft Docs
description: 本文提供了一个演练，演示如何使用 Azure 门户创建中继命名空间。
services: service-bus-relay
documentationcenter: .net
author: lingliw
manager: digimobile
editor: ''
ms.assetid: 78ab6753-877a-4426-92ec-a81675d62a57
ms.service: service-bus-relay
ms.devlang: tbd
ms.topic: conceptual
ms.tgt_pltfrm: multiple
ms.workload: na
origin.date: 01/21/2020
ms.date: 2/6/2020
ms.author: v-lingwu
ms.openlocfilehash: edb227c606f8bd8c2ff37043653f8bc364278217
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77068381"
---
# <a name="create-a-relay-namespace-using-the-azure-portal"></a>使用 Azure 门户创建中继命名空间

命名空间是适用于所有 Azure 中继组件的作用域容器。 单个命名空间中可存在多个中继，命名空间通常用作应用程序容器。 目前有两种方法可用来创建中继命名空间：

1. Azure 门户（本文）。
2. [Azure 资源管理器](../azure-resource-manager/management/overview.md)模板。

## <a name="create-a-namespace-in-the-azure-portal"></a>在 Azure 门户中创建命名空间

[!INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

祝贺你！ 现已创建中继命名空间。

## <a name="next-steps"></a>后续步骤

* [中继常见问题](relay-faq.md)
* [.NET 入门](relay-hybrid-connections-dotnet-get-started.md)
* [节点入门](relay-hybrid-connections-node-get-started.md)

