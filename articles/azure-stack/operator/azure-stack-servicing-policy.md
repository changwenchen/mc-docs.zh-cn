---
title: Azure Stack Hub 服务策略
titleSuffix: Azure Stack Hub
description: 了解 Azure Stack Hub 服务策略以及如何使集成系统保持受支持状态。
author: WenJason
ms.topic: article
origin.date: 02/07/2020
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: harik
ms.lastreviewed: 01/11/2019
ms.openlocfilehash: 631320c74254a2d616998dd46286a3ca868b8edf
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "79290909"
---
# <a name="azure-stack-hub-servicing-policy"></a>Azure Stack Hub 服务策略

本文介绍 Azure Stack Hub 集成系统的服务策略，以及必须如何做才能使系统保持受支持状态。

## <a name="download-update-packages-for-integrated-systems"></a>下载集成系统的更新包

Microsoft 会发布完整的每月更新包和修补程序包来解决特定问题。

每月更新包托管在安全的 Azure 终结点中。 可以使用 [Azure Stack Hub 更新下载程序工具](https://aka.ms/azurestackupdatedownload)手动下载它们。 如果缩放单元已连接，则更新会以“可用更新”的形式自动显示在管理员门户中。  完整的每月更新包在每次发布时都会详细地进行记录。 有关每次发布的详细信息，可以单击本文[更新包发布频率](#update-package-release-cadence)部分的任何发布。

修补程序更新包托管在同一个安全的 Azure 终结点中。 可以使用每篇相应的修补程序知识库文章（例如，[Azure Stack Hub 修补程序 1.1809.12.114](https://support.microsoft.com/help/4481548/azure-stack-hotfix-1-1809-12-114)）中的嵌入式链接下载它们。 Azure Stack Hub 操作员可以按照[在 Azure Stack Hub 中应用更新](azure-stack-apply-updates.md)中的过程下载 .xml、.bin 和 .exe 文件并将其导入，这与操作完整的每月更新包类似。 使用已连接缩放单元的 Azure Stack Hub 操作员会看到修补程序自动出现在管理员门户中，并会看到消息“有可用更新”。 

如果缩放单元未连接，并且希望收到有关每个修补程序版本的通知，请订阅每个版本中记录的 [RSS](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss) 或 [ATOM](https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom) 源。

## <a name="update-package-types"></a>更新包类型

集成系统有两种类型的更新包：

- **Microsoft 软件更新**。 Microsoft 会负责 Microsoft 软件更新包的端到端服务生命周期。 这些包可以包括最新的 Windows Server 安全更新、非安全更新和 Azure Stack Hub 功能更新。 可以直接从 Microsoft 下载这些更新包。

- **OEM 硬件供应商提供的更新**。 Azure Stack Hub 硬件合作伙伴负责硬件相关固件和驱动程序更新包的端到端服务生命周期（包括指导）。 此外，对于硬件生命周期主机上的所有软件和硬件，Azure Stack Hub 硬件合作伙伴拥有并维护指导。 OEM 硬件供应商在自己的下载站点上托管这些更新包。

## <a name="update-package-release-cadence"></a>更新包发布频率

Microsoft 预期每月发布软件更新包。 但是，可能一个月内发布多个更新或没有任何更新。 OEM 硬件供应商会根据需要发布更新。

在[管理更新概述](azure-stack-updates.md)文档中，可以了解如何规划和管理更新，以及如何确定当前版本。

有关特定更新（包括其下载方法）的信息，请参阅该更新的发行说明：

- [Azure Stack Hub 1910 更新](/azure-stack/operator/release-notes?view=azs-1910)
- [Azure Stack Hub 1908 更新](/azure-stack/operator/release-notes?view=azs-1908)
- [Azure Stack Hub 1907 更新](/azure-stack/operator/release-notes?view=azs-1907)
- [Azure Stack Hub 1906 更新](/azure-stack/operator/release-notes?view=azs-1906)

## <a name="hotfixes"></a>修补程序

有时，Microsoft 会为 Azure Stack Hub 提供修补程序（通常是预防性或时效性的程序）来解决特定问题。 发布的每个修补程序都附带相应的 Microsoft 知识库文章，其中详细描述了问题、原因和解决方法。

可以像下载和安装普通的 Azure Stack Hub 完整更新包一样下载和安装修补程序。 但是，与完整更新不同，修补程序在几分钟内即可完成安装。 我们建议 Azure Stack Hub 操作员在安装修补程序时设置维护时段。 修补程序会更新 Azure Stack Hub 云的版本，使你可以轻松确定是否已应用该修补程序。 对于仍在支持期内的每个 Azure Stack Hub 版本，将单独提供修补程序。 特定迭代的每个修复程序是累积性的，包含同一版本的以往更新。 可以在相应的知识库文章中详细了解特定修补程序的适用性。 请参阅上一部分中的发行说明链接。

有关当前可用的修补程序的信息，请参阅该更新的发行说明：

- [Azure Stack Hub 1910 修补程序](/azure-stack/operator/release-notes?view=azs-1910#hotfixes)
- [Azure Stack Hub 1908 修补程序](/azure-stack/operator/release-notes?view=azs-1908#hotfixes-1)
- [Azure Stack Hub 1907 修补程序](/azure-stack/operator/release-notes?view=azs-1907#hotfixes-2)
- [Azure Stack Hub 1906 修补程序](/azure-stack/operator/release-notes?view=azs-1906#hotfixes-3)

## <a name="keep-your-system-under-support"></a>保持系统受支持

为了使 Azure Stack Hub 实例保持受支持的状态，该实例必须运行最新发布的更新版本或运行之前的两个更新版本之一。

修补程序不被视为主要更新版本。 如果 Azure Stack Hub 实例落后于“两个以上的更新”  ，则认为它不符合条件。 必须至少更新到最低支持版本才能获得支持。

例如，如果最新发布的更新版本为 1904，在此之前的两个更新包为版本 1903 和 1902，则 1902 和 1903 仍受支持， 但 1901 不受支持。 即使最近一到两个月没有发布任何版本，此策略也有效。 例如，如果最新版本为 1807，但没有版本 1806，则此前的两个更新包（1805 和 1804）仍受支持。

Microsoft 软件更新包是非累积性的，其先决条件是需要前一个更新包或修补程序。 如果决定延后一个或多个更新，如果想要获取最新版本，请考虑整体运行时。

## <a name="get-support"></a>获取支持

Azure Stack Hub 遵循与 Azure 相同的支持流程。 企业客户可以[创建 Azure 支持请求](https://support.azure.cn/zh-cn/support/support-azure/)。 如果你是云解决方案提供商 (CSP) 的客户，请联系 CSP 获得支持。 有关详细信息，请参阅 [Azure 支持常见问题解答](https://www.azure.cn/support/faq/)。

如果在排查更新问题时需要帮助，请参阅[排查 Azure Stack Hub 修补程序和更新问题的最佳做法](azure-stack-updates-troubleshoot.md)。

## <a name="next-steps"></a>后续步骤

- [在 Azure Stack Hub 中管理更新](azure-stack-updates.md)
- [排查 Azure Stack Hub 修补程序和更新问题的最佳做法](azure-stack-updates-troubleshoot.md)
