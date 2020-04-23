---
title: 将 WebLogic 应用程序迁移到 Azure Kubernetes 服务上的 WildFly
description: 本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。
author: mriem
ms.author: manriem
ms.topic: conceptual
ms.date: 2/28/2020
ms.openlocfilehash: d17551aeb1041415e2c5b6d5fd8a43d3b7b670aa
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673383"
---
# <a name="migrate-weblogic-applications-to-wildfly-on-azure-kubernetes-service"></a>将 WebLogic 应用程序迁移到 Azure Kubernetes 服务上的 WildFly

本指南介绍在需要迁移现有 WebLogic 应用程序以使之在 Azure Kubernetes 服务容器中的 WildFly 上运行时应注意的事项。

## <a name="before-you-start"></a>开始之前

如果无法满足任何预迁移要求，请参阅随附迁移指南：

* [将 WebLogic 应用程序迁移到 Azure 虚拟机](migrate-weblogic-to-virtual-machines.md)

## <a name="pre-migration"></a>预迁移

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

### <a name="validate-that-the-supported-java-version-works-correctly"></a>验证支持的 Java 版本是否正常运行

使用 Azure Kubernetes 服务上的 WildFly 需要 Java 的特定版本。 因此，需验证应用程序能否使用该受支持的版本正确运行。 如果当前服务器使用受支持的 JDK（如 Oracle JDK 或 IBM OpenJ9），则此验证尤其重要。

若要获取当前版本，请登录到生产服务器并运行以下命令：

```bash
java -version
```

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

### <a name="validate-whether-and-how-the-file-system-is-used"></a>验证是否使用以及如何使用文件系统

使用应用程序服务器上的文件系统需要重新配置，在极少数情况下需要体系结构更改。 文件系统可供 WebLogic 共享模块或应用程序代码使用。 可以标识以下部分所述的部分或全部方案。

#### <a name="read-only-static-content"></a>只读静态内容

如果应用程序当前提供静态内容，则需为其提供一个备用位置。 可能需要考虑将静态内容移到 Azure Blob 存储，并添加 Azure CDN，方便用户在全球范围内快速下载。 有关详细信息，请参阅 [Azure 存储中的静态网站托管](/azure/storage/blobs/storage-blob-static-website)和[快速入门：将 Azure 存储帐户与 Azure CDN 集成](/azure/cdn/cdn-create-a-storage-account-with-cdn)。

#### <a name="dynamically-published-static-content"></a>动态发布的静态内容

如果应用程序允许那些通过应用程序上传/生成但在创建后不可变的静态内容，则可将上述 Azure Blob 存储和 Azure CDN 与 Azure 函数配合使用，以便处理上传和 CDN 刷新操作。 我们提供了一个示例实现，用于[通过 Azure Functions 进行静态内容的上传和 CDN 预加载操作](https://github.com/Azure-Samples/functions-java-push-static-contents-to-cdn)。

#### <a name="dynamic-or-internal-content"></a>动态或内部内容

对于经常由应用程序写入和读取的文件（如临时数据文件），或者仅对应用程序可见的静态文件，可以将 Azure 存储共享作为持久卷进行装载。 有关详细信息，请参阅[在 Azure Kubernetes 服务中动态创建永久性卷并将其用于 Azure 文件存储](/azure/aks/azure-files-dynamic-pv)。

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
