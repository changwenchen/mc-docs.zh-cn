---
title: Azure Stack Hub 上的应用服务 Update 7 发行说明
description: 了解基于 Azure Stack Hub 的应用服务 Update 7 的功能、已知问题和更新下载位置。
author: WenJason
manager: digimobile
ms.topic: article
origin.date: 10/11/2019
ms.date: 02/24/2020
ms.author: v-jay
ms.reviewer: ''
ms.openlocfilehash: 368a26c3ac697cd688bdb71637af258906d03d57
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "77540958"
---
# <a name="app-service-on-azure-stack-hub-update-7-release-notes"></a>Azure Stack Hub 上的应用服务 Update 7 发行说明

这些发行说明介绍 Azure Stack Hub 上的 Azure 应用服务 Update 7 中的改进和修复，以及任何已知问题。 已知问题分为与部署、更新过程直接相关的问题，以及内部版本（安装后）的问题。

> [!IMPORTANT]
> 请将 1907 更新应用于 Azure Stack Hub 集成系统，或部署最新的 Azure Stack 开发工具包，然后部署 Azure 应用服务 1.7。


## <a name="build-reference"></a>内部版本参考

Azure Stack Hub 上的应用服务 Update 7 的内部版本号为 **84.0.2.10**。

### <a name="prerequisites"></a>先决条件

在开始部署之前，请参阅[准备工作文档](azure-stack-app-service-before-you-get-started.md)。

开始将 Azure Stack Hub 上的 Azure 应用服务升级到 1.7 之前：

- 确保所有角色在 Azure Stack Hub 管理门户的 Azure应用服务管理中处于“就绪”状态

- 备份应用服务和 Master 数据库：
  - AppService_Hosting；
  - AppService_Metering；
  - Master

- 备份租户应用内容文件共享

- 同步发布市场的**自定义脚本扩展**版本 **1.9.3**

### <a name="new-features-and-fixes"></a>新功能和修复

Azure Stack Hub 上的 Azure 应用服务 Update 7 包含以下改进和修复：

- [CVE-2019-1372](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-1372) 远程代码执行漏洞的解决方案

- 针对**应用服务租户、管理员、函数门户和 Kudu 工具**的更新。 与 Azure Stack Hub 门户 SDK 版本一致。

- 将 **Azure Functions 运行时**更新到 **v1.0.12582**。

- 针对核心服务的更新，用于提高可靠性和错误消息传递，以便更轻松地诊断常见问题。

- **针对以下应用程序框架和工具的更新**：
  - ASP.NET Core 2.2.46
  - Zul OpenJDK 8.38.0.13
  - Tomcat 7.0.94
  - Tomcat 8.5.42
  - Tomcat 9.0.21
  - PHP 5.6.40
  - PHP 7.3.6
  - 已将 Kudu 更新到 82.10503.3890

- **对所有角色的基础操作系统的更新**：
  - [适用于 x64 系统的 Windows Server 2016 的 2019-08 累积更新 (KB4512495)](https://support.microsoft.com/help/4512495)

- **访问限制现已在用户门户中启用**：
  - 从此发布版开始，用户可以根据已发布文档 [Azure 应用服务访问限制](/app-service/app-service-ip-restrictions)中的说明为其 Web/API/Functions 应用程序配置访问限制。**注意**：Azure Stack Hub 上的 Azure 应用服务不支持服务终结点。

- **部署选项（经典）功能已还原**：
  - 用户可以再次使用“部署选项(经典)”从 GitHub、Bitbucket、Dropbox、OneDrive、本地和外部存储库配置其应用的部署，以及为其应用程序设置“部署凭据”。

- **Azure 函数监视**已正确配置。

- **Windows 更新行为**：我们已根据用户反馈更改了通过 Update 7 在应用服务角色上对 Windows 更新进行配置的方式：
  - 三种模式：
    - **禁用** - Windows 更新服务处于禁用状态，Windows 将使用 KB 进行更新，该 KB 随附在基于 Azure Stack Hub 的 Azure 应用服务发布版中；
    - **自动** - Windows 更新服务处于启用状态，由 Windows 更新决定以何种方式在何时进行更新；
    - **托管** - Windows 更新服务处于禁用状态，Azure 应用服务会在对单个角色执行 OnStart 期间执行 Windows 更新循环。

  **新**部署 - Windows 更新服务默认禁用。

  **现有**部署 - 如果在“控制器”上修改了设置，则该值现在会从 **False** 更改为 **Disabled**，以前的值 **true** 会变为 **Automatic**

### <a name="post-deployment-steps"></a>部署后步骤

> [!IMPORTANT]
> 如果已经为应用服务资源提供程序提供 SQL Always On 实例，则必须[将 appservice_hosting 和 appservice_metering 数据库添加到可用性组](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/availability-group-add-a-database)并同步数据库，以免在进行数据库故障转移时丢失服务。

### <a name="known-issues-post-installation"></a>已知问题（安装后）

- 如 Azure Stack Hub 上的 Azure 应用服务部署文档中所述，当应用服务部署在现有虚拟网络中并且文件服务器仅在专用网络上可用时，工作人员将无法访问文件服务器。

如果选择部署到现有虚拟网络和内部 IP 地址以连接到文件服务器，则必须添加出站安全规则，以便在工作子网和文件服务器之间启用 SMB 流量。 转到管理门户中的 WorkersNsg 并添加具有以下属性的出站安全规则：
 * 源：任意
 * 源端口范围：*
 * 目标：IP 地址
 * 目标 IP 地址范围：文件服务器的 IP 范围
 * 目标端口范围：445
 * 协议：TCP
 * 操作：允许
 * 优先级：700
 * 姓名：Outbound_Allow_SMB445

### <a name="known-issues-for-cloud-admins-operating-azure-app-service-on-azure-stack-hub"></a>云管理员在操作基于 Azure Stack Hub 的 Azure 应用服务时的已知问题

请参阅 [Azure Stack Hub 1907 发行说明](azure-stack-release-notes-1907.md)中的文档

## <a name="next-steps"></a>后续步骤

- 有关 Azure 应用服务的概述，请参阅[基于 Azure Stack Hub 的 Azure 应用服务概述](azure-stack-app-service-overview.md)。
- 若要详细了解如何完成基于 Azure Stack Hub 的应用服务的部署准备，请参阅[基于 Azure Stack Hub 的应用服务的准备工作](azure-stack-app-service-before-you-get-started.md)。
