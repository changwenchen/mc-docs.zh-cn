---
title: 在 Azure 中配置负载均衡器 TCP 空闲超时
titleSuffix: Azure Load Balancer
description: 本文介绍如何配置 Azure 负载均衡器 TCP 空闲超时。
services: load-balancer
documentationcenter: na
author: WenJason
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
origin.date: 01/09/2020
ms.date: 04/06/2020
ms.author: v-jay
ms.openlocfilehash: 815aee7b7d55b1460f2092b0ba72b718c1512a66
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "80625633"
---
# <a name="configure-tcp-idle-timeout-settings-for-azure-load-balancer"></a>为 Azure 负载均衡器配置 TCP 空闲超时设置

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

本文需要 Azure PowerShell 模块 5.4.1 或更高版本。 运行 `Get-Module -ListAvailable Az` 查找已安装的版本。 如果需要升级，请参阅[安装 Azure PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-Az-ps)。 如果在本地运行 PowerShell，则还需运行 `Connect-AzAccount -Environment AzureChinaCloud` 以创建与 Azure 的连接。

## <a name="tcp-idle-timeout"></a>TCP 空闲超时
Azure 负载均衡器的空闲超时设置为 4 分钟到 30 分钟。 默认情况下，它设置为 4 分钟。 如果处于非活动状态的时间超过超时值，则不能保证在客户端和云服务之间保持 TCP 或 HTTP 会话。

当连接关闭时，客户端应用程序可能收到以下错误消息：“基础连接已关闭: 应保持连接状态的连接已由服务器关闭”。

常见的做法是使用 TCP 保持连接状态。 这种做法可以将连接状态保持更长时间。 有关详细信息，请参阅 [.NET 示例](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx)。 在启用保持连接状态的情况下，在连接处于非活动状态时发送数据包。 用于保持连接状态的数据包将确保不会达到空闲超时值，并且连接会维持很长时间。

此设置仅适用于入站连接。 为了避免断开连接，请将 TCP 保持连接的时间间隔配置为小于空闲超时设置，或者提高空闲超时值。 为支持这些方案，已添加对可配置空闲超时的支持。

TCP 保持连接状态适用于不受电池使用时间约束的情况。 不建议将其用于移动应用程序。 在移动应用程序中使用 TCP 保持连接状态可能会加快设备电池的耗尽速度。

![TCP 超时](./media/load-balancer-tcp-idle-timeout/image1.png)

以下章节介绍了如何更改公共 IP 和负载均衡器资源的空闲超时设置。

## <a name="configure-the-tcp-timeout-for-your-instance-level-public-ip-to-15-minutes"></a>将实例级公共 IP 的 TCP 超时值配置为 15 分钟

```azurepowershell
$publicIP = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyResourceGroup
$publicIP.IdleTimeoutInMinutes = "15"
Set-AzPublicIpAddress -PublicIpAddress $publicIP
```

`IdleTimeoutInMinutes` 是可选项。 如果未设置，默认超时为 4 分钟。 可接受的超时范围为 4 到 30 分钟。

## <a name="set-the-tcp-timeout-on-a-load-balanced-rule-to-15-minutes"></a>将负载均衡规则的 TCP 超时设置为 15 分钟

若要为负载均衡器设置空闲超时，请在负载均衡规则上设置“IdleTimeoutInMinutes”。 例如：

```azurepowershell
$lb = Get-AzLoadBalancer -Name "MyLoadBalancer" -ResourceGroup "MyResourceGroup"
$lb | Set-AzLoadBalancerRuleConfig -Name myLBrule -IdleTimeoutInMinutes 15
```
## <a name="next-steps"></a>后续步骤

[内部负载均衡器概述](load-balancer-internal-overview.md)

[开始配置面向 Internet 的负载均衡器](quickstart-create-standard-load-balancer-powershell.md)

[配置负载均衡器分发模式](load-balancer-distribution-mode.md)
