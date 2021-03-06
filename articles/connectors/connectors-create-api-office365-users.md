---
title: 在逻辑应用中添加 Office 365 用户连接器
description: 使用 REST API 参数的 Office 365 用户连接器概述
services: ''
documentationcenter: ''
author: MandiOhlinger
manager: anneta
editor: ''
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
origin.date: 08/18/2016
ms.author: v-yiso
ms.date: 03/26/2018
ms.openlocfilehash: 404984bc7a1ef605d19cfc95f696c63720261519
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78304696"
---
# <a name="get-started-with-the-office-365-users-connector"></a>Office 365 用户连接器入门
连接到 Office 365 用户，获取配置文件、搜索用户等。 通过 Office 365 用户，可以：

* 根据从 Office 365 用户中获取的数据生成业务流。 
* 使用获取直接下属、获取经理的用户配置文件等操作。 这些操作可获得响应，并使输出可用于其他操作。 例如，获取用户的直接下属，然后使用此信息更新 SQL Azure 数据库。 

若要立即开始创建逻辑应用，请参阅[创建逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

## <a name="create-a-connection-to-office-365-users"></a>创建到 Office 365 用户的连接
将此连接器添加到逻辑应用时，必须登录到 Office 365 用户帐户，并允许逻辑应用连接到帐户。

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

创建连接后，输入 Office 365 用户属性，如用户 ID。 本文中的 **REST API 参考**介绍了这些属性。

## <a name="connector-specific-details"></a>特定于连接器的详细信息

在[连接器详细信息](https://docs.microsoft.com/connectors/officeusers/)中查看在 Swagger 中定义的触发器和操作，并查看限制。

## <a name="more-connectors"></a>更多连接器
返回到 [API 列表](apis-list.md)。