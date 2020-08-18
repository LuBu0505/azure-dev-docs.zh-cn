---
author: edburns
ms.author: edburns
ms.date: 08/09/2020
ms.openlocfilehash: e006f2ec9d6f0d3b2d83a6653d65da19489dc221
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052247"
---
### <a name="account-for-authentication-and-authorization"></a>用于身份验证和授权的帐户

大多数应用程序都有某种身份验证和授权。  如果使用 LDAP 进行身份验证，Azure 上的 WebLogic Server 支持自动集成。 市场套餐使用带安全 LDAP 的 Azure Active Directory 域服务 (Azure AD DS)。  该套餐会根据 Azure AD DS 中的数据创建 WebLogic Server 的默认领域。  有关详细信息，请参阅[用于将 WebLogic Server 上的 Java 应用迁移到 Azure 的最终用户授权和身份验证](../migrate-weblogic-with-aad-ldap.md)。
