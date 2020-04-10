---
title: 将 Java SE 应用程序迁移到 Azure 应用服务上的 Java SE
description: 本指南介绍在需要将现有 Spring Boot 或其他单独的 Web 应用程序迁移到使用 Java SE 的 Azure 应用服务时应注意的事项。
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 01/22/2019
ms.openlocfilehash: 91292d50f49bde2b76084f8a09119ae74a20f72f
ms.sourcegitcommit: 951fc116a9519577b5d35b6fb584abee6ae72b0f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2020
ms.locfileid: "80612119"
---
# <a name="migrate-executable-jar-web-applications-to-java-se-on-azure-app-service"></a>将可执行的 JAR Web 应用程序迁移到 Azure 应用服务上的 Java SE

本指南介绍在需要将现有 Spring Boot 或其他嵌入式服务器 Web 应用程序迁移到使用 Java SE 的 Azure 应用服务时应注意的事项。

## <a name="before-you-start"></a>开始之前

如果无法满足任何预迁移要求，请参阅以下伴随迁移指南：

* 将可执行的 JAR 应用程序迁移到 Azure Kubernetes 服务上的容器（按指南进行计划）
* 将可执行的 JAR 应用程序迁移到 Azure 虚拟机（按指南进行计划）

## <a name="pre-migration"></a>预迁移

### <a name="switch-to-a-supported-platform"></a>切换到受支持的平台

应用服务提供特定版本的 Java SE。 若要确保兼容性，请在继续执行其余步骤之前，将应用程序迁移到当前环境的受支持版本之一。 务必全面测试生成的配置。 请在此类测试中使用最新且稳定的 Linux 发布版。

[!INCLUDE [note-obtain-your-current-java-version](includes/migration/note-obtain-your-current-java-version.md)]

### <a name="inventory-external-resources"></a>清点外部资源

标识外部资源，如数据源、JMS 消息代理和其他服务的 URL。 在 Spring Boot 应用程序中，通常可在通常名为 *application.properties* 或 *application.yml* 的文件的 *src/main/directory* 中找到此类资源的配置。 此外，请查看生产部署的环境变量，了解任何相关的配置设置。

#### <a name="databases"></a>数据库

确定任何 SQL 数据库的连接字符串。

Spring Boot 应用程序的连接字符串通常出现在配置文件中。

下面是 *application.properties* 文件中的示例：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

下面是 *application.yaml* 文件中的示例：

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

有关详细信息，请参阅 Spring 文档中的 [JPA 存储库](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)和 [JDBC 存储库](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)。

#### <a name="jms-message-brokers"></a>JMS 消息代理

确定正在使用的代理。 应该可以通过检查相关依赖项的生成清单（通常为 *pom.xml* 或 *build.gradle*）来实现此目的。

例如，使用 ActiveMQ 的 Spring Boot 应用程序通常会在 *pom.xml* 中包含此依赖项：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

使用专用代理的 Spring Boot 应用程序通常直接在代理的 JMS 驱动程序库中包含依赖项。 下面是 *build.gradle* 文件中的示例：

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```

确定所使用的代理后，请找到相应的设置（通常在 Spring Boot 的 *application.properties* 和 *application.yml* 文件中）。

下面是 *application.properties* 文件中的示例：

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

下面是 *application.yaml* 文件中的示例：

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

#### <a name="all-other-external-resources"></a>所有其他的外部资源

在本指南中，不可能记录每个可能的外部依赖项。 你的团队负责验证你是否可以在进行应用服务迁移之后满足应用程序的所有外部依赖项的要求。

### <a name="inventory-secrets"></a>清点机密

#### <a name="passwords-and-secure-strings"></a>密码和安全字符串

检查生产部署上的所有属性和配置文件以及所有环境变量中是否有机密字符串和密码。 在 Spring Boot 应用程序中，此类字符串可能位于 *application.properties* 或 *application.yml* 中。

[!INCLUDE [inventory-certificates](includes/migration/inventory-certificates.md)]

[!INCLUDE [inventory-persistence-usage](includes/migration/inventory-persistence-usage.md)]

### <a name="special-cases"></a>特殊情况

某些生产方案可能需要进行其他更改或施加额外的限制。 虽然这种情况可能不怎么出现，但务必确保它们不适用于应用程序，或者在适用于应用程序的情况下已得到正确解决。

#### <a name="determine-whether-application-relies-on-scheduled-jobs"></a>确定应用程序是否依赖于计划的作业

计划的作业（如 Quartz 计划程序任务或 cron 作业）不能与应用服务配合使用。 应用服务不会阻止你部署内含计划任务的应用程序。 但是，如果应用程序横向扩展，则同一个计划的作业可能会按照计划期间运行多次。 这种情况可能会导致意外的后果。

清点应用程序进程内外部所有计划的作业。

#### <a name="determine-whether-your-application-contains-os-specific-code"></a>确定应用程序是否包含特定于 OS 的代码

如果应用程序包含任何要适应运行应用程序的 OS 的代码，则需对应用程序进行重构，使之不依赖于基础 OS。 例如，可能需要将文件系统路径中使用的 `/` 或 `\` 替换为 [`File.Separator`](https://docs.oracle.com/javase/8/docs/api/java/io/File.html#separator) 或 [`Path.get`](https://docs.oracle.com/javase/8/docs/api/java/nio/file/Paths.html#get-java.lang.String-java.lang.String...-)。

#### <a name="identify-all-outside-processesdaemons-running-on-the-production-servers"></a>确定在生产服务器上运行的所有外部进程/守护程序

在应用程序服务器外运行的进程（如监视守护程序）需要迁移到其他位置，或者需要将其消除。

#### <a name="identify-handling-of-non-http-requests-or-multiple-ports"></a>确定对非 HTTP 请求或多个端口的处理

应用服务仅支持单个端口上的单个 HTTP 终结点。 如果应用程序在多个端口上进行侦听，或者接受使用 HTTP 以外协议的请求，请不要使用 Azure 应用服务。

## <a name="migration"></a>迁移

### <a name="parameterize-the-configuration"></a>将配置参数化

请确保所有外部资源坐标（例如数据库连接字符串）和其他可自定义的设置可以从环境变量中读取。 如果要迁移 Spring Boot 应用程序，则所有配置设置都应该已经可外部化。 有关详细信息，请参阅 Spring Boot 文档中的[外部化配置](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)。

下面是一个从 *application.properties* 文件引用 `SERVICEBUS_CONNECTION_STRING` 环境变量的示例：

```properties
spring.jms.servicebus.connection-string=${SERVICEBUS_CONNECTION_STRING}
spring.jms.servicebus.topic-client-id=contoso1
spring.jms.servicebus.idle-timeout=10000
```

### <a name="provision-an-app-service-plan"></a>预配应用服务计划

从[可用服务计划列表](https://azure.microsoft.com/pricing/details/app-service/linux/)中，选择其规格满足或超过当前生产硬件规格的计划。

> [!NOTE]
> 如果计划运行过渡/Canary 部署或使用[部署槽位](/azure/app-service/deploy-staging-slots)，应用服务计划必须包含该附加容量。 建议对 Java 应用程序使用高级或更高等级的计划。

[创建应用服务计划](/azure/app-service/app-service-plan-manage#create-an-app-service-plan)。

### <a name="create-and-deploy-web-apps"></a>创建和部署 Web 应用

需要在应用服务计划中针对每个要运行的可执行 JAR 文件创建 Web 应用（选择“Java SE”作为运行时堆栈）。

#### <a name="maven-applications"></a>Maven 应用程序

如果应用程序是从 Maven POM 文件生成的，则请[使用 Maven 的 Webapp 插件](/azure/java/spring-framework/deploy-spring-boot-java-app-with-maven-plugin#configure-maven-plugin-for-azure-app-service)来创建 Web 应用并部署应用程序。

#### <a name="non-maven-applications"></a>非 Maven 应用程序

如果无法使用 Maven 插件，则需通过如下所示的其他机制来预配 Web 应用：

* [Azure 门户](https://portal.azure.com/#create/Microsoft.WebSite)
* [Azure CLI](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)
* [Azure PowerShell](/powershell/module/az.websites/new-azwebapp)

创建 Web 应用后，请使用[提供的部署机制](/azure/app-service/deploy-ftp)之一来部署应用程序。 如果可能，应将应用程序上传到 */home/site/wwwroot/app.jar*。 如果不希望将 JAR 重命名为 app.jar，则可使用命令上传 shell 脚本来运行 JAR。 然后，将此脚本的完整路径粘贴到门户“配置”部分的[启动文件](/azure/app-service/containers/app-service-linux-faq#built-in-images)文本框中。 启动脚本不从放置它的目录运行。 因此，请始终使用绝对路径在启动脚本中引用文件（例如：`java -jar /home/myapp/myapp.jar`）。

### <a name="migrate-jvm-runtime-options"></a>迁移 JVM 运行时选项

如果应用程序需要特定的运行时选项，请[使用最适合的机制来指定它们](/azure/app-service/containers/configure-language-java#set-java-runtime-options)。

[!INCLUDE [configure-custom-domain-and-ssl](includes/migration/configure-custom-domain-and-ssl.md)]

[!INCLUDE [import-backend-certificates](includes/migration/import-backend-certificates.md)]

### <a name="migrate-external-resource-coordinates-and-other-settings"></a>迁移外部资源坐标和其他设置

执行[用于迁移连接字符串和其他设置的这些步骤](/azure/app-service/containers/configure-language-java#spring-boot-1)。

> [!NOTE]
> 对于任何已使用[将配置参数化](#parameterize-the-configuration)部分的变量参数化的 Spring Boot 应用程序设置，必须在应用程序配置中定义这些环境变量。 任何未使用环境变量显式参数化的 Spring Boot 应用程序设置仍可由它们通过“应用程序配置”重写。 例如：

  ```properties
  spring.jms.servicebus.connection-string=${CUSTOMCONNSTR_SERVICE_BUS}
  spring.jms.servicebus.topic-client-id=contoso1
  spring.jms.servicebus.idle-timeout=1800000
  ```

![应用服务应用程序配置](media/migrate-java-se-to-java-se-app-service/app-service-parameterized-spring-boot-app-settings.png)

[!INCLUDE [migrate-scheduled-jobs](includes/migration/migrate-scheduled-jobs.md)]

### <a name="restart-and-smoke-test"></a>重启和冒烟测试

最后，需重启 Web 应用以应用所有配置更改。 重启完成后，请验证应用程序是否正常运行。

## <a name="post-migration"></a>迁移后

将应用程序迁移到 Azure 应用服务后，即应验证其运行是否符合预期。 完成此操作后，可以参考我们提供的一些建议，使应用程序的云原生性更好。

### <a name="recommendations"></a>建议

* 如果选择使用 */home* 目录进行文件存储，请考虑[将其替换为 Azure 存储](/azure/app-service/containers/how-to-serve-content-from-azure-storage)。

* 如果 */home* 目录中的配置包含连接字符串、SSL 密钥和其他机密信息，请考虑使用 [Azure Key Vault](/azure/app-service/app-service-key-vault-references) 和/或[参数注入与应用程序设置](/azure/app-service/configure-common#configure-app-settings)（如果可能）。

* 请考虑[使用部署槽位](/azure/app-service/deploy-staging-slots)实现可靠的部署，不需停机。

* 设计和实施 DevOps 策略。 若要在提高开发速度的同时保持可靠性，请考虑[通过 Azure Pipelines 自动进行部署和测试](/azure/devops/pipelines/ecosystems/java-webapp)。 如果使用部署槽位，则可[自动部署到槽位](/azure/devops/pipelines/targets/webapp?view=azure-devops&tabs=yaml#deploy-to-a-slot)并进行后续的槽位交换。

* 设计和实施业务连续性和灾难恢复策略。 对于关键应用程序，请考虑[多区域部署体系结构](/azure/architecture/reference-architectures/app-service-web-app/multi-region)。
