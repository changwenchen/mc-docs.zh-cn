---
title: 收集必需参数
services: cognitive-services
author: IEvangelist
manager: nitinme
description: 所有认知服务的容器的参数
ms.service: cognitive-services
ms.topic: include
origin.date: 10/15/2019
ms.date: 02/26/2020
ms.author: dapine
ms.openlocfilehash: 6520f6e452737afce0a76da610b090e327f9584c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78154664"
---
## <a name="gathering-required-parameters"></a>收集必需参数

对于所有认知服务的容器来说，有三种主要参数是必需的。 最终用户许可协议 (EULA) 的值必须为 `accept`。 此外还需要终结点 URL 和 API 密钥。

### <a name="endpoint-uri-endpoint_uri"></a>终结点 URI `{ENDPOINT_URI}`

在相应的认知服务资源的 Azure 门户“概览”  页上提供*终结点* URI 值。 导航到“概览”  页，将鼠标悬停在“终结点”上就会显示一个 `Copy to clipboard` <span class="docon docon-edit-copy x-hidden-focus"></span> 图标。 复制后在需要时使用。

![收集终结点 URI 供以后使用](../media/overview-endpoint-uri.png)

### <a name="keys-api_key"></a>密钥 `{API_KEY}`

此密钥用于启动容器，可以从相应认知服务资源的 Azure 门户的“密钥”页获取。 导航到“密钥”页并单击  `Copy to clipboard` 图标。<span class="docon docon-edit-copy x-hidden-focus"></span>

![获取两个密钥中的一个供以后使用](../media/keys-copy-api-key.png)

> [!IMPORTANT]
> 这些订阅密钥用于访问认知服务 API。 不要共享你的密钥。 以安全方式存储密钥（例如，使用 Azure Key Vault 来存储）。 此外，我们建议定期重新生成这些密钥。 发出 API 调用只需一个密钥。 重新生成第一个密钥时，可以使用第二个密钥来持续访问服务。