---
title: include 文件
description: include 文件
services: billing
author: rockboyfor
ms.service: cost-management-billing
ms.topic: include
origin.date: 07/22/2019
ms.date: 01/06/2020
ms.author: v-yeche
ms.custom: include file
ms.openlocfilehash: 492974fee354dfd2feffa57e598d9794108c7c2a
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "75631723"
---
<!--MOONCAKE: CORRECT ON https://www.azure.cn/pricing-->
<!--MOONCAKE: CORRECT ON /billing/billing-add-change-azure-subscription-administrator/-->

| 资源 | 默认限制 | 最大限制 |
| --- | --- | --- |
| 每个[订阅](https://www.azure.cn/pricing)的 VM 数 |每个区域 25,000 个<sup>1</sup>。 |每个区域 25,000 个。 |
| 每个[订阅](https://www.azure.cn/pricing)的 VM 核心总数 |每个区域 20 个<sup>1</sup> | 联系支持人员。 |
| VM 系列（例如 Dv2 和 F）、每个[订阅](https://www.azure.cn/pricing)的核心数 |每个区域 20 个<sup>1</sup> | 联系支持人员。 |
| 每个订阅的[协同管理员数](/billing/billing-add-change-azure-subscription-administrator/) |不受限制。 |不受限制。 |
| 每个订阅在每个区域中的[存储帐户数](../articles/storage/common/storage-quickstart-create-account.md) |250 |250 |
| 每个订阅的[资源组数](../articles/azure-resource-manager/resource-group-overview.md) |980 |980 |
| 每个订阅的[可用性集数](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) |每个区域 2,000 个。 |每个区域 2,000 个。 |
| Azure 资源管理器 API 请求大小 |4,194,304 字节。 |4,194,304 字节。 |
| 每个订阅的标记数<sup>2</sup> |不受限制。 |不受限制。 |
| 每个订阅的唯一标记计算<sup>2</sup> | 10,000 | 10,000 |
| 每个订阅的[云服务数](../articles/cloud-services/cloud-services-choose-me.md) |不适用<sup>3</sup> |不适用<sup>3</sup> |
| 每个订阅的[地缘组数](../articles/virtual-network/virtual-networks-migrate-to-regional-vnet.md) |不适用<sup>3</sup> |不适用<sup>3</sup> |
| 每个位置的[订阅级部署数](../articles/azure-resource-manager/deploy-to-subscription.md) | 800<sup>4</sup> | 800 |

<sup>1</sup>默认限制因套餐类别类型（例如试用和标准预付费套餐）以及系列（例如 Dv2 和 F）而异。

<!--Not Available on  For example, the default for Enterprise Agreement subscriptions is 350-->

<sup>2</sup>每个订阅可以应用无限数量的标记。 每个资源或资源组的标记数限制为 50。 当标记数少于或等于 10,000 时，资源管理器仅返回订阅中[唯一标记名和值的列表](https://docs.microsoft.com/rest/api/resources/tags)。 即使数目超过 10,000，也仍可按标记查找资源。 

<sup>3</sup>使用 Azure 资源组和资源管理器时不再需要这些功能。

<sup>4</sup>如果达到部署数限制 800，则会从历史记录中删除不再需要的部署。 若要删除订阅级别部署，请使用 [Remove-AzDeployment](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzDeployment) 或 [az deployment delete](https://docs.azure.cn/cli/deployment?view=azure-cli-latest#az-deployment-delete)。

> [!NOTE]
> 虚拟机核心数存在区域总数限制。 区域大小系列（例如 Dv2 和 F）也存在限制。这些限制是单独实施的。 例如，假设某个订阅在中国东部的 VM 核心数限制为 30 个，即 A 系列的核心数限制为 30，D 系列的核心数限制也为 30。 此订阅可以部署 30 个 A1 VM、30 个 D1 VM，或两者的组合，但总共不能超过 30 个核心。 例如，10 个 A1 VM 和 20 个 D1 VM 就是一种组合。  
> 
>

<!-- Update_Description: update meta properties, wording update, update link -->