---
title: 将 Tomcat 应用程序迁移到 Azure Spring Cloud
description: 本指南介绍在需要将现有 Tomcat 应用程序迁移到 Azure Spring Cloud 时应注意的事项
author: yevster
ms.author: yebronsh
ms.topic: conceptual
ms.date: 6/16/2020
ms.openlocfilehash: 7b8a29c2769b3c4b04a40053d0470bfc6b1a0cca
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275181"
---
# <a name="migrate-a-tomcat-application-to-azure-spring-cloud"></a>将 Tomcat 应用程序迁移到 Azure Spring Cloud

本指南介绍在需要迁移现有 Tomcat 应用程序以使其在 Azure Spring Cloud 上运行时应注意的事项。

## <a name="pre-migration"></a>预迁移

若要确保迁移成功，请在开始之前完成以下各节中所述的评估和清点步骤。

### <a name="switch-to-a-supported-platform"></a>切换到受支持的平台

Azure Spring Cloud 提供特定版本的 Java SE。 若要确保兼容性，请在继续执行其余步骤之前，将应用程序迁移到当前环境的受支持版本之一。 务必全面测试生成的配置。 请在此类测试中使用最新且稳定的 Linux 发布版。

[!INCLUDE [note-obtain-your-current-java-version](includes/note-obtain-your-current-java-version.md)]

[!INCLUDE [determine-whether-and-how-the-file-system-is-used](includes/determine-whether-and-how-the-file-system-is-used.md)]

### <a name="identify-the-application-builddependency-system"></a>确定应用程序生成/依赖关系系统

确定用于生成/打包应用程序的工具，包括下载所有依赖项。

[!INCLUDE [inventory-external-resources](includes/inventory-external-resources.md)]

### <a name="inventory-secrets"></a>清点机密

#### <a name="passwords-and-secure-strings"></a>密码和安全字符串

检查生产服务器上的所有属性和配置文件中是否有机密字符串和密码。 务必检查 *$CATALINA_BASE/conf* 中的 *server.xml* 和 *context.xml*。 还可以在应用程序中查找包含密码或凭据的配置文件 (META-INF/context.xml)。

[!INCLUDE [inventory-certificates](includes/inventory-certificates.md)]

### <a name="determine-whether-your-application-contains-os-specific-code"></a>确定应用程序是否包含特定于 OS 的代码

[!INCLUDE [determine-whether-your-application-contains-os-specific-code-no-title](includes/determine-whether-your-application-contains-os-specific-code-no-title.md)]

[!INCLUDE [identify-all-outside-processes-and-daemons-running-on-the-production-servers](includes/identify-all-outside-processes-and-daemons-running-on-the-production-servers.md)]

### <a name="determine-whether-tomcat-is-connected-to-a-web-server"></a>确定 Tomcat 是否连接到 Web 服务器

Tomcat 可以通过 tomcat 连接器（例如 `mod_jk`）连接到 Apache 等静态 Web 服务器。 检查 `conf` 目录中的 `workers.properties` 文件，以确定是否存在此类连接。

### <a name="special-cases"></a>特殊情况

某些生产方案可能需要进行其他更改或施加额外的限制。 虽然这种情况可能不怎么出现，但务必确保它们不适用于应用程序，或者在适用于应用程序的情况下已得到正确解决。

#### <a name="determine-if-the-application-uses-filters"></a>确定应用程序是否使用筛选器

检查应用程序的 web.xml 文件中是否存在任何[配置的筛选器](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html#Expires_Filter/Basic_configuration_sample)。

[!INCLUDE [determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud](includes/determine-whether-your-application-relies-on-scheduled-jobs-azure-spring-cloud.md)]

#### <a name="determine-whether-non-http-connectors-are-used"></a>确定是否使用了非 HTTP 连接器

Azure Spring Cloud 仅支持单个不可自定义的 HTTP 端口上的 HTTP 连接。 如果应用程序需要其他端口或其他协议，请勿使用 Azure Spring Cloud。

若要确定应用程序使用的 HTTP 连接器，请在 Tomcat 配置中查找 *server.xml* 文件中的 `<Connector>` 元素。

#### <a name="determine-whether-ssl-session-tracking-is-used"></a>确定是否使用了 SSL 会话跟踪

在 Azure Spring Cloud 上，SSL 会话将在到达应用程序代码之前终止，因此你无法使用 [SSL 会话跟踪](https://tomcat.apache.org/tomcat-9.0-doc/servletapi/javax/servlet/SessionTrackingMode.html#SSL)。 你需要改用 [Spring 会话](https://docs.spring.io/spring-session/docs/current/reference/html5/index.html)。

#### <a name="determine-whether-tomcat-realms-are-used"></a>确定是否使用了 Tomcat 领域

在 Azure Spring Cloud 上，必须使用 Spring Security 来代替 Tomcat 领域。 检查 server.xml 文件，以清点任何[配置的领域](https://tomcat.apache.org/tomcat-9.0-doc/realm-howto.html#Configuring_a_Realm)。

#### <a name="determine-whether-servlet-filters-are-used"></a>确定是否使用了 servlet 筛选器

检查应用程序的 web.xml 文件中是否存在任何 `<filter>` 元素。 有关可用筛选器的列表，请参阅 [Tomcat 筛选器文档](https://tomcat.apache.org/tomcat-9.0-doc/config/filter.html)。

## <a name="migration"></a>迁移

### <a name="remove-connection-to-web-server-if-present"></a>删除与 Web 服务器的连接（如果存在）

如果 Tomcat 连接到静态 Web 服务器（通常通过 `mod_jk` 连接到 Apache），请禁用该连接，以便 Tomcat 作为独立服务器运行，并根据需要从标准服务器创建 Web 重定向。 考虑通过 Azure 内容分发网络 (CDN) 将静态 Web 内容迁移到 Azure 存储。 有关详细信息，请参阅 [Azure 存储中的静态网站托管](/azure/storage/blobs/storage-blob-static-website)和[快速入门：将 Azure 存储帐户与 Azure CDN 集成](/azure/cdn/cdn-create-a-storage-account-with-cdn)。

### <a name="update-to-tomcat-9"></a>更新到 Tomcat 9

如果当前应用程序在 Tomcat 9 之前的 Tomcat 版本上运行，请迁移到 Tomcat 9，并验证应用程序是否功能完备。 有关详细信息，请参阅 [Tomcat 9 迁移指南](http://tomcat.apache.org/migration-9.html)。

### <a name="switch-to-maven-or-gradle"></a>切换到 Maven 或 Gradle

Spring Boot 和 Spring Cloud 需要 Maven 或 Gradle 才可进行生成和依赖项管理。 如果应用程序使用的是其他生成或依赖项管理系统，请在继续操作之前切换到当前版本的 [Maven](https://maven.apache.org/download.cgi)。 虽然 Gradle 也受支持，但我们将在本指南的各个步骤中使用 Maven。 如果决定使用 Gradle，请相应地调整说明。

为应用程序创建一个 [POM 文件](https://maven.apache.org/pom.html)，并确保应用程序以完整的功能生成和运行，然后再继续。

### <a name="migrate-to-spring-boot"></a>迁移到 Spring Boot

下表显示了将 Tomcat 应用程序先后迁移到 Spring Boot 和 Azure Spring Cloud 所需的迁移和代码更改的摘要。 如果应用程序中使用了“旧版”列中的任何元素，则应将该元素替换为“最小迁移”或“建议的迁移”（最佳）列中的相应元素。

|旧的 | 检查位置 |最小迁移 |建议的迁移|
|---|---|---|---|
|[通过数据源建立 JDBC](https://docs.oracle.com/javase/tutorial/jdbc/basics/connecting.html)|server.xml|包含 [JDBC 模板](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template)的 [Spring Data 数据源](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-sql)|如果适合，请考虑 Spring Data 和 JPA。|
|Servlet |web.xml|启用 [Servlet 上下文初始化](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer)并通过 `@WebServlet` 进行批注|重写为 [Spring-MVC 控制器（通过 `@RestController`](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller)）
|筛选器 |web.xml| 启用 [Servlet 上下文初始化](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container-context-initializer)并[通过 `@WebFilter` 进行批注](https://docs.oracle.com/javaee/7/api/javax/servlet/annotation/WebFilter.html) |与“最小迁移”相同|
|Java 服务器页面 (JSP)|web.xml 和 WAR 文件内容|[Spring MVC 的 JSP 视图](https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-view-jsp)|单独托管视图层|
|Java 消息服务 (JMS)|server.xml|将连接工厂实例化为 [Spring Bean](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection)|使用 [Spring JMS](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/integration.html#jms-using)
|WAR 文件中的静态内容（图像、JavaScript 文件等）|静态内容目录（通常为 /static、/public 或 /resources）  |将内容移动到 /src/main/resources|请参阅[预迁移中的静态内容建议](#read-only-static-content)。
|WAR 文件外部的静态内容（图像、JavaScript 文件等）|本地文件系统上的路径|将内容移动到 src/main/resources。 在源代码中搜索硬编码路径，并将其替换为 [ClassPathResource](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/core/io/ClassPathResource.html) |请参阅[预迁移中的静态内容建议](#read-only-static-content)。

1. 如果应用程序依赖于通过 JNDI 资源（如 JDBC 驱动程序）注入的库，请将这些库作为依赖项添加到 POM 文件中。 从 Tomcat 服务器（通常是从 tomcat/lib 目录）中删除这些库，并在继续操作之前验证应用程序是否以完整功能运行。

1. 将 Spring Boot 父 POM 添加到 POM 文件。 有关详细信息，请参阅 Spring Boot 文档中的[创建 POM](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#getting-started-first-application-pom)。

1. 将 Spring Boot Tomcat Starter 作为依赖项添加到 POM 文件：

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    ```

    虽然这之前是一个 Tomcat 应用程序，但请勿添加 `war` 作为目标包。

1. 将 Tomcat 数据源替换为 Spring 数据源。 为应用程序使用的所有数据库[配置 Spring 数据源](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-configure-a-datasource)。 如果有任何代码执行直接 SQL 查询，请将其修改为[使用 JdbcTemplate](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-using-jdbc-template)。 请参阅 [Spring Framework 文档](https://docs.spring.io/spring/docs/current/spring-framework-reference/data-access.html#jdbc)和 [Spring 数据文档](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)，以获取事务管理和 CRUD 工具等其他数据访问功能。

1. 虽然可以在[嵌入的 servlet 容器](https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/html/spring-boot-features.html#boot-features-embedded-container)中包含 servlet 实现，但我们不建议这样做。 相反，请将 [servlet 实现](https://docs.oracle.com/javaee/7/api/javax/servlet/http/HttpServletRequest.html)替换为 Spring [Rest 控制器](https://spring.io/guides/gs/rest-service/#_create_a_resource_controller)。 如果应用程序使用非 Spring MVC 框架，请将其替换为 Spring MVC。 有关详细信息，请参阅 [ Spring MVC 批注的控制器参考](https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/web.html#mvc-controller)。

1. 通过 [Spring bean](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-spring-beans-and-dependency-injection) 重新创建所有其他 JNDI 依赖项。 支持使用 Spring 惯用的机制进行消息传递，例如使用 [Spring JMS](https://spring.io/guides/gs/messaging-jms/)。

1. 将 Tomcat 领域替换为 [Spring Security](https://docs.spring.io/spring-security/site/docs/current/reference/html5/#servlet-filters-review)。 考虑使用 Azure Active Directory 通过[用于 Active Directory 的 Spring Boot Starter](/azure/developer/java/spring-framework/spring-boot-starters-for-azure#azure-active-directory) 进行授权管理。

1. 通过 [Spring bean](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-as-spring-bean) 或[类路径扫描](https://docs.spring.io/spring-boot/docs/current/reference/html/howto.html#howto-add-a-servlet-filter-or-listener-using-scanning)重新创建在 web.xml 中配置的 Servlet 筛选器。

1. 如果应用程序包含或引用了静态内容（如图像或 JavaScript 文件），则应将这些文件移动到项目源代码中的 src/main/resources。 移动文件后，更新源代码以删除任何本地文件系统引用。 使用 Spring 的 `ClassPathResource` 类访问此类文件。

通过运行 `mvn spring-boot:run` 来测试应用程序。 验证生成的应用程序是否以完整功能运行，然后再继续。

[!INCLUDE [migrate-steps-spring-boot-azure-spring-cloud](includes/migrate-steps-spring-boot-azure-spring-cloud.md)]

## <a name="post-migration"></a>迁移后

[!INCLUDE [post-migration-spring-boot-azure-spring-cloud](includes/post-migration-spring-boot-azure-spring-cloud.md)]
