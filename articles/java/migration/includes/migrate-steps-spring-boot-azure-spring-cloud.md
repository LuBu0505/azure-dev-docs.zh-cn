---
author: yevster
ms.author: yebronsh
ms.date: 8/25/2020
ms.openlocfilehash: 7970cae2b9ac39f7012e74b1334d12b2c5006af0
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062040"
---
### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>创建 Azure Spring Cloud 实例和应用

在 Azure 订阅中预配 Azure Spring Cloud 实例（如果尚未存在）。 然后在那里创建应用程序。 有关详细信息，请参阅[快速入门：使用 Azure 门户启动现有 Azure Spring Cloud 应用程序](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](configure-persistent-storage-azure-spring-cloud.md)]

[!INCLUDE [migrate-all-certificates-to-keyvault-azure-spring-cloud](migrate-all-certificates-to-keyvault-azure-spring-cloud.md)]

### <a name="remove-application-performance-management-apm-integrations"></a>删除应用程序性能管理 (APM) 集成

消除与 APM 工具/代理的任何集成。 有关使用 Azure Monitor 配置性能管理的信息，请参阅[迁移后](#post-migration)部分。

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>在应用程序中禁用指标客户端和终结点

删除应用程序中使用的任何指标客户端或应用程序中公开的任何指标终结点。

### <a name="deploy-the-application"></a>部署应用程序

部署每个已迁移的微服务（不包括 Spring Cloud Config 和注册表服务器），请参见[快速入门：使用 Azure 门户启动现有 Azure Spring Cloud 应用程序](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)。

### <a name="configure-per-service-secrets-and-externalized-settings"></a>配置基于服务的机密和外部化设置

可以将任何基于服务的配置设置作为环境变量注入每个服务。 在 Azure 门户中，使用以下步骤：

1. 导航到 Azure Spring Cloud 实例，然后选择“应用”。
1. 选择要配置的服务。
1. 选择“配置”。
1. 输入要配置的变量。
1. 选择“保存”。

![Spring Cloud 应用配置设置](../media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

### <a name="migrate-and-enable-the-identity-provider"></a>迁移和启用标识提供者

如果任何 Spring Cloud 应用程序需要身份验证或授权，请确保将其配置为访问标识提供者：

* 如果标识提供者是 Azure Active Directory，则不需要进行任何更改。
* 如果标识提供者是本地 Active Directory 林，请考虑使用 Azure Active Directory 实现混合标识解决方案。 有关详细信息，请参阅[混合标识文档](/azure/active-directory/hybrid/)。
* 如果标识提供者是另一个本地解决方案（如 PingFederate），请参阅 [Azure AD Connect 的自定义安装](/azure/active-directory/hybrid/how-to-connect-install-custom)主题，以使用 Azure Active Directory 配置联合。 或者，考虑使用 Spring 安全性通过 [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) 或 [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 来使用标识提供者。

### <a name="expose-the-application"></a>公开应用程序

默认情况下，部署到 Azure Spring Cloud 的应用程序在外部不可见。 可通过以下命令公开应用程序：

```azurecli
az spring-cloud app update -n <application name> --is-public true
```

如果你正在使用或打算使用 Spring Cloud 网关（在下一节中详细介绍），请跳过此步骤。
