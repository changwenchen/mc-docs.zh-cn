---
title: 在 Azure Stack Hub 中更新 DNS 转发器
description: 了解如何在 Azure Stack Hub 中更新 DNS 转发器。
author: WenJason
ms.topic: conceptual
origin.date: 11/21/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: thoroet
ms.lastreviewed: 11/21/2019
ms.openlocfilehash: a5009df5995146e51b20f72dc7a037578d4a5443
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77541004"
---
# <a name="update-the-dns-forwarder-in-azure-stack-hub"></a>在 Azure Stack Hub 中更新 DNS 转发器

Azure Stack Hub 基础结构至少需要一个可访问的 DNS 转发器来解析外部名称。 必须提供 DNS 转发器才能部署 Azure Stack Hub。 该输入在 Azure Stack Hub 内部 DNS 服务器中用作转发器，并为身份验证、市场管理或使用情况等服务启用外部名称解析。

DNS 是一项可更改的关键数据中心基础结构服务，如果 DNS 更改，则必须更新 Azure Stack Hub。

本文介绍如何使用特权终结点 (PEP) 在 Azure Stack Hub 中更新 DNS 转发器。 建议使用两个可靠的 DNS 转发器 IP 地址。

1. 连接到[特权终结点](azure-stack-privileged-endpoint.md)。 请注意，不必通过打开支持票证来解锁特权终结点。

2. 运行以下命令，查看当前配置的 DNS 转发器。 或者，也可以使用管理门户区域属性：

   ```powershell
   Get-AzsDnsForwarder
   ```

3. 运行以下命令更新 Azure Stack Hub，以使用新的 DNS 转发器：

   ```powershell
    Set-AzsDnsForwarder -IPAddress "IPAddress 1","IPAddress 2"
   ```

4. 请查看命令输出中是否有错误。

## <a name="next-steps"></a>后续步骤

[防火墙集成](azure-stack-firewall.md)
