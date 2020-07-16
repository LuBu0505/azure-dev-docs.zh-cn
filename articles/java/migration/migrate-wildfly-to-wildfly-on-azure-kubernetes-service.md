---
title: 将 WildFly 应用程序迁移到 Azure Kubernetes 服务上的 WildFly
description: 本指南介绍在需要迁移现有 WildFly 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 3/16/2020
ms.custom: devx-track-java
ms.openlocfilehash: cf056d9d961662e20f4fa4112a153150bf05460f
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379691"
---
# <a name="migrate-wildfly-applications-to-wildfly-on-azure-kubernetes-service"></a>将 WildFly 应用程序迁移到 Azure Kubernetes 服务上的 WildFly

本指南介绍在需要迁移现有 WildFly 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

[!INCLUDE [inventory-server-capacity-aks](includes/inventory-server-capacity-aks.md)]

### <a name="inventory-all-secrets"></a>清点所有机密

检查生产服务器上的所有属性和配置文件中是否有机密和密码。 请确保在 WAR 中检查 *jboss-web.xml*。 还可以在应用程序中查找包含密码或凭据的配置文件。

请考虑将这些机密存储到 Azure KeyVault 中。 有关详细信息，请参阅 [Azure Key Vault 基本概念](/azure/key-vault/basic-concepts)。

[!INCLUDE [inventory-all-certificates](includes/inventory-all-certificates.md)]

[!INCLUDE [validate-that-the-supported-java-version-works-correctly-wildfly](includes/validate-that-the-supported-java-version-works-correctly-wildfly.md)]

### <a name="inventory-jndi-resources"></a>清点 JNDI 资源

清点所有 JNDI 资源。 某些资源（如 JMS 消息代理）可能需要迁移或重新配置。

### <a name="determine-whether-session-replication-is-used"></a>确定是否使用了会话复制

如果应用程序依赖于会话复制，则必须更改应用程序，摆脱此依赖关系。

#### <a name="inside-your-application"></a>在应用程序中

检查 *WEB-INF/jboss-web.xml* 和/或 *WEB-INF/web.xml* 文件。

### <a name="document-datasources"></a>记录数据源

如果应用程序使用任何数据库，则需捕获以下信息：

* 数据源名称是什么？
* 连接池配置是什么？
* 在哪里可以找到 JDBC 驱动程序 JAR 文件？

有关详细信息，请参阅 WildFly 文档中的 [DataSource 配置](http://docs.wildfly.org/19/Admin_Guide.html#DataSource)。

### <a name="determine-whether-and-how-the-file-system-is-used"></a>确定是否使用以及如何使用文件系统

使用应用程序服务器上的文件系统需要重新配置，在极少数情况下需要体系结构更改。 文件系统可供 WildFly 模块或应用程序代码使用。 可以标识以下部分所述的部分或全部方案。

[!INCLUDE [static-content](includes/static-content.md)]

[!INCLUDE [dynamic-or-internal-content-aks](includes/dynamic-or-internal-content-aks.md)]

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs](includes/determine-whether-your-application-relies-on-scheduled-jobs.md)]

[!INCLUDE [determine-whether-a-connection-to-on-premises-is-needed](includes/determine-whether-a-connection-to-on-premises-is-needed.md)]

[!INCLUDE [determine-whether-jms-queues-or-topics-are-in-use](includes/determine-whether-jms-queues-or-topics-are-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-entity-beans](includes/determine-whether-your-application-uses-entity-beans.md)]

[!INCLUDE [determine-whether-the-java-ee-application-client-feature-is-in-use-aks](includes/determine-whether-the-java-ee-application-client-feature-is-in-use-aks.md)]

[!INCLUDE [determine-whether-your-application-contains-os-specific-code](includes/determine-whether-your-application-contains-os-specific-code.md)]

[!INCLUDE [determine-whether-ejb-timers-are-in-use](includes/determine-whether-ejb-timers-are-in-use.md)]

### <a name="determine-whether-jca-connectors-are-in-use"></a>确定是否使用了 JCA 连接器

如果应用程序使用 JCA 连接器，则需验证是否可以在 WildFly 上使用 JCA 连接器。 如果 JCA 实现与 WildFly 相关联，则需重构应用程序，摆脱该依赖关系。 如果可以使用该连接器，则需将这些 JAR 添加到服务器 classpath 中，并将所需的配置文件放在 WildFly 服务器目录中的正确位置，使其可用。

[!INCLUDE [determine-whether-jaas-is-in-use](includes/determine-whether-jaas-is-in-use.md)]

[!INCLUDE [determine-whether-your-application-uses-a-resource-adapter](includes/determine-whether-your-application-uses-a-resource-adapter.md)]

[!INCLUDE [determine-whether-your-application-is-composed-of-multiple-wars](includes/determine-whether-your-application-is-composed-of-multiple-wars.md)]

### <a name="determine-whether-your-application-is-packaged-as-an-ear"></a>确定应用程序是否打包为 EAR

如果将应用程序打包为 EAR 文件，请务必检查 *application.xml* 文件并捕获配置。

> [!NOTE]
> 如果希望能够独立缩放每个 Web 应用程序，以便更好地使用 AKS 资源，则应将 EAR 分解为单独的 Web 应用程序。

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

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

将应用程序迁移到 Azure Kubernetes 服务后，即应验证其运行是否符合预期。 完成该操作后，可以参考我们提供的一些建议，使应用程序的云原生性更好。

[!INCLUDE [recommendations-wildfly-on-aks](includes/recommendations-wildfly-on-aks.md)]
