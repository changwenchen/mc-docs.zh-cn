---
title: 排查 Azure 中的 Linux 虚拟机部署问题 | Azure
description: 排查 Azure 资源管理器部署模型中的 Linux 虚拟机部署问题。
services: virtual-machines-windows
documentationcenter: ''
author: rockboyfor
manager: digimobile
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
origin.date: 11/01/2018
ms.date: 11/11/2019
ms.author: v-yeche
ms.openlocfilehash: 333a30849f8c0a0da0d3405a9e0ad9ed7b45bb62
ms.sourcegitcommit: b469d275694fb86bbe37a21227e24019043b9e88
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/30/2020
ms.locfileid: "82596331"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>排查 Azure 中的 Linux 虚拟机部署问题

若要排查 Azure 中虚拟机 (VM) 的部署问题，请查看[热门问题](#top-issues)了解常见故障和解决方法。

如果对本文中的任何观点存在疑问，可以联系 [Azure 支持](https://support.azure.cn/support/contact/)上的 Azure 专家。 或者，你也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/support/support-azure/)提交请求。

## <a name="top-issues"></a>常见问题
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>群集无法支持请求的 VM 大小
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- 以更小的 VM 大小重试请求。
- 如果无法更改请求的 VM 大小：
    - 停止可用性集中的所有 VM。 依次单击“资源组”  > 资源组 >“资源”  > 可用性集 >“虚拟机”  > 虚拟机 >“停止”  。
    - 在所有 VM 都停止后，创建相应大小的 VM。
    - 先启动新 VM，再选择所有已停止的 VM 并单击“启动”。

## <a name="the-cluster-does-not-have-free-resources"></a>群集没有可用资源
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- 请稍后重试请求。
- 如果新 VM 属于不同的可用性集
    - 在同一区域的其他可用性集中创建 VM。
    - 将新 VM 添加到同一虚拟网络。

<!--Not Available on BizSpart, GPU, N-Series VB -->
## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>为什么我无法为 Ubuntu NV VM 安装 GPU 驱动程序？

目前，仅在运行 Ubuntu Server 16.04 LTS 的 Azure NC VM 上提供 Linux GPU 支持。 有关详细信息，请参阅[在运行 Linux 的 N 系列 VM 上安装 GPU 驱动程序](../linux/n-series-driver-setup.md)。

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>我的 Linux N 系列 VM 缺少驱动程序

有关 Linux VM 的驱动程序，请单击[此处](../linux/n-series-driver-setup.md)。 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>在我的 N 系列 VM 中找不到 GPU 实例

若要利用运行 Windows Server 2016 或 Windows Server 2012 R2 的 Azure N 系列 VM 的 GPU 功能，在部署后必须在每个 VM 上安装 NVIDIA 图形驱动程序。 针对 [Windows VM](../windows/n-series-driver-setup.md) 和 [Linux VM](../linux/n-series-driver-setup.md) 提供了驱动程序安装信息。

## <a name="is-n-series-vms-available-in-my-region"></a>我所在的地区是否提供 N 系列 VM？

可以从[可用产品(按区域)](https://status.azure.com/status/) 以及[此处](https://www.azure.cn/pricing/details/virtual-machines)的定价来查看可用性。

<!--MOONCAKE: CORRECT ON [Products available by region table](https://status.azure.com/status/)-->

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>在调整 VM 的大小时，我看不到我需要的 VM 大小系列。

运行的 VM 部署到物理服务器。 Azure 区域中的物理服务器被分入常见物理硬件群集中。 需要将 VM 移到其他硬件群集才能重设 VM 大小，具体操作因部署 VM 所用部署模型而异。

- 对于在经典部署模型中部署的 VM，必须删除并重新部署云服务部署，才能将 VM 大小更改为其他大小系列。

- 对于在 Resource Manager 部署模型中部署的 VM，必须先停止可用性集中的所有 VM，才能更改可用性集中任何 VM 的大小。

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>在可用性集中部署时，列出的 VM 大小不受支持。

请选择可用性集的群集支持的大小。 建议在创建可用性集时，选择所需要的最大 VM 大小，并将其作为到可用性集的第一次部署。

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Azure 上支持哪些 Linux 发行版本/版本？

有关列表，可以参阅 [Azure 认可的 Linux 发行版本](../linux/endorsed-distros.md)。

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>是否可以将现有的经典 VM 添加到可用性集？

是的。 可以将现有经典 VM 添加到新的或现有的可用性集。 有关详细信息，请参阅[将现有虚拟机添加到可用性集](https://docs.microsoft.com/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine)。

## <a name="next-steps"></a>后续步骤
如果对本文中的任何观点存在疑问，可以联系 [Azure 支持](https://support.azure.cn/support/contact/)上的 Azure 专家。

或者，你也可以提出 Azure 支持事件。 请转到 [Azure 支持站点](https://support.azure.cn/support/support-azure/)提交请求。

<!--Update_Description: wording update, update link  -->
