---
title: 适用于 Azure Functions 的 Azure IoT 中心绑定
description: 了解如何在 Azure Functions 中使用 IoT 中心触发器和绑定。
author: craigshoemaker
ms.topic: reference
ms.date: 03/02/2020
ms.author: v-junlch
ms.openlocfilehash: 2420b840adcca88e3d740f092e0593885f8afbf1
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "78266048"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>适用于 Azure Functions 的 Azure IoT 中心绑定

此系列文章介绍如何使用 IoT 中心的 Azure Functions 绑定。 IoT 中心支持基于 [Azure 事件中心绑定](functions-bindings-event-hubs.md)。

> [!IMPORTANT]
> 虽然下述代码示例使用事件中心 API，但给定的语法适用于 IoT 中心函数。

| 操作 | 类型 |
|--------|------|
| 响应发送到 IoT 中心事件流的事件。 | [触发器](./functions-bindings-event-iot-trigger.md) |
| 将事件写入 IoT 事件流 | [输出绑定](./functions-bindings-event-iot-output.md) |

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>后续步骤

- [响应发送到事件中心事件流的事件（触发器）](./functions-bindings-event-iot-trigger.md)
- [将事件写入事件流（输出绑定）](./functions-bindings-event-iot-output.md)

