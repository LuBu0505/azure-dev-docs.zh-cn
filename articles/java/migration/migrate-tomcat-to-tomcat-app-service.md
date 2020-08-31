---
title: 将 Tomcat 应用程序迁移到 Azure 应用服务上的 Tomcat
description: 本指南介绍在需要迁移现有 Tomcat 应用程序以使之在使用 Tomcat 的 Azure 应用服务上运行时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 1/20/2020
ms.custom: devx-track-java
ms.openlocfilehash: 227908087ffdbdc3ce27a3da721464ff91b6b085
ms.sourcegitcommit: 2f832baf90c208a8a69e66badef5f126d23bbaaf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2020
ms.locfileid: "88725191"
---
# <a name="migrate-tomcat-applications-to-tomcat-on-azure-app-service"></a>将 Tomcat 应用程序迁移到 Azure 应用服务上的 Tomcat

本指南介绍在需要迁移现有 Tomcat 应用程序以使之在使用 Tomcat 9.0 的 Azure 应用服务上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

如果无法满足任何预迁移要求，请参阅以下伴随迁移指南：

* [将 Tomcat 应用程序迁移到 Azure Kubernetes 服务上的容器](migrate-tomcat-to-containers-on-azure-kubernetes-service.md)
* 将 Tomcat 应用程序迁移到 Azure 虚拟机（按指南进行计划）

### <a name="switch-to-a-supported-platform"></a>切换到受支持的平台

应用服务在特定版本的 Java 上提供特定版本的 Tomcat。 若要确保兼容性，请在继续执行其余步骤之前，将应用程序迁移到当前环境中支持的 Tomcat 和 Java 版本之一。 务必全面测试生成的配置。 请在此类测试中使用最新且稳定的 Linux 发布版。

[!INCLUDE [note-obtain-your-current-java-version-app-service](includes/note-obtain-your-current-java-version-app-service.md)]

若要获取当前的 Tomcat 版本，请登录到生产服务器并运行以下命令：

```bash
${CATALINA_HOME}/bin/version.sh
```

若要获取 Azure 应用服务使用的当前版本，请下载 [Tomcat 9](https://tomcat.apache.org/download-90.cgi)，具体取决于你计划在 Azure 应用服务中使用的版本。

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

[!INCLUDE [inventory-secrets](includes/inventory-secrets.md)]

### <a name="inventory-certificates"></a>清点证书

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

<!-- App-Service-specific addendum to inventory-persistence-usage -->
#### <a name="dynamic-or-internal-content"></a>动态或内部内容

对于经常由应用程序写入和读取的文件（如临时数据文件），或者仅对应用程序可见的静态文件，可以将 Azure 存储装载到应用服务文件系统中。 有关详细信息，请参阅[从 Linux 上的应用服务中的 Azure 存储提供内容](/azure/app-service/configure-connect-to-azure-storage)。

### <a name="identify-session-persistence-mechanism"></a>识别会话持久性机制

若要确定正在使用的会话持久性管理器，请在应用程序和 Tomcat 配置中检查 *context.xml* 文件。 请查找 `<Manager>` 元素，然后记下 `className` 属性的值。

Tomcat 的内置 [PersistentManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html) 实现（如 [StandardManager](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Standard_Implementation) 或 [FileStore](https://tomcat.apache.org/tomcat-9.0-doc/config/manager.html#Nested_Components)）不适用于分布式缩放平台，如应用服务。 由于应用服务可能会在多个实例之间进行负载均衡，并随时以透明方式重启任何实例，因此不建议将可变状态持久保存到文件系统。

如果需要会话持久性，则需要使用会将内容写入外部数据存储的备用 `PersistentManager` 实现，例如使用 Redis 缓存的 VMware Tanzu 会话管理器。 有关详细信息，请参阅[将 Redis 作为会话缓存与 Tomcat 配合使用](/azure/app-service/containers/configure-language-java#use-redis-as-a-session-cache-with-tomcat)。

### <a name="special-cases"></a>特殊情况

某些生产方案可能需要进行其他更改或施加额外的限制。 虽然这种情况可能不怎么出现，但务必确保它们不适用于应用程序，或者在适用于应用程序的情况下已得到正确解决。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>确定应用程序是否依赖于计划的作业

计划的作业（如 Quartz 计划程序任务或 cron 作业）不能与应用服务配合使用。 应用服务不会阻止你部署内含计划任务的应用程序。 但是，如果应用程序横向扩展，则同一个计划的作业可能会按照计划期间运行多次。 这种情况可能会导致意外的后果。

清点应用程序服务器内外部所有计划的作业。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>确定应用程序是否包含特定于 OS 的代码

[!INCLUDE [determine-whether-your-application-contains-os-specific-code-no-title](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

#### <a name="determine-whether-tomcat-clustering-is-used"></a>确定是否使用了 Tomcat 聚类分析

Azure 应用服务不支持 [Tomcat 聚类分析](https://tomcat.apache.org/tomcat-9.0-doc/cluster-howto.html)。 可以改为通过 Azure 应用服务来配置和管理缩放和负载均衡，无需使用特定于 Tomcat 的功能。 可以将会话状态持久保存到备用位置，使之可以跨副本使用。 有关详细信息，请参阅[识别会话持久性机制](#identify-session-persistence-mechanism)。

若要确定应用程序是否使用聚类分析，请在 *server.xml* 文件中查找 `<Host>` 或 `<Engine>` 元素内的 `<Cluster>` 元素。

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>确定在生产服务器上运行的所有外部进程/守护程序

需要在其他位置进行迁移或消除在应用程序服务器外运行的任何进程，如监视守护程序。

#### <a name="determine-whether-non-http-connectors-are-used"></a>确定是否使用了非 HTTP 连接器

应用服务仅支持单个 HTTP 连接器。 如果应用程序需要其他连接器（如 AJP 连接器），请不要使用应用服务。

若要确定应用程序使用的 HTTP 连接器，请在 Tomcat 配置中查找 *server.xml* 文件中的 `<Connector>` 元素。

#### <a name="determine-whether-memoryrealm-is-used"></a>确定是否使用了 MemoryRealm

[MemoryRealm](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/realm/MemoryRealm.html) 需要一个持久的 XML 文件。 在 Azure 应用服务上，需要将此文件上传到 /home 目录或其子目录之一，或上传到装载的存储。 然后需要相应地修改 `pathName` 参数。

若要确定当前是否在使用 `MemoryRealm`，请检查 *server.xml* 和 *context.xml* 文件，并搜索已在其中将 `className` 属性设置为 `org.apache.catalina.realm.MemoryRealm` 的 `<Realm>` 元素。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>确定是否使用了 SSL 会话跟踪

应用服务在 Tomcat 运行时外部执行会话卸载，因此无法使用 [SSL 会话跟踪](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)。 请改用另一会话跟踪模式（`COOKIE` 或 `URL`）。 如果需要 SSL 会话跟踪，请不要使用应用服务。

#### <a name="determine-whether-accesslogvalve-is-used"></a>确定是否使用了 AccessLogValve

如果使用 [AccessLogValve](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/valves/AccessLogValve.html)，则应将 `directory` 参数设置为 `/home/LogFiles` 或其子目录之一。

## <a name="migration"></a>迁移

### <a name="parameterize-the-configuration"></a>将配置参数化

在预迁移步骤中，你可能已在 server.xml 和 context.xml 文件中标识了一些机密和外部依赖项（如数据源） 。 对于标识的每个项，请将任何用户名、密码、连接字符串或 URL 替换为环境变量。

例如，假设 *context.xml* 文件包含以下元素：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="jdbc:postgresql://postgresdb.contoso.com/wickedsecret?ssl=true"
    driverClassName="org.postgresql.Driver"
    username="postgres"
    password="t00secure2gue$$"
/>
```

在这种情况下，可以对其进行更改，如以下示例所示：

```xml
<Resource
    name="jdbc/dbconnection"
    type="javax.sql.DataSource"
    url="${postgresdb.connectionString}"
    driverClassName="org.postgresql.Driver"
    username="${postgresdb.username}"
    password="${postgresdb.password}"
/>
```

### <a name="provision-an-app-service-plan"></a>预配应用服务计划

从[应用服务定价](https://azure.microsoft.com/pricing/details/app-service/linux/)的可用服务计划列表中，选择其规格满足或超过当前生产硬件规格的计划。

> [!NOTE]
> 如果计划运行过渡/Canary 部署或使用部署槽位，应用服务计划必须包含该附加容量。 建议对 Java 应用程序使用高级或更高等级的计划。 有关详细信息，请参阅[设置 Azure 应用服务中的过渡环境](/azure/app-service/deploy-staging-slots)。

然后，创建应用服务计划。 有关详细信息，请参阅[在 Azure 中管理应用服务计划](/azure/app-service/app-service-plan-manage)。

### <a name="create-and-deploy-web-apps"></a>创建和部署 Web 应用

需要在应用服务计划（选择一个版本的 Tomcat 作为运行时堆栈）中针对部署到 Tomcat 服务器的每个 WAR 文件创建一个 Web 应用。

> [!NOTE]
> 虽然可以将多个 WAR 文件部署到单个 Web 应用，但这根本没有必要。 如果将多个 WAR 文件部署到单个 Web 应用，则会妨碍每个应用程序根据其自身的使用需求进行缩放。 另外，这样还会增加后续部署管道的复杂性。 如果单个 URL 上需要有多个可用的应用程序，请考虑使用路由解决方案，如 [Azure 应用程序网关](/azure/application-gateway/)。

#### <a name="maven-applications"></a>Maven 应用程序

如果应用程序是从 Maven POM 文件生成的，则请[使用 Maven 的 Webapp 插件](/azure/app-service/containers/quickstart-java#configure-the-maven-plugin)来创建 Web 应用并部署应用程序。

#### <a name="non-maven-applications"></a>非 Maven 应用程序

如果无法使用 Maven 插件，则需通过如下所示的其他机制来预配 Web 应用：

* [Azure 门户](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

创建 Web 应用后，请使用[提供的部署机制](/azure/app-service/deploy-zip)之一来部署应用程序。

### <a name="migrate-jvm-runtime-options"></a>迁移 JVM 运行时选项

如果应用程序需要特定的运行时选项，请[使用最适合的机制来指定它们](/azure/app-service/containers/configure-language-java#set-java-runtime-options)。

### <a name="populate-secrets"></a>填充机密

使用“应用程序设置”存储特定于应用程序的任何机密。 若要在多个应用程序中使用相同的机密，或者需要精细的访问策略和审核功能，请改为[使用 Azure Key Vault](/azure/app-service/containers/configure-language-java#use-keyvault-references)。

[!INCLUDE [configure-custom-domain-and-ssl](includes/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/import-backend-certificates.md)]

### <a name="migrate-data-sources-libraries-and-jndi-resources"></a>迁移数据源、库和 JNDI 资源

有关数据源配置步骤，请参阅[为 Azure 应用服务配置 Linux Java 应用](/azure/app-service/containers/configure-language-java)中的[数据源](/azure/app-service/containers/configure-language-java#data-sources)部分。

[!INCLUDE[Tomcat datasource additional instructions](includes/tomcat-datasource-additional-instructions.md)]

按照[数据源 JAR 文件的步骤](/azure/app-service/containers/configure-language-java#finalize-configuration)迁移任何其他服务器级 classpath 依赖关系。

迁移任何其他的[共享服务器级 JDNI 资源](/azure/app-service/containers/configure-language-java#shared-server-level-resources)。

> [!NOTE]
> 如果遵循一个 webapp 一个 WAR 这样一种建议的体系结构，请考虑将服务器级 classpath 库和 JNDI 资源迁移到应用程序中。 这会大大简化组件管理和变更管理。

### <a name="migrate-remaining-configuration"></a>迁移剩余配置

完成上述部分后，应在 */home/tomcat/conf* 中安装可自定义的服务器配置。

复制任何其他配置（如[领域](https://tomcat.apache.org/tomcat-9.0-doc/config/realm.html)和 [JASPIC](https://tomcat.apache.org/tomcat-9.0-doc/config/jaspic.html)），完成迁移。

[!INCLUDE [migrate-scheduled-jobs](includes/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>重启和冒烟测试

最后，需重启 Web 应用以应用所有配置更改。 重启完成后，请验证应用程序是否正常运行。

## <a name="post-migration"></a>迁移后

将应用程序迁移到 Azure 应用服务后，即应验证其运行是否符合预期。 完成此操作后，可以参考我们提供的一些建议，使应用程序的云原生性更好。

### <a name="recommendations"></a>建议

* 如果选择使用 */home* 目录进行文件存储，请考虑[将其替换为 Azure 存储](/azure/app-service/configure-connect-to-azure-storage)。

* 如果 /home 目录中的配置包含连接字符串、SSL 密钥和其他机密信息，请考虑结合使用 [Azure Key Vault](/azure/app-service/app-service-key-vault-references) 和/或[参数注入与应用程序设置](/azure/app-service/configure-common#configure-app-settings)（如果可能）。

* 请考虑[使用部署槽位](/azure/app-service/deploy-staging-slots)实现可靠的部署，不需停机。

* 设计和实施 DevOps 策略。 若要在提高开发速度的同时保持可靠性，请考虑[通过 Azure Pipelines 自动进行部署和测试](/azure/devops/pipelines/ecosystems/java-webapp)。 如果使用部署槽位，则可[自动部署到槽位](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)，然后进行槽位交换。

* 设计和实施业务连续性和灾难恢复策略。 对于关键应用程序，请考虑[多区域部署体系结构](/azure/architecture/reference-architectures/app-service-web-app/multi-region)。
