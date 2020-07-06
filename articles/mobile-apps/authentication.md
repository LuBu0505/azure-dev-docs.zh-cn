---
title: 通过 Visual Studio App Center 和 Azure 服务向移动应用添加身份验证
description: 了解 Visual Studio App Center 等服务，这些服务可帮助设置用户身份验证，并使移动应用程序能够使用社交帐户、Azure Active Directory 和自定义身份验证进行身份验证。
author: codemillmatt
ms.assetid: 34a8a070-2222-4faf-9090-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 782123b4b567010a43ce72c622f7db5a65679db9
ms.sourcegitcommit: f02ff55cef47581303a217cf1d310879cd722237
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84632579"
---
# <a name="add-authentication-and-manage-user-identities-in-your-mobile-apps"></a>在移动应用中添加身份验证并管理用户标识

在应用程序中拥有用户及其行为的视图使开发人员可以通过为用户提供定制体验来更好地吸引用户。 无论你是为组织内的用户构建协作应用程序的应用程序开发人员，还是在创建下一个社交网络平台，你都需要一种方法来对用户进行身份验证并管理用户标识。 标识管理服务是移动后端服务最重要的功能之一。

使用以下服务可在移动应用中启用用户身份验证。

## <a name="azure-active-directory-b2c"></a>Azure Active Directory B2C

[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/) 是企业对消费者 (B2C) 标识管理服务，开发人员可以使用该服务对其客户进行身份验证。 通过此白色标签服务，开发人员可以自定义和控制用户如何安全地与其 Web、桌面、移动或单页应用程序交互。 用户可以使用 Azure AD B2C 注册、登录、重置密码和编辑个人资料。 Azure AD B2C 实现某种形式的 OpenID Connect 和 OAuth 2.0 协议。 

### <a name="azure-active-directory-b2c-features"></a>Azure Active Directory B2C 功能

- 借助客户首选的标识提供者以安全方式对客户进行身份验证。
- 管理客户标识和访问。
- 获取对社交媒体（如 Facebook、GitHub、Google、LinkedIn、Twitter、WeChat 和 Weibo）的登录支持。
- 使用行业标准协议（例如 OpenID Connect 或 SAML）连接到用户帐户，以便在各种平台上实现标识管理。
- 提供品牌注册和登录体验。
- 与 CRM 数据库、营销分析工具和帐户验证系统轻松集成。
- 捕获客户的登录、首选项和转换数据。

### <a name="azure-active-directory-b2c-references"></a>Azure Active Directory B2C 参考

- [Azure 门户](https://portal.azure.com/)
- [Azure AD B2C 文档](/azure/active-directory-b2c/)
- [快速入门](/azure/active-directory-b2c/active-directory-b2c-quickstarts-web-app)
- [示例](/azure/active-directory-b2c/code-samples)

## <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) 是 Microsoft 推出的基于云的标识和访问管理服务，可帮助员工登录及获取对以下内容的访问权限：

- 外部资源，例如 Microsoft Office 365、Azure 门户以及成千上万的其他软件即服务 (SaaS) 应用程序。
- 内部资源，例如公司网络和 Intranet 上的应用，以及由自己的组织开发的任何云应用。

### <a name="azure-active-directory-features"></a>Azure Active Directory 功能

- 通过将用户连接到其所需的应用程序，实现无缝且高度安全的访问。
- 基于用户、位置、设备、数据和应用程序上下文的全面标识保护和增强的标识和访问安全性。
- 适用于商用和自定义应用程序（如 Office 365、Salesforce.com 和 Box）的数千个预先集成的应用。
- 能够大规模管理访问权限。

### <a name="azure-active-directory-references"></a>Azure Active Directory 参考

- [Azure 门户](https://portal.azure.com/)
- [什么是 Azure AD？](/azure/active-directory/fundamentals/active-directory-whatis)
- [Azure Active Directory 入门](/azure/active-directory/fundamentals/active-directory-whatis)
- [快速入门](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)