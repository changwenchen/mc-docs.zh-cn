---
title: 管理员管理用户和设备 - Azure MFA - Azure Active Directory
description: 管理员如何更改用户设置，例如，强制用户再次完成验证过程。
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
origin.date: 10/28/2019
ms.date: 11/12/2019
ms.author: v-junlch
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9a8b487832421639e30f041e632e7979dc3c843
ms.sourcegitcommit: c1ba5a62f30ac0a3acb337fb77431de6493e6096
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2020
ms.locfileid: "74084487"
---
# <a name="manage-user-settings-with-azure-multi-factor-authentication-in-the-cloud"></a>管理云中 Azure 多重身份验证的用户设置

作为管理员，可以管理以下用户和设备设置：

* 要求用户再次提供联系方式
* 删除应用密码
* 在所有信任的设备上要求 MFA

## <a name="manage-authentication-methods"></a>管理身份验证方法

作为被分配“身份验证管理员”角色的管理员，你可以要求用户重置其密码、重新注册 MFA，或者从其用户对象撤消现有的 MFA 会话。

![从 Azure 门户管理身份验证方法](./media/howto-mfa-userdevicesettings/manage-authentication-methods.png)

1. 登录 [Azure 门户](https://portal.azure.cn)。
1. 在左侧，选择“Azure Active Directory” **“用户”** “所有用户” >    >   。
1. 选择要在其上执行操作的用户，然后选择“身份验证方法”。 
   - “重置密码”会重置用户的密码并分配一个必须在下次登录时更改的临时密码。 
   - “要求重新注册 MFA”在生效后，会请求用户在下次登录时设置一个新的 MFA 身份验证方法。 
   - “撤消 MFA 会话”会清除用户的被系统记住的 MFA 会话，并要求用户下一次登录时执行 MFA。这是设备上的策略要求的。 

## <a name="delete-users-existing-app-passwords"></a>删除用户现有的应用密码

此设置会删除用户创建的所有应用密码。 与这些应用密码关联的非浏览器应用将会停止工作，直到创建新应用密码为止。 需要全局管理员权限才能执行此操作。

### <a name="how-to-delete-users-existing-app-passwords"></a>如何删除用户现有的应用密码

1. 登录 [Azure 门户](https://portal.azure.cn)。
2. 在左侧，选择“Azure Active Directory” **“用户”** “所有用户” >    >   。
3. 在右侧，选择工具栏上的“多重身份验证”  。 多重身份验证页面将打开。
4. 选中要管理的用户或用户旁的框。 右侧会显示快速步骤选项列表。
5. 选择“管理用户设置”  。
6. 选中“删除选定用户生产的所有现有应用密码”框  。
   ![删除所有现有的应用密码](./media/howto-mfa-userdevicesettings/deleteapppasswords.png)
7. 单击“保存”  。
8. 单击“关闭”  。

## <a name="next-steps"></a>后续步骤

- 获取有关如何[配置 Azure 多重身份验证设置](howto-mfa-mfasettings.md)的详细信息
- 如果用户需要帮助，可让其参阅[双重验证用户指南](../user-help/multi-factor-authentication-end-user.md)

<!-- Update_Description: wording update -->