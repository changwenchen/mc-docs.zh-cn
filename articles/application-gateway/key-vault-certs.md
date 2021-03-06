---
title: 使用 Azure Key Vault 证书进行 TLS 终止
description: 了解如何将 Azure 应用程序网关与 Key Vault 集成，以便存储附加到支持 HTTPS 的侦听器的服务器证书。
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/26/2020
ms.author: v-junlch
ms.openlocfilehash: 37f1ed5353bed1bb37ad3a2d65d5e31f97e40425
ms.sourcegitcommit: e3512c5c2bbe61704d5c8cbba74efd56bfe91927
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "82267694"
---
# <a name="tls-termination-with-key-vault-certificates"></a>使用 Key Vault 证书进行 TLS 终止

[Azure Key Vault](../key-vault/key-vault-overview.md) 是平台托管的机密存储，可以用来保证机密、密钥和 TLS/SSL 证书的安全。 Azure 应用程序网关支持与密钥保管库集成，以存储附加到支持 HTTPS 的侦听器的服务器证书。 此支持仅限 v2 SKU 版应用程序网关。

Key Vault 集成提供了两种用于 TLS 终止的模型：

- 你可以显式提供附加到侦听器的 TLS/SSL 证书。 此模型是将 TLS/SSL 证书传递到应用程序网关进行 TLS 终止的传统方式。
- 可以选择在创建支持 HTTPS 的侦听器时提供对现有 Key Vault 证书或机密的引用。

应用程序网关与 Key Vault 集成具有许多优势，其中包括：

- 更高的安全性，因为 TLS/SSL 证书不直接由应用程序开发团队处理。 集成允许独立的安全团队执行以下操作：
  * 设置应用程序网关。
  * 控制应用程序网关生命周期。
  * 授权选定的应用程序网关访问存储在密钥保管库中的证书。
- 支持将现有证书导入密钥保管库中。 或者使用 Key Vault API 与任何受信任的 Key Vault 合作伙伴一起创建并管理新证书。
- 支持自动续订存储在密钥保管库中的证书。

应用程序网关目前仅支持经软件验证的证书。 不支持硬件安全模块 (HSM) 验证的证书。 在应用程序网关在配置为使用 Key Vault 证书以后，其实例会从 Key Vault 检索证书并将其安装到本地进行 TLS 终止。 实例还按 24 小时的时间间隔轮询 Key Vault，以便检索续订版的证书（如果存在）。 如果发现了更新的证书，则目前与 HTTPS 侦听器关联的 TLS/SSL 证书会自动轮换。

> [!NOTE]
> Azure 门户仅支持 KeyVault 证书，而不支持机密。 应用程序网关仍支持从 KeyVault 引用机密，而只能通过 PowerShell、CLI、API、ARM 模板等非门户资源进行引用。 

## <a name="how-integration-works"></a>集成工作原理

应用程序网关与 Key Vault 集成需要一个三步配置过程：

1. **创建用户分配的托管标识**

   你创建或重用现有的用户分配的托管标识，供应用程序网关用来代表你从 Key Vault 检索证书。 有关详细信息，请参阅[什么是 Azure 资源的托管标识？](../active-directory/managed-identities-azure-resources/overview.md)。 这一步在 Azure Active Directory 租户中创建新标识。 此标识受那个用来创建标识的订阅的信任。

1. **配置密钥保管库**

   然后导入现有的证书，或者在密钥保管库中创建新证书。 此证书将供通过应用程序网关的应用程序使用。 在此步骤中，也可使用密钥保管库机密，该机密将存储为无密码的 base-64 编码的 PFX 文件。 我们建议使用证书类型是因为适用于密钥保管库中证书类型对象的自动续订功能。 在创建证书或机密以后，即可在密钥保管库中定义访问策略，此类策略允许为标识授予对机密的“获取”  访问权限。
   
   > [!NOTE]
   > 如果通过 ARM 模板来部署应用程序网关（不管是使用 Azure CLI 还是使用 PowerShell），或通过从 Azure 门户部署的 Azure 应用程序来执行此操作，则以 base-64 编码 PFX 文件形式存储在密钥保管库中的 SSL 证书必须无密码  。 另外，还必须完成[在部署过程中使用 Azure Key Vault 传递安全参数值](../azure-resource-manager/templates/key-vault-parameter.md)中的步骤。 将 `enabledForTemplateDeployment` 设置为 `true` 尤其重要。

1. **配置应用程序网关**

   在完成前面的两个步骤以后，即可设置或修改现有的应用程序网关，以便使用用户分配的托管标识。 你还可以配置 HTTP 侦听器的 TLS/SSL 证书，使之指向 Key Vault 证书或机密 ID 的完整 URI。

   ![密钥保管库证书](./media/key-vault-certs/ag-kv.png)

## <a name="next-steps"></a>后续步骤

[通过 Azure PowerShell 配置使用 Key Vault 证书的 TLS 终止](configure-keyvault-ps.md)

