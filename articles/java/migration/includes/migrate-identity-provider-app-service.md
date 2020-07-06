---
author: yevster
ms.author: yebronsh
ms.date: 5/19/2020
ms.openlocfilehash: 51ed106fde71e37467571baab2b327b092dddc15
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507621"
---
### <a name="migrate-and-enable-the-identity-provider"></a>迁移和启用标识提供者

如果应用程序需要身份验证或授权，请遵照以下指导将其配置为访问标识提供者：

* 如果标识提供者是 Azure Active Directory，则不需要进行任何更改。
* 如果标识提供者是本地 Active Directory 林，请考虑使用 Azure Active Directory 实现混合标识解决方案。 有关详细信息，请参阅[混合标识文档](/azure/active-directory/hybrid/)。
* 如果标识提供者是另一个本地解决方案（如 PingFederate），请参阅 [Azure AD Connect 的自定义安装](/azure/active-directory/hybrid/how-to-connect-install-custom)主题，以使用 Azure Active Directory 配置联合。 或者，考虑使用 Spring 安全性通过 [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) 或 [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 来使用标识提供者。
