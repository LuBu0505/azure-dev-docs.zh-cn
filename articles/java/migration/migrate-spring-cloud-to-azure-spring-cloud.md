---
title: 将 Spring Cloud 应用程序迁移到 Azure Spring Cloud
description: 本指南介绍在需要迁移现有 Spring Cloud 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 2/12/2020
ms.custom: devx-track-java
ms.openlocfilehash: e4be32594940d2c207610e7d709ccb328c38ecf6
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831693"
---
# <a name="migrate-spring-cloud-applications-to-azure-spring-cloud"></a>将 Spring Cloud 应用程序迁移到 Azure Spring Cloud

本指南介绍在需要迁移现有 Spring Cloud 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

如果无法满足任何预迁移要求，请参阅以下伴随迁移指南：

* 将可执行的 JAR 应用程序迁移到 Azure Kubernetes 服务上的容器（按指南进行计划）
* 将可执行的 JAR 应用程序迁移到 Azure 虚拟机（按指南进行计划）

### <a name="inspect-application-components"></a>检查应用程序组件

[!INCLUDE [determine-whether-and-how-the-file-system-is-used-azure-spring-cloud](includes/determine-whether-and-how-the-file-system-is-used-azure-spring-cloud.md)]

#### <a name="determine-whether-any-of-the-services-contain-os-specific-code"></a>确定是否有服务包含特定于 OS 的代码

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [switch-to-a-supported-platform-azure-spring-cloud](includes/switch-to-a-supported-platform-azure-spring-cloud.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

对于使用 Spring Boot 1.x 的任何应用程序，请按照 [Spring Boot 2.0 迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)将其更新为受支持的 Spring Boot 版本。 有关受支持的版本，请参阅[准备 Java Spring 应用以进行部署](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)。

#### <a name="identify-spring-cloud-versions"></a>确定 Spring Cloud 版本

检查要迁移的每个应用程序的依赖项，以确定其使用的 Spring Cloud 组件的版本。

##### <a name="maven"></a>Maven

在 Maven 项目中，Spring Cloud 版本通常在 `spring-cloud.version` 属性中设置：

```xml
  <properties>
    <java.version>1.8</java.version>
    <spring-cloud.version>Hoxton.SR3</spring-cloud.version>
  </properties>
```

##### <a name="gradle"></a>Gradle

在 Gradle 项目中，Spring Cloud 版本通常在“额外属性”块中设置：

```gradle
ext {
  set('springCloudVersion', "Hoxton.SR3")
}
```

需要将所有应用程序更新为使用受支持的 Spring Cloud 版本。 有关受支持的版本列表，请参阅[准备 Java Spring 应用以进行部署](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#spring-boot-and-spring-cloud-versions)。

[!INCLUDE [identify-logs-metrics-apm-azure-spring-cloud.md](includes/identify-logs-metrics-apm-azure-spring-cloud.md)]

#### <a name="identify-zipkin-dependencies"></a>确定 Zipkin 依赖项

确定应用程序是否在 Zipkin 上具有显式依赖项。 在 Maven 或 Gradle 依赖项中查找 `io.zipkin.java` 组的依赖项。

### <a name="inventory-external-resources"></a>清点外部资源

标识外部资源，如数据源、JMS 消息代理和其他服务的 URL。 在 Spring Cloud 应用程序中，通常可以在以下位置之一找到此类资源的配置：

* 在 src/main/directory 文件夹中，位于通常称为 application.properties 或 application.yml 的文件中  。
* 在之前的步骤中标识的 Spring Cloud Config 存储库中。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

确定正在使用的一个或多个代理后，请找到相应的设置。 在 Spring Cloud 应用程序中，通常可以在应用程序目录的 application.properties 和 application.yml 中找到，或在 Spring Cloud Config 服务器存储库中找到 。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

#### <a name="identity-providers"></a>标识提供者

标识需要身份验证和/或授权的所有标识提供者和所有 Spring Cloud 应用程序。 有关可如何配置标识提供者的信息，请参阅以下内容：

* 有关 OAuth2 配置，请参阅 [Spring Cloud 安全性快速入门](https://cloud.spring.io/spring-cloud-security/2.1.x/multi/multi__quickstart.html#_quickstart)。
* 有关 Auth0 Spring 安全性配置，请参阅 [Auth0 Spring 安全性文档](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization)。
* 有关 PingFederate Spring 安全性配置，请参阅 [Auth0 PingFederate 说明](https://auth0.com/authenticate/java-spring-security/ping-federate/)。

#### <a name="resources-configured-through-vmware-tanzu-application-service-tas-formerly-pivotal-cloud-foundry"></a>通过 VMware Tanzu 应用程序服务（TAS，之前称为 Pivotal Cloud Foundry）配置的资源

对于使用 TAS 托管的应用程序，通常通过 TAS 服务绑定配置外部资源（包括前面所述的资源）。 若要检查此类资源的配置，请使用 [TAS (Cloud Foundry) CLI](https://docs.cloudfoundry.org/cf-cli/) 查看应用程序的 `VCAP_SERVICES` 变量。

```bash
# Log into TAS, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <organization name>
cf target space <space name>

# Display variables for the application
cf env <Application Name>
```

检查绑定到应用程序的外部服务配置设置的 `VCAP_SERVICES` 变量。 有关详细信息，请参阅 [TAS (Cloud Foundry) 文档](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES)。

#### <a name="all-other-external-resources"></a>所有其他的外部资源

本指南不可能记录每个可能的外部依赖项。 迁移后，你将负责验证你能否满足应用程序的所有外部依赖项的要求。

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-cloud](includes/inventory-configuration-sources-and-secrets-spring-cloud.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-cloud](includes/inspect-the-deployment-architecture-spring-cloud.md)]

## <a name="migration"></a>迁移

### <a name="remove-explicit-configuration-server-settings"></a>删除显式配置服务器设置

在要迁移的服务中，查找 Eureka 设置的任何显式分配，并将其删除。 此类设置通常出现在 application.properties 或 application.yml 文件中 ：

**application.yml**

```yaml
eureka:
  client:
    serviceUrl:
      defaultZone: http://myusername:mysecretpassword@localhost:8761/eureka/
```

如果应用程序配置中出现类似的设置，请将其删除。 Azure Spring Cloud 会自动注入其配置服务器的连接信息。

### <a name="create-an-azure-spring-cloud-instance-and-apps"></a>创建 Azure Spring Cloud 实例和应用

在 Azure 订阅中预配 Azure Spring Cloud 实例。 然后，为要迁移的每个服务预配一个应用。 请不要包含 Spring Cloud 注册表和配置服务器。 请包含 Spring Cloud 网关服务。 有关说明，请参阅[快速入门：使用 Azure 门户启动现有 Azure Spring Cloud 应用程序](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)。

### <a name="prepare-the-spring-cloud-config-server"></a>准备 Spring Cloud Config 服务器

在 Azure Spring Cloud 实例中配置配置服务器。 有关详细信息，请参阅[教程：为服务设置 Spring Cloud 配置服务器实例](/azure/spring-cloud/spring-cloud-tutorial-config-server)。

> [!NOTE]
> 如果当前的 Spring Cloud Config 存储库位于本地文件系统或本地，你首先需要将配置文件迁移或复制到基于私有云的存储库，例如 GitHub、Azure Repos 或 BitBucket。

[!INCLUDE [ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud](includes/ensure-console-logging-and-configure-diagnostic-settings-azure-spring-cloud.md)]

[!INCLUDE [configure-persistent-storage-azure-spring-cloud](includes/configure-persistent-storage-azure-spring-cloud.md)]

### <a name="migrate-spring-cloud-vault-secrets-to-azure-keyvault"></a>将 Spring Cloud Vault 机密迁移到 Azure KeyVault

可以使用 Azure KeyVault Spring Boot 起动器，通过 Spring 将机密直接注入到应用程序。 有关详细信息，请参阅[如何使用适用于 Azure Key Vault 的 Spring Boot 起动器](../spring-framework/configure-spring-boot-starter-java-app-with-azure-key-vault.md)。

> [!NOTE]
> 迁移可能需要重命名一些机密。 请相应地更新应用程序代码。

### <a name="migrate-all-certificates-to-keyvault"></a>将所有证书迁移到 KeyVault

Azure Spring Cloud 不提供对 JRE 密钥存储的访问权限，因此必须将证书迁移到 Azure KeyVault，并更改应用程序代码，才能访问 KeyVault 中的证书。 有关详细信息，请参阅 [Key Vault 证书入门](/azure/key-vault/certificates/certificate-scenarios)和[适用于 Java 的 Azure Key Vault 证书客户端库](/java/api/overview/azure/security-keyvault-certificates-readme)。

### <a name="remove-application-performance-management-apm-integrations"></a>删除应用程序性能管理 (APM) 集成

消除与 APM 工具/代理的任何集成。 有关使用 Azure Monitor 配置性能管理的信息，请参阅[迁移后](#post-migration)部分。

### <a name="replace-explicit-zipkin-dependencies-with-spring-cloud-starters"></a>将显式 Zipkin 依赖项替换为 Spring Cloud 起动器

如果任何已迁移的应用程序具有显式 Zipkin 依赖项，请将其删除，并将其替换为 Spring Cloud 起动器，如[准备要在 Azure Spring Cloud 中部署的 Java Spring 应用程序](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment)的[分布式跟踪依赖项](/azure/spring-cloud/spring-cloud-tutorial-prepare-app-deployment#distributed-tracing-dependency)部分中所述。 有关 Azure App Insights 的分布式跟踪的信息，请参阅[迁移后](#post-migration)部分。

### <a name="disable-metrics-clients-and-endpoints-in-your-applications"></a>在应用程序中禁用指标客户端和终结点

删除应用程序中使用的任何指标客户端或应用程序中公开的任何指标终结点。

### <a name="deploy-the-services"></a>部署服务

部署每个已迁移的微服务（不包括 Spring Cloud Config 和注册表服务器），如[快速入门：使用 Azure 门户启动现有 Azure Spring Cloud 应用程序](/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal)中所述。

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
* 如果标识提供者是本地 Active Directory 林，请考虑使用 Azure Active Directory 实现混合标识解决方案。 有关指南，请参阅[混合标识文档](/azure/active-directory/hybrid/)。
* 如果标识提供者是另一个本地解决方案（如 PingFederate），请参阅 [Azure AD Connect 的自定义安装](/azure/active-directory/hybrid/how-to-connect-install-custom)主题，以使用 Azure Active Directory 配置联合。 或者，考虑使用 Spring 安全性通过 [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) 或 [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 来使用标识提供者。

### <a name="update-client-applications"></a>更新客户端应用程序

更新所有客户端应用程序的配置，以使用已迁移应用程序的已发布 Azure Spring Cloud 终结点。

## <a name="post-migration"></a>迁移后

* 请考虑为自动、一致的部署添加部署管道。 提供有关 [Azure Pipelines](/azure/spring-cloud/spring-cloud-howto-cicd)、[GitHub Actions](/azure/spring-cloud/spring-cloud-howto-github-actions) 和 [Jenkins](/azure/jenkins/tutorial-jenkins-deploy-cli-spring-cloud-service) 的说明。

* 请考虑使用过渡部署在生产中测试代码更改，然后再将其提供给一些或所有最终用户。 有关详细信息，请参阅[在 Azure Spring Cloud 中设置过渡环境](/azure/spring-cloud/spring-cloud-howto-staging-environment)。

* 请考虑添加服务绑定，以便将应用程序连接到受支持的 Azure 数据库。 如果使用这些服务绑定，则无需向 Spring Cloud 应用程序提供连接信息（包括凭据）。

* 请考虑[使用分布式跟踪和 Azure App Insights](/azure/spring-cloud/spring-cloud-tutorial-distributed-tracing) 来监视应用程序的性能和交互。

* 请考虑添加 Azure Monitor 预警规则和操作组，以快速检测并解决异常情况。 有关详细信息，请参阅[教程：使用警报和操作组监视 Spring Cloud 资源](/azure/spring-cloud/spring-cloud-tutorial-alerts-action-groups)。

* 请考虑将 Azure Spring Cloud 部署复制到其他区域，以降低延迟，提高可靠性和容错。 使用 [Azure 流量管理器](/azure/traffic-manager)在部署之间实现负载均衡，或使用 [Azure Front Door](/azure/frontdoor) 添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。

* 如果不需要异地复制，请考虑添加 [Azure 应用程序网关](/azure/application-gateway)，以便添加 SSL 卸载和具有 DDoS 防护的 Web 应用程序防火墙。

* 如果应用程序使用旧的 Spring Cloud Netflix 组件，请考虑将它们替换为当前替代项：

   | 旧的                        | 当前                                                |
   |-------------------------------|--------------------------------------------------------|
   | Spring Cloud Eureka           | Spring Cloud 服务注册表                          |
   | Spring Cloud Netflix Zuul     | Spring Cloud 网关                                   |
   | Spring Cloud Netflix Archaius | Spring Cloud 配置服务器                             |
   | Spring Cloud Netflix 功能区   | Spring Cloud 负载均衡器（客户端负载均衡器） |
   | Spring Cloud Hystrix          | Spring Cloud 断路器 + Resilience4J            |
   | Spring Cloud Netflix Turbine  | Micrometer + Prometheus                                |