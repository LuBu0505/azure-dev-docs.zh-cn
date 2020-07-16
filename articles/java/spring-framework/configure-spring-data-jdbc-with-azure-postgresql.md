---
title: 将 Spring Data JDBC 与 Azure Database for PostgreSQL 配合使用
description: 了解如何将 Spring Data JDBC 与 Azure Database for PostgreSQL 数据库配合使用。
documentationcenter: java
ms.date: 05/18/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: e6c95da37584db483d5acedaf738bfb0512a1de3
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86378911"
---
# <a name="use-spring-data-jdbc-with-azure-database-for-postgresql"></a>将 Spring Data JDBC 与 Azure Database for PostgreSQL 配合使用

本主题演示如何创建示例应用程序，使其使用 [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) 在 [Azure Database for PostgreSQL](/azure/postgresql/) 数据库中存储和检索信息。

[JDBC](https://en.wikipedia.org/wiki/Java_Database_Connectivity) 是标准的 Java API，用于连接到传统的关系数据库。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>示例应用程序

在本文中，我们将对示例应用程序进行编码。 如果希望加快进程，可通过 [https://github.com/Azure-Samples/quickstart-spring-data-jdbc-postgresql](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-postgresql) 获得已编码的应用程序。

[!INCLUDE [spring-data-postgresql-setup.md](includes/spring-data-postgresql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

使用以下命令在命令行上生成应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,postgresql -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>将 Spring Boot 配置为使用 Azure Database for PostgreSQL

打开 src/main/resources/application.properties 文件，添加以下文本：

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:postgresql://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/demo
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_POSTGRESQL_PASSWORD

spring.datasource.initialization-mode=always
```

将这两个 `$AZ_DATABASE_NAME` 变量和 `$AZ_POSTGRESQL_PASSWORD` 变量替换为在本文开头部分配置的值。

> [!WARNING]
> 配置属性 `spring.datasource.initialization-mode=always` 意味着 Spring Boot 将在每次服务器启动时使用我们稍后将创建的 schema.sql 文件来自动生成数据库架构。 这非常适合测试，但请记住，这会在每次重启时删除数据，因此你不应在生产中使用。

现在，你应能够使用提供的 Maven 包装器启动应用程序，如下所示：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-01.png#lightbox)

### <a name="create-the-database-schema"></a>创建数据库架构

Spring Boot 会自动执行 src/main/resources/schema.sql，以便创建数据库架构。 创建该文件并添加以下内容：

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

停止正在运行的应用程序并使用以下命令重启。 现在，该应用程序将使用之前创建的 `demo` 数据库，并在其中创建一个 `todo` 表。

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>编写应用程序代码

接下来，添加将使用 JDBC 在 PostgreSQL 服务器中存储和检索数据的 Java 代码。

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-jdbc-with-azure-postgresql/create-postgresql-02.png#lightbox)

祝贺你！ 你已创建了一个 Spring Boot 应用程序，该应用程序使用 JDBC 在 Azure Database for PostgreSQL 中存储和检索数据。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>其他资源

有关 Spring Data JDBC 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](/azure/developer/java/) 和[使用 Azure DevOps 和 Java](/azure/devops/)。
