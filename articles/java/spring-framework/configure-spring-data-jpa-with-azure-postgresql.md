---
title: 将 Spring Data JPA 与 Azure Database for PostgreSQL 配合使用
description: 了解如何将 Spring Data JPA 与 Azure Database for PostgreSQL 数据库配合使用。
documentationcenter: java
ms.date: 06/19/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 50b2c1e560fa82f7fc6dfd438f31cf0e5cd283b4
ms.sourcegitcommit: 7da78b35a847db9929554962dfcc47860f472fb9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133682"
---
# <a name="use-spring-data-jpa-with-azure-database-for-postgresql"></a>将 Spring Data JPA 与 Azure Database for PostgreSQL 配合使用

本主题演示如何创建示例应用程序，使其使用 [Spring Data JPA](https://spring.io/projects/spring-data-jpa) 在 [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/) 中存储和检索信息。

[Java 持久性 API (JPA)](https://en.wikipedia.org/wiki/Java_Persistence_API) 是用于对象关系映射的标准 Java API。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

通过在命令行中输入以下命令，生成此应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,postgresql -d baseDir=azure-database-workshop -d bootVersion=2.3.0.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>将 Spring Boot 配置为使用 Azure Database for PostgreSQL

打开 src/main/resources/application.properties 文件，添加以下内容。 确保将这两个 `$AZ_DATABASE_NAME` 变量和 `$AZ_POSTGRESQL_PASSWORD` 变量替换为在本文开头部分配置的值。

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:postgresql://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_POSTGRESQL_PASSWORD

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
```

> [!WARNING]
> 配置属性 `spring.jpa.hibernate.ddl-auto=create-drop` 意味着 Spring Boot 会在应用程序启动时自动创建数据库架构，并在关闭时尝试将其删除。 此属性很适合用于测试，但不应在生产中使用！

现在，我们应该能够使用提供的 Maven 包装器启动应用程序：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-01.png#lightbox)

## <a name="code-the-application"></a>编写应用程序代码

接下来添加 Java 代码，以便使用 JPA 在 PostgreSQL 服务器中存储和检索数据。

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-jpa-with-azure-postgresql/create-postgresql-02.png#lightbox)

祝贺你！ 你已创建了一个 Spring Boot 应用程序，该应用程序使用 JPA 在 Azure Database for PostgreSQL 中存储和检索数据。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>其他资源

有关 Spring Data JPA 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](/azure/developer/java/) 和[使用 Azure DevOps 和 Java](/azure/devops/)。
