---
title: B 系列可突增 - Azure 虚拟机
description: 描述 B 系列可突增 Azure VM 大小。
services: virtual-machines
author: rockboyfor
manager: digimobile
ms.service: virtual-machines
ms.topic: article
origin.date: 02/03/2020
ms.date: 03/09/2020
ms.author: v-yeche
ms.openlocfilehash: 7d219260000c4c6b5defd314432ae5fc9bbe7712
ms.sourcegitcommit: e94ed1c9eff4e88be2ca389909e60b14cc0d92f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79084518"
---
# <a name="b-series-burstable-virtual-machine-sizes"></a>B 系列可突增虚拟机大小

B 系列 VM 非常适合于并非持续需要 CPU 完全性能的工作负荷，例如 Web 服务器、概念证明、小型数据库和开发生成环境。 这些工作负荷通常具有可突增的性能要求。 B 系列使用户可以购买具有基准性能的 VM 大小，并且该 VM 实例在 CPU 使用率未达到其基线时会增加积分。 在虚拟机累积积分后，当应用程序需要更高的 CPU 性能时，虚拟机可以使用高达 100% vCPU 突增到基线以上。

B 系列有以下 VM 大小：

高级存储：支持

高级存储缓存：不支持

实时迁移：支持

内存保留更新：支持

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | VM 的基本 CPU 性能 | VM 的最大 CPU 性能 | 初始信用额度 | 累积的额度/小时 | 最大累积积分 | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/MBps | 最大非缓存磁盘吞吐量：IOPS/MBps | 最大 NIC 数 |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Standard_B1ls<sup>1</sup> | 1  | 0.5 | 4   | 5%   | 100%  | 30  | 3   | 72   | 2  | 200/10    | 160/10    | 2  |
| Standard_B1s              | 1  | 1   | 4   | 10%  | 100%  | 30  | 6   | 144  | 2  | 400/10    | 320/10    | 2  |
| Standard_B1ms             | 1  | 2   | 4   | 20%  | 100%  | 30  | 12  | 288  | 2  | 800/10    | 640/10    | 2  |
| Standard_B2s              | 2  | 4   | 8   | 40%  | 200%  | 60  | 24  | 576  | 4  | 1600/15   | 1280/15   | 3  |
| Standard_B2ms             | 2  | 8   | 16  | 60%  | 200%  | 60  | 36  | 864  | 4  | 2400/22.5 | 1920/22.5 | 3  |
| Standard_B4ms             | 4  | 16  | 32  | 90%  | 400%  | 120 | 54  | 1296 | 8  | 3600/35   | 2880/35   | 4  |
| Standard_B8ms             | 8  | 32  | 64  | 135% | 800%  | 240 | 81  | 1944 | 16 | 4320/50   | 4320/50   | 4  |
| Standard_B12ms            | 12 | 48  | 96  | 202% | 1200% | 360 | 121 | 2909 | 16 | 6480/75   | 4320/50   | 6  |
| Standard_B16ms            | 16 | 64  | 128 | 270% | 1600% | 480 | 162 | 3888 | 32 | 8640/100  | 4320/50   | 8  |
| Standard_B20ms            | 20 个 | 80  | 160 | 337% | 2000% | 600 | 203 | 4860 | 32 | 10800/125 | 4320/50   | 8  |

<sup>1</sup> 仅 Linux 支持 B1ls

## <a name="workload-example"></a>工作负荷示例

考虑一下办公签入/签出应用程序。 该应用程序在营业时间需要 CPU 突发，但在休息时间不需要太多计算力。 在此示例中，工作负荷要求使用 64GiB RAM 的 16vCPU 虚拟机才能有效工作。

下表显示每小时流量数据，而图表是该流量的视觉表示形式。

B16 特征：

最大 CPU 性能：16vCPU * 100% = 1600%

基线：270%

![每小时流量数据图表](./media/b-series-burstable/office-workload.png)

| 方案 | 时间 | CPU 使用率 (%) | 累积的积分<sup>1</sup> | 可用积分 |
| --- | --- | --- | --- | --- |
| B16ms 部署 | 部署 | 部署  | 480（初始信用额度） | 480 |
| 无流量 | 0:00 | 0 | 162 | 642 |
| 无流量 | 1:00 | 0 | 162 | 804 |
| 无流量 | 2:00 | 0 | 162 | 966 |
| 无流量 | 3:00 | 0 | 162 | 1128 |
| 无流量 | 4:00 | 0 | 162 | 1290 |
| 无流量 | 5:00 | 0 | 162 | 1452 |
| 低流量 | 6:00 | 270 | 0 | 1452 |
| 来到办公室的员工（应用需要 80% vCPU） | 7:00 | 1280 | -606 | 846 |
| 继续来到办公室的员工（应用需要 80% vCPU） | 8:00 | 1280 | -606 | 240 |
| 低流量 | 9:00 | 270 | 0 | 240 |
| 低流量 | 10:00 | 100 | 102 | 342 |
| 低流量 | 11:00 | 50 | 132 | 474 |
| 低流量 | 12:00 | 100 | 102 | 576 |
| 低流量 | 13:00 | 100 | 102 | 678 |
| 低流量 | 14:00 | 50 | 132 | 810 |
| 低流量 | 15:00 | 100 | 102 | 912 |
| 低流量 | 16:00 | 100 | 102 | 1014 |
| 签出的员工（应用需要 100% vCPU） | 17:00 | 1600 | -798 | 216 |
| 低流量 | 18:00 | 270 | 0 | 216 |
| 低流量 | 19:00 | 270 | 0 | 216 |
| 低流量 | 20:00 | 50 | 132 | 348 |
| 低流量 | 21:00 | 50 | 132 | 480 |
| 无流量 | 22:00 | 0 | 162 | 642 |
| 无流量 | 23:00 | 0 | 162 | 804 |

<sup>1</sup> 累积的积分/在一小时中使用的积分等于：`((Base CPU perf of VM - CPU Usage) / 100) * 60 minutes`。  

<!--Not CORRECT ON MOONCAKE B16ms is expensive that D16s_v3 -->
<!--D16s_v3: CNY4195.31     B16ms: CNY4678.57-->

## <a name="q--a"></a>问题解答

### <a name="q-how-do-you-get-135-baseline-performance-from-a-vm"></a>问：如何从 VM 获得 135% 的基线性能？

**答**：135% 的基线性能在组成 VM 大小的 8 个 vCPU 之间共享。 例如，如果应用程序使用 8 个核心中的 4 个进行批处理，并且这 4 个 vCPU 中的每个都以 30% 的利用率运行，那么 VM CPU 性能的总额就等于 120%。  这意味着 VM 将基于从基线性能起的 15% 增量来累积积分时间。  但它也意味着，如果有积分可用，则同一个 VM 可以使用所有 8 个 vCPU 的 100%，为该 VM 提供 800% 的最大 CPU 性能。

### <a name="q-how-can-i-monitor-my-credit-balance-and-consumption"></a>问：如何监视积分余额和消耗情况

**答**：我们将在未来几周内引入 2 个新指标，**积分**指标将允许用户查看 VM 已累积的积分，**已消耗的积分**指标将显示 VM 已从银行消耗的 CPU 积分数。    你将能够从门户的指标窗格中查看这些指标，也可以编程方式通过 Azure Monitor API 查看这些指标。

有关如何访问 Azure 指标数据的详细信息，请参阅 [Azure 中的指标概述](../azure-monitor/platform/data-platform.md)。

### <a name="q-how-are-credits-accumulated"></a>问：积分如何累积？

**答**：VM 累积和消耗率是这样设置的：完全在基本性能级别运行的 VM 既没有突增积分的净累积，也没有突增积分的净消耗。  每当在基本性能级别下运行时，VM 的积分都会净增加，而每当 VM 的 CPU 利用率高于其基本性能级别时，VM 的积分都将净减少。

**示例**：为小型时间和考勤数据库应用程序使用 B1ms 大小部署一个 VM。 此大小允许应用程序使用最多 20% 的 vCPU 作为基线，这是我每分钟可以使用或累积的 0.2 个积分。

应用程序在员工工作日（在 7:00-9:00 AM 和 4:00 - 6:00PM 之间）的开始和结束时间处于繁忙状态。 在一天的其他 20 小时内，应用程序通常处于空闲状态，仅使用 10% 的 vCPU。 对于非高峰小时，我每分钟获得 0.2 个积分，但每分钟仅消耗 0.l 个积分，因此我的 VM 将每小时累积 0.1 x 60 = 6 个积分。  对于非高峰的 20 小时，我将累积 120 个积分。  

在高峰小时内，应用程序的平均 vCPU 使用率为 60%，我仍每分钟获得 0.2 个积分，但我每分钟消耗 0.6 个积分，净成本为一分钟 0.4 个积分，或者每小时 0.4 x 60 = 24 个积分。 我每天有 4 小时的高峰使用期，因此我的高峰使用期成本为 4 x 24 = 96 个积分。

如果我用在非高峰时间获得的 120 个积分减去在高峰时间内使用的 96 个积分，我每天将累积可用于其他突发活动的 24 个积分。

### <a name="q-how-can-i-calculate-credits-accumulated-and-used"></a>问：如何计算累积的和使用的积分？

**答**：可以使用以下公式：

(VM 的基础 CPU 性能 - CPU 使用率) / 100 = 累积的积分或每分钟使用量

例如，在上面的实例中，基线是 20%，如果使用 10% 的 CPU，则累积 (20%-10%)/100 = 0.1 个积分/分钟。

### <a name="q-does-the-b-series-support-premium-storage-data-disks"></a>问：B 系列是否支持高级存储数据磁盘？

**答**：B 系列大小支持高级存储数据磁盘。

### <a name="q-why-is-my-remaining-credit-set-to-0-after-a-redeploy-or-a-stopstart"></a>问：重新部署或停止/启动后，剩余信用额度为什么设置为 0？

**答**：当 VM 状态为“已重新部署”并且 VM 移到另一节点时，累积的信用额度将丢失。 如果 VM 已停止/已启动，但仍保留在同一节点，VM 将保留累积的信用额度。 只要 VM 在节点上重新启动，它就会获取初始信用额度，对于 Standard_B8ms，该信用额度为 240 分钟。

### <a name="q-what-happens-if-i-deploy-an-unsupported-os-image-on-b1ls"></a>问：如果在 B1ls 上部署不受支持的 OS 映像，会发生什么情况？

**答**：B1ls 仅支持 Linux 映像，如果你部署任何其他 OS 映像，可能无法获得最佳的客户体验。

## <a name="other-sizes"></a>其他大小

- [常规用途](sizes-general.md)
- [计算优化](sizes-compute.md)
- [内存优化](sizes-memory.md)
    
    <!--Not Available on - [Storage optimized](sizes-storage.md)-->
    
- [GPU 优化](sizes-gpu.md)

    <!--Not Available on - [High performance compute](sizes-hpc.md)-->

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。

<!-- Update_Description: new article about sizes b series burstable -->
<!--NEW.date: 03/09/2020-->