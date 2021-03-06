---
title: 通知中心 TLS 更新
description: 了解 Azure 通知中心对 TLS 的支持。
services: notification-hubs
documentationcenter: .net
author: sethmanheim
manager: femila
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: multiple
ms.topic: article
origin.date: 04/07/2020
ms.date: 04/20/2020
ms.author: v-tawe
ms.reviewer: jowargo
ms.lastreviewed: 01/28/2020
ms.openlocfilehash: f43ce93ef5a3692596855ac04d32340a43668f34
ms.sourcegitcommit: 89ca2993f5978cd6dd67195db7c4bdd51a677371
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82588649"
---
# <a name="transport-layer-security-tls"></a>传输层安全 (TLS) (Transport Layer Security) (TLS)

为了确保更高级别的安全性，通知中心将于 2020 年 5 月 31 日（从 2020 年 4 月 30 日延长）禁用对 TLS 版本 1.0 和 1.1 的支持。 这些较旧的协议提供弱加密，容易遭受 BEAST 和 POODLE 攻击。 在运行 Android 5 或更高版本或 iOS 5 或更高版本的设备上部署的应用程序不受此更改影响，因为那些操作系统支持 TLS 1.2，并且客户端和服务器在连接时将协商双方都支持的最高协议版本。

我们建议你检查使用 Azure 通知中心的所有应用程序，以确保它们使用最合适的库和支持 TLS 1.2 的 TLS 堆栈。

## <a name="update-apps"></a>更新应用

你可以使用称为“应用传输安全性 (ATS)”的 Apple 网络安全功能确保你的 iOS 应用使用的是 TLS 1.2。 ATS 不能用于早于 iOS 9.0 或 macOS 10.11 的 SDK，你可以从 [Apple 的文档](https://developer.apple.com/documentation/security/preventing_insecure_network_connections)了解更多信息。

对于使用 SSLSocket 实例的 Android 应用程序，请在每个 SSLSocket 实例上设置启用的协议，如 [setEnabledProtocols](https://developer.android.com/reference/javax/net/ssl/SSLSocket#setEnabledProtocols(java.lang.String%5B%5D)) 中所述。

[TLS 协议兼容性](https://support.globalsign.com/customer/portal/articles/2934392-tls-protocol-compatibility)支持页面上的表可以帮助你在操作系统与兼容的 TLS 版本之间进行映射。

<!-- For more information, see the overview of the [support for TLS protocols on Windows](https://docs.microsoft.com/archive/blogs/kaushal/support-for-ssltls-protocols-on-windows). -->

## <a name="next-steps"></a>后续步骤

- [通知中心概述](notification-hubs-push-notification-overview.md)