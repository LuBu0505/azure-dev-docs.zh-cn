---
title: 将 Spring Boot 应用程序迁移到 Azure Spring Cloud
description: 本指南介绍在需要迁移现有 Spring Boot 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 7bc4a5188181f3b4b6d98b5308a5027a42bbac5e
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810580"
---
# <a name="migrate-spring-boot-applications-to-azure-spring-cloud"></a>将 Spring Boot 应用程序迁移到 Azure Spring Cloud

本指南介绍在需要迁移现有 Spring Boot 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

如果无法满足任何预迁移要求，请参阅以下伴随迁移指南：

* 将可执行的 JAR 应用程序迁移到 Azure Kubernetes 服务上的容器（按指南进行计划）
* 将可执行的 JAR 应用程序迁移到 Azure 虚拟机（按指南进行计划）

### <a name="inspect-application-components"></a>检查应用程序组件

[!INCLUDE [identify-local-state](includes/identify-local-state-azure-spring-cloud.md)]

[!INCLUDE [static-content-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>确定是否有服务包含特定于 OS 的代码

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

对于使用 Spring Boot 1.x 的任何应用程序，请按照 [Spring Boot 2.0 迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)将其更新为受支持的 Spring Boot 版本。 有关受支持的版本，请参阅[准备 Java Spring 应用以进行部署](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)。

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

### <a name="inventory-external-resources"></a>清点外部资源

标识外部资源，如数据源、JMS 消息代理和其他服务的 URL。 在 Spring Boot 应用程序中，通常可在通常名为 application.properties 或 application.yml 的文件的 src/main/directory 文件夹中找到此类资源的配置  。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

确定正在使用的一个或多个代理后，请找到相应的设置。 在 Spring Boot 应用程序中，通常可以在应用程序目录的 application.properties 和 application.yml 文件中找到它们 。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-identity-providers-spring-boot.md](includes/inventory-identity-providers-spring-boot.md)]

#### <a name="identify-any-clients-relying-on-a-non-standard-port"></a>识别依赖于非标准端口的所有客户端

Azure Spring Cloud 将覆盖已部署的应用程序中的 `server.port` 设置。 如果客户的任何客户端依赖于 443 以外的端口上提供的应用程序，则需要对其进行修改。

#### <a name="all-other-external-resources"></a>所有其他的外部资源

本指南不可能记录每个可能的外部依赖项。 迁移后，你将负责验证你能否满足应用程序的所有外部依赖项的要求。

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

## <a name="migration"></a>迁移

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>创建 Azure Spring Cloud 实例和应用

在 Azure 订阅中预配 Azure Spring Cloud 实例（如果尚未存在）。 然后在那里创建应用程序。 有关详细信息，请参阅[快速入门：使用 Azure 门户启动现有 Azure Spring Cloud 应用程序](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-all-certificates-to-keyvault"></a>将所有证书迁移到 KeyVault

Azure Spring Cloud 不提供对 JRE 密钥存储的访问权限，因此必须将证书迁移到 Azure KeyVault，并更改应用程序代码，才能访问 KeyVault 中的证书。 有关详细信息，请参阅 [Key Vault 证书入门](/azure/key-vault/certificates/certificate-scenarios)和[适用于 Java 的 Azure Key Vault 证书客户端库](/java/api/overview/azure/security-keyvault-certificates-readme)。

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

![Spring Cloud 应用配置设置](media/migrate-spring-cloud-to-azure-spring-cloud/spring-cloud-app-configuration-settings.png)

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

## <a name="post-migration"></a>迁移后

现在你已完成迁移，请验证应用程序是否按预期运行。 然后可借助以下建议，提高应用程序的云原生性。

* 请考虑将应用程序与 Spring Cloud 注册表配合使用。 这将允许其他已部署的微服务和客户端动态发现你的应用程序。 有关详细信息，请参阅[教程：准备 Java Spring 应用以进行部署](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)。 然后修改任何应用程序客户端，以使用 Spring Client 负载均衡器。 这允许客户端获取应用程序的所有正在运行的实例地址，并查找在其他实例损坏或无响应时可以使用的实例。 有关详细信息，请参阅 Spring 博客中的 [Spring 使用技巧：Spring Cloud Loadbalancer](https://spring.io/blog/2020/03/25/spring-tips-spring-cloud-loadbalancer)。

* 请考虑添加 [Spring Cloud 网关](https://cloud.spring.io/spring-cloud-gateway/reference/html/)实例，而不是公开你的应用程序。 Spring Cloud 网关为 Azure Spring Cloud 实例中部署的所有应用程序/微服务提供单一终结点。 如果已部署 Spring Cloud 网关，请确保将其配置为将流量路由到新部署的应用程序。

* 请考虑添加一个 Spring Cloud Config 服务器来集中管理和版本控制所有 Spring Cloud 微服务的配置。 首先，创建一个 Git 存储库来容纳配置，并配置 Azure Spring Cloud 实例以使用该配置。 有关详细信息，请参阅[教程：为服务设置 Spring Cloud 配置服务器实例](/azure/spring-cloud/spring-cloud-tutorial-config-server)。 然后使用以下步骤迁移配置：

  1. 在配置 Git 存储库中创建一个目录，并使其名称与你在 Azure Spring Cloud 实例上定义的应用程序的名称相同。

  1. 在此目录中，创建一个包含以下内容的 bootstrap.yml 文件：

     ```yml
     spring:
       application:
         name: <Your the application name used in the previous step>
     ```

  1. 在上面的目录中创建一个 application.yml 文件，然后将应用程序设置移动到该目录中。 如果这些设置以前在 .properties 文件中，则需要将其转换为 YAML。

  1. 提交这些更改并将其推送到 Git 存储库。

* 请考虑为自动、一致的部署添加部署管道。 提供有关 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) 和 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) 的说明。

* 请考虑使用过渡部署在生产中测试代码更改，然后再将其提供给一些或所有最终用户。 有关详细信息，请参阅[在 Azure Spring Cloud 中设置过渡环境](/azure/spring-cloud/spring-cloud-howto-staging-environment)。

* 请考虑添加服务绑定，以便将应用程序连接到受支持的 Azure 数据库。 如果使用这些服务绑定，则无需向 Spring Cloud 应用程序提供连接信息（包括凭据）。

* 请考虑使用分布式跟踪和 Azure App Insights 来监视应用程序的性能和交互。 有关详细信息，请参阅[将分布式跟踪与 Azure Spring Cloud 配合使用](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing)。

* 请考虑添加 Azure Monitor 预警规则和操作组，以快速检测并解决异常情况。 有关详细信息，请参阅[教程：使用警报和操作组监视 Spring Cloud 资源](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)。

* 请考虑将 Azure Spring Cloud 部署复制到其他区域，以降低延迟，提高可靠性和容错。 使用 [Azure 流量管理器](/azure/traffic-manager)在部署之间实现负载均衡，或使用 [Azure Front Door](/azure/frontdoor) 添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。

* 如果不需要异地复制，请考虑添加 [Azure 应用程序网关](/azure/application-gateway)，以便添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。
