---
title: 将 WebLogic 应用程序迁移到 Azure Kubernetes 服务上的 WildFly
description: 本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 2/28/2020
ms.openlocfilehash: b8df6a28083521bca900e5c1c939c6456546f349
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988928"
---
# <a name="migrate-weblogic-applications-to-wildfly-on-azure-kubernetes-service"></a>将 WebLogic 应用程序迁移到 Azure Kubernetes 服务上的 WildFly

本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

如果无法满足任何预迁移要求，请参阅随附迁移指南：

* [将 WebLogic 应用程序迁移到 Azure 虚拟机](migrate-weblogic-to-virtual-machines.md)

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

[!INCLUDE [inventory-all-secrets](includes/inventory-all-secrets.md)]

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [inventory-jndi-resources](includes/inventory-jndi-resources.md)]

### <a name="determine-whether-session-replication-is-used"></a>确定是否使用了会话复制

如果应用程序依赖于会话复制，则无论是否有 Oracle Coherence*Web，都可以使用两个选项：

* 重构应用程序，使用数据库进行会话管理。
* 重构应用程序，将 Azure Redis 服务的会话外部化。 有关详细信息，请参阅 [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)。

[!INCLUDE [document-datasources](includes/document-datasources.md)]

[!INCLUDE [determine-whether-weblogic-has-been-customized](includes/determine-whether-weblogic-has-been-customized.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

[!INCLUDE [determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries](includes/determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries.md)]

[!INCLUDE [determine-whether-osgi-bundles-are-used](includes/determine-whether-osgi-bundles-are-used.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-oracle-service-bus-is-in-use](includes/determine-whether-oracle-service-bus-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

[!INCLUDE [determine-whether-your-application-is-packaged-as-an-ear](includes/determine-whether-your-application-is-packaged-as-an-ear.md)]

<!-- AKS-specific extension of the last INCLUDE. -->
> [!NOTE]
> 如果希望能够独立缩放每个 Web 应用程序，以便更好地使用 AKS 资源，则应将 EAR 分解为单独的 Web 应用程序。
<!-- end extension -->

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly-wildfly](includes/validate-that-the-supported-java-version-works-correctly-wildfly.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

### <a name="determine-whether-weblogic-scripting-tool-wlst-is-used"></a>确定是否使用了 WebLogic 脚本编写工具 (WLST)

如果当前使用 WLST 来执行部署，则需评估其功能。 如果 WLST 在部署过程中更改应用程序的任何（运行时）参数，则需确保这些参数符合以下选项之一的要求：

* 它们已外部化为应用设置。
* 它们已嵌入应用程序中。
* 在部署过程中，它们使用的是 JBoss CLI。

如果 WLST 执行的操作超出上述范围，则在迁移过程中需要做一些额外的工作。

### <a name="determine-whether-your-application-uses-weblogic-specific-apis"></a>确定应用程序是否使用特定于 WebLogic 的 API

如果应用程序使用特定于 WebLogic 的 API，则需重构该代码，删除那些依赖项。 例如，如果使用了 [Java API Reference for Oracle WebLogic Server](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/wlapi/index.html?overview-summary.html)（适用于 Oracle WebLogic Server 的 Java API 参考）中提到的类，则表示你已在应用程序中使用了特定于 WebLogic 的 API。

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

### <a name="determine-whether-a-deployment-plan-was-used"></a>确定是否使用了部署计划

如果应用是使用部署计划部署的，则需评估部署计划正在执行的操作。 如果部署计划是直接部署，则可部署 Web 应用程序，无需进行任何更改。 如果部署计划较为复杂，则需确定是否可以使用 JBoss CLI 在部署过程中正确配置应用程序。 如果无法使用 JBoss CLI，则需重构应用程序，使之不再需要部署计划。

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-and-how-the-file-system-is-used"></a>确定是否使用以及如何使用文件系统

使用应用程序服务器上的文件系统需要重新配置，在极少数情况下需要体系结构更改。 文件系统可供 WebLogic 共享模块或应用程序代码使用。 可以标识以下部分所述的部分或全部方案。

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

### <a name="determine-whether-jca-connectors-are-used"></a>确定是否使用了 JCA 连接器

如果应用程序使用 JCA 连接器，则需验证是否可以在 WildFly 上使用 JCA 连接器。 如果 JCA 实现与 WebLogic 相关联，则需重构应用程序，去除该依赖关系。 如果可以使用该连接器，则需将这些 JAR 添加到服务器 classpath 中，并将所需的配置文件放在 WildFly 服务器目录中的正确位置，使其可用。

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

### <a name="determine-whether-weblogic-clustering-is-used"></a>确定是否使用了 WebLogic 聚类分析

最可能的情况是，你已将应用程序部署到多个 WebLogic 服务器上，以实现高可用性。 Azure Kubernetes 服务能够缩放，但如果你已使用 WebLogic 群集 API，则需重构代码，避免使用该 API。

[!INCLUDE [perform-in-place-testing](includes/perform-in-place-testing.md)]

## <a name="migration"></a>迁移

[!INCLUDE [provision-azure-container-registry-and-azure-kubernetes-service](includes/provision-azure-container-registry-and-azure-kubernetes-service.md)]

[!INCLUDE [create-a-docker-image-for-wildfly](includes/create-a-docker-image-for-wildfly.md)]

[!INCLUDE [build-and-push-the-docker-image-to-azure-container-registry](includes/build-and-push-the-docker-image-to-azure-container-registry.md)]

[!INCLUDE [provision-a-public-ip-address](includes/provision-a-public-ip-address.md)]

[!INCLUDE [deploy-to-aks](includes/deploy-to-aks.md)]

### <a name="configure-persistent-storage"></a>配置持久性存储

如果应用程序需要非易失性存储，请配置一个或多个[持久性卷](/azure/aks/azure-disks-dynamic-pv)。

[!INCLUDE [migrate-scheduled-jobs-aks](includes/migrate-scheduled-jobs-aks.md)]

## <a name="post-migration"></a>迁移后

将应用程序迁移到 Azure Kubernetes 服务后，即应验证其运行是否符合预期。 完成该操作后，请查看以下建议，以便提高应用程序的云原生性。

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
