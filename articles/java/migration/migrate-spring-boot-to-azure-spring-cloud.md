---
title: 将 Spring Boot 应用程序迁移到 Azure Spring Cloud
description: 本指南介绍在需要迁移现有 Spring Boot 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 5/26/2020
ms.custom: devx-track-java
ms.openlocfilehash: 4d2f84c6c77294c9a2d25028608e2feb712599f8
ms.sourcegitcommit: 4036ac08edd7fc6edf8d11527444061b0e4531ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89062039"
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

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>迁移后

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]
