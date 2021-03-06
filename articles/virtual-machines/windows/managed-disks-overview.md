---
title: Azure 托管磁盘简介
description: 概要介绍 Azure 托管磁盘，这些磁盘在使用 Azure Windows VM 时处理存储帐户
author: rockboyfor
ms.service: virtual-machines-windows
ms.topic: overview
origin.date: 12/02/2019
ms.date: 02/10/2020
ms.author: v-yeche
ms.subservice: disks
ms.openlocfilehash: b426f5e10a32ef395e5b4f8a31ea75ccbe8dcb1c
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77428650"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure 托管磁盘简介

Azure 托管磁盘是由 Azure 托管并与 Azure 虚拟机配合使用的块级存储卷。 托管磁盘类似于本地服务器中的物理磁盘，但它是虚拟化的。 使用托管磁盘时，只需指定磁盘大小、磁盘类型并预配磁盘即可。 预配此磁盘后，Azure 会处理剩余磁盘。

可用的磁盘类型包括高级固态硬盘 (SSD)、标准 SSD 和标准硬盘驱动器 (HDD)。 有关每种磁盘类型的信息，请参阅[选择适用于 IaaS VM 的磁盘类型](disks-types.md)。

<!--Not Available on Ultra Solid State Drives (SSD) (Preview)-->

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [选择适用于 IaaS VM 的磁盘类型](disks-types.md)

<!-- Update_Description: update meta properties, wording update -->