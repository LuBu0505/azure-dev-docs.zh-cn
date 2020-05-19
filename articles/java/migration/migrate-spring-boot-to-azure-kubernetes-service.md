---
title: 迁移 Spring Boot 应用程序以在 Azure Kubernetes 服务上运行
description: 本指南介绍在需要迁移现有 Spring Boot 应用程序以使之在 Azure Kubernetes 服务容器中运行时应注意的事项。
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 4/10/2020
ms.openlocfilehash: 6c2781914e65d28a57f2f80ed287921eab3b76ae
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369989"
---
# <a name="migrate-spring-boot-applications-to-azure-kubernetes-service"></a>将 Spring Boot 应用程序迁移到 Azure Kubernetes 服务

本指南介绍在需要迁移现有 Spring Boot 应用程序以使之在 Azure Kubernetes 服务 (AKS) 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

### <a name="validate-that-the-supported-java-version-works-correctly"></a>验证支持的 Java 版本是否正常运行

在 AKS 上运行 Spring Boot 应用程序时，建议使用受支持的 Java 版本。 确认应用程序使用受支持的版本正常运行。

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

### <a name="determine-whether-and-how-the-file-system-is-used"></a>确定是否使用以及如何使用文件系统

如果 Spring Boot 应用程序要使用文件系统，则需要重新配置，在极少数情况下需要体系结构更改。 可以标识以下部分所述的部分或全部方案。

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [identify-spring-boot-versions](includes/identify-spring-boot-versions.md)]

对于使用 Spring Boot 1.x 的任何应用程序，请按照 [Spring Boot 2.0 迁移指南](https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-2.0-Migration-Guide)将其更新为受支持的 Spring Boot 版本。

### <a name="review-your-database-properties"></a>查看数据库属性

如果应用程序使用数据库，请查看 application.properties 文件中的数据库属性，以确保在迁移到 AKS 后，Spring Boot 应用程序仍然可以访问数据库。 如果数据库是本地数据库，则需要将其迁移到云，或与本地数据库建立连接。

### <a name="identify-log-aggregation-solutions"></a>确定日志聚合解决方案

确定要迁移的应用程序所使用的任何日志聚合解决方案。

### <a name="identify-application-performance-management-apm-agents"></a>确定应用程序性能管理 (APM) 代理

确定与应用程序一起使用的任何应用程序性能监视代理（例如 Dynatrace 和 Datadog）。 需要将这些 APM 代理重新配置为包含在 Dockerfile 或 Jib 配置中，或使用 Application Insights 进程内 Java 代理。

### <a name="identify-zipkin-dependencies"></a>确定 Zipkin 依赖项

确定应用程序是否在 Zipkin 上具有显式依赖项。 在 Maven 或 Gradle 依赖项中查找 `io.zipkin.java` 组的依赖项。

### <a name="inventory-external-resources"></a>清点外部资源

标识外部资源，如数据源、JMS 消息代理和其他服务的 URL。 在 Spring Boot 应用程序中，通常可在通常名为 application.properties 或 application.yml 的文件的 src/main/directory 文件夹中找到此类资源的配置  。

[!INCLUDE [inventory-databases-spring-boot](includes/inventory-databases-spring-boot.md)]

[!INCLUDE [identify-jms-brokers-in-spring](includes/identify-jms-brokers-in-spring.md)]

确定正在使用的一个或多个代理后，请找到相应的设置。 在 Spring Boot 应用程序中，通常可以在应用程序目录的 application.properties 和 application.yml 文件中找到它们 。

[!INCLUDE [jms-broker-settings-examples-in-spring](includes/jms-broker-settings-examples-in-spring.md)]

[!INCLUDE [identify-external-caches-azure-spring-cloud](includes/identify-external-caches-azure-spring-cloud.md)]

[!INCLUDE [inventory-configuration-sources-and-secrets-spring-boot](includes/inventory-configuration-sources-and-secrets-spring-boot.md)]

[!INCLUDE [inspect-the-deployment-architecture-spring-boot](includes/inspect-the-deployment-architecture-spring-boot.md)]

#### <a name="identity-providers"></a>标识提供者

确定需要身份验证和/或授权的所有标识提供者和所有 Spring Boot 应用程序。 有关可如何配置标识提供者的信息，请参阅以下内容：

* 有关 OAuth 或 OAuth2 Spring 安全性配置，请参阅 [Spring 安全性](https://spring.io/projects/spring-security)。
* 有关 Auth0 Spring 安全性配置，请参阅 [Auth0 Spring 安全性文档](https://auth0.com/docs/quickstart/backend/java-spring-security5/01-authorization)。
* 有关 PingFederate Spring 安全性配置，请参阅 [Auth0 PingFederate 说明](https://auth0.com/authenticate/java-spring-security/ping-federate/)。

#### <a name="resources-configured-through-pivotal-cloud-foundry-pcf"></a>通过 Pivotal Cloud Foundry (PCF) 配置的资源

对于使用 Pivotal Cloud Foundry 托管的应用程序，通常通过 PCF 服务绑定配置外部资源（包括前面所述的资源）。 若要检查此类资源的配置，请使用 [Cloud Foundry CLI](https://docs.cloudfoundry.org/cf-cli/) 查看应用程序的 `VCAP_SERVICES` 变量。

```bash
# Log into PCF, if needed (enter credentials when prompted)
cf login -a <API endpoint>

# Set the organization and space containing the application, if not already selected during login.
cf target org <Organization Name>
cf target space <Space Name>

# Display variables for the application
cf env <Application Name>
```

检查绑定到应用程序的外部服务配置设置的 `VCAP_SERVICES` 变量。 有关详细信息，请参阅 [PCF 文档](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES)。

### <a name="in-place-testing"></a>就地测试

在创建容器映像之前，请将应用程序迁移到要在 AKS 上使用的 JDK 和 Spring Boot 版本。 全面测试应用程序，确保兼容性和性能。

## <a name="migration"></a>迁移

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

### <a name="create-a-docker-image-for-spring-boot"></a>创建 Spring Boot 的 Docker 映像

若要创建 Dockerfile，需要符合以下先决条件：

* 受支持的 JDK。
* JVM 运行时选项。
* 可以通过某种方法传入环境变量（如果适用）。

然后，可以执行以下部分所述的步骤（如果适用）。 对于 Dockerfile 和 Spring Boot 应用程序，可以从使用 [Spring Boot 容器快速入门存储库](https://github.com/Azure/spring-boot-container-quickstart)着手。

#### <a name="configure-keyvault-flexvolume"></a>配置 KeyVault FlexVolume

创建 Azure KeyVault 并填充所有必要的机密。 有关详细信息，请参阅[快速入门：使用 Azure CLI 在 Azure Key Vault 中设置和检索机密](/azure/key-vault/quick-create-cli)。 然后，配置 [KeyVault FlexVolume](https://github.com/Azure/kubernetes-keyvault-flexvol/blob/master/README.md)，使这些机密可供 Pod 访问。

还需要更新用于启动 Spring Boot 应用程序的启动脚本。 此脚本必须在启动应用程序之前将证书导入 Spring Boot 使用的密钥存储。

### <a name="build-and-push-the-docker-image-to-azure-container-registry"></a>构建 Docker 映像并将其推送到 Azure 容器注册表

创建 Dockerfile 后，需要生成 Docker 映像并将其发布到 Azure 容器注册表。

如果你使用了我们的 [Spring Boot 容器快速入门 GitHub 存储库](https://github.com/Azure/spring-boot-container-quickstart)，则构建映像并将其推送到 Azure 容器注册表的过程相当于调用以下三个命令。

在这些示例中，`MY_ACR` 环境变量保存 Azure 容器注册表的名称，`MY_APP_NAME` 变量保存要在 Azure 容器注册表中使用的 Web 应用程序的名称。

生成部署文件：

```bash
mvn package
```

登录到 Azure 容器注册表：

```azurecli
az acr login -n ${MY_ACR}
```

生成并推送映像：

```azurecli
az acr build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME} .
```

也可使用 Docker CLI 在本地生成并测试映像，如以下命令所示。 此方法可以简化在一开始部署到 ACR 之前对映像进行的测试和优化。 但是，它要求安装 Docker CLI 并确保 Docker 后台程序正在运行。

生成映像：

```bash
docker build -t ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

在本地运行映像：

```bash
docker run -it -p 8080:8080 ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

现在可以在 `http://localhost:8080` 上访问应用程序。

登录到 Azure 容器注册表：

```azurecli
az acr login -n ${MY_ACR}
```

向 Azure 容器注册表推送映像：

```bash
docker push ${MY_ACR}.azurecr.io/${MY_APP_NAME}
```

若要更深入地了解如何在 Azure 中生成和存储容器映像，请参阅 Learn 模块：[使用 Azure 容器注册表生成和存储容器映像](/learn/modules/build-and-store-container-images/)。

如果使用了 [Spring Boot 容器快速入门 GitHub 存储库](https://github.com/Azure/spring-boot-container-quickstart)，还可以添加自定义密钥存储，该密钥存储将在启动时添加到 JVM。 如果将密钥存储文件放在 /opt/spring-boot/mycert.crt，则会发生这种添加操作。 为此，可以将文件直接添加到 Dockerfile，或使用 KeyVault FlexVolume，如前文所述。

如果使用了 [Spring Boot 容器快速入门 GitHub 存储库](https://github.com/Azure/spring-boot-container-quickstart)，则还可以通过在 Kubernetes 部署文件中设置 `APPLICATIONINSIGHTS_CONNECTION_STRING` 环境变量来启用 Application Insights（环境变量的值应类似于 `InstrumentationKey=00000000-0000-0000-0000-000000000000`）。 有关详细信息，请参阅 [Java 无代码应用程序监视 Azure Monitor Application Insights](/azure/azure-monitor/app/java-in-process-agent)。

如果不需要对 Docker 映像进行任何自定义，也可以浏览 [Maven Jib 插件](https://github.com/GoogleContainerTools/jib/tree/master/jib-maven-plugin)的使用或部署到 AKS。 有关详细信息，请参阅[将 Spring Boot 应用程序部署到 Azure Kubernetes 服务](/azure/developer/java/spring-framework/deploy-spring-boot-java-app-on-kubernetes)。

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

### <a name="deploy-to-aks"></a>部署到 AKS

创建并应用 Kubernetes YAML 文件。 有关详细信息，请参阅[快速入门：使用 Azure CLI 部署 Azure Kubernetes 服务群集](/azure/aks/kubernetes-walkthrough#run-the-application)。 如果要创建外部负载均衡器（不管是针对应用程序，还是针对入口控制器），请确保提供在上一部分以 `LoadBalancerIP` 形式预配的 IP 地址。

包括环境变量形式的外部化参数。 有关详细信息，请参阅[为容器定义环境变量](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)。

创建部署 YAML 时，请确保包含内存和 CPU 设置，以便正确调整容器的大小。

### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>确保控制台日志记录并配置诊断设置

配置日志记录，使所有应用程序都记录到控制台而不是文件。

将应用程序部署到 Azure Kubernetes 服务后，可以使用 `kubectl` 查看日志。

#### <a name="logstashelk-stack"></a>LogStash/ELK 堆栈

如果使用 LogStash/ELK 堆栈进行日志聚合，请将诊断设置配置为将控制台输出流式传输到 [Azure 事件中心](/azure/event-hubs/)。 然后，使用 [LogStash EventHub 插件](https://github.com/logstash-plugins/logstash-input-azure_event_hubs)将记录的事件引入到 LogStash 中。

#### <a name="splunk"></a>Splunk

如果使用 Splunk 进行日志聚合，请将诊断设置配置为将控制台输出流式传输到 [Azure Blob 存储](/azure/storage/blobs/)。 然后，使用 [Microsoft 云服务的 Splunk 附加产品](https://splunkbase.splunk.com/app/3757/)将记录的事件引入到 Splunk 中。

### <a name="migrate-and-enable-the-identity-provider"></a>迁移和启用标识提供者

如果任何 Spring Boot 应用程序需要身份验证或授权，请确保将其配置为访问标识提供者：

* 如果标识提供者是 Azure Active Directory，则不需要进行任何更改。
* 如果标识提供者是本地 Active Directory 林，请考虑使用 Azure Active Directory 实现混合标识解决方案。 有关指南，请参阅[混合标识文档](/azure/active-directory/hybrid/)。
* 如果标识提供者是另一个本地解决方案（如 PingFederate），请参阅 [Azure AD Connect 的自定义安装](/azure/active-directory/hybrid/how-to-connect-install-custom)主题，以使用 Azure Active Directory 配置联合。 或者，考虑使用 Spring 安全性通过 [OAuth2/OpenID Connect](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#oauth2) 或 [SAML](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-saml2) 来使用标识提供者。

### <a name="configure-persistent-storage"></a>配置持久性存储

如果应用程序需要非易失性存储，请配置一个或多个[持久性卷](/azure/aks/azure-disks-dynamic-pv)。

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>迁移后

将应用程序迁移到 AKS 后，即应验证其运行是否符合预期。 完成该操作后，可以参考我们提供的一些建议，使应用程序的云原生性更好。

### <a name="recommendations"></a>建议

* 考虑向分配给入口控制器或应用程序负载均衡器的 IP 地址添加 DNS 名称。 有关详细信息，请参阅[在 AKS 中使用静态公共 IP 地址创建入口控制器](/azure/aks/ingress-static-ip)。

* 考虑为应用程序添加 [HELM 图表](https://helm.sh/docs/topics/charts/)。 可以通过 Helm 图表将应用程序部署参数化，供更多样化的客户使用和自定义。

* 设计和实施 DevOps 策略。 若要在提高开发速度的同时保持可靠性，请考虑通过 Azure Pipelines 自动进行部署和测试。 有关详细信息，请参阅[生成并部署到 AKS](/azure/devops/pipelines/ecosystems/kubernetes/aks-template)。

* 启用[针对群集的 Azure 监视](/azure/azure-monitor/insights/container-insights-enable-existing-clusters)，以便收集容器日志、跟踪使用情况等。

* 考虑通过 Prometheus 公开特定于应用程序的指标。 Prometheus 是在 Kubernetes 社区中广泛采用的开源指标框架。 可以配置 [Azure Monitor 中的 Prometheus 指标抓取](/azure/azure-monitor/insights/container-insights-prometheus-integration)，而不是托管你自己的 Prometheus 服务器，以便进行应用程序指标聚合，并自动响应或升级异常情况。

* 设计和实施业务连续性和灾难恢复策略。 对于关键应用程序，请考虑[多区域部署体系结构](/azure/aks/operator-best-practices-multi-region)。

* 查看 [Kubernetes 版本支持策略](/azure/aks/supported-kubernetes-versions#kubernetes-version-support-policy)。 你有责任持续[更新 AKS 群集](/azure/aks/upgrade-cluster)，确保其始终运行受支持的版本。

* 让所有负责群集管理和应用程序开发的团队成员查看相关的 [AKS 最佳做法](/azure/aks/best-practices)。

* 确保部署文件指定滚动更新的完成方式。 有关详细信息，请参阅 Kubernetes 文档中的[滚动更新部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment)。

* 设置自动缩放以处理高峰时间负载。 有关详细信息，请参阅[自动缩放群集以满足 AKS 上的应用程序需求](/azure/aks/cluster-autoscaler)。

* 考虑[监视代码缓存大小](https://docs.oracle.com/javase/8/embedded/develop-apps-platforms/codecache.htm)并向 Dockerfile 中的 `JAVA_OPTS` 变量添加参数 `-XX:InitialCodeCacheSize` 和 `-XX:ReservedCodeCacheSize`，进一步优化性能。
