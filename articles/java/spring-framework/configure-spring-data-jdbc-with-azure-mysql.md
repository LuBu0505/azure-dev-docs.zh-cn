---
title: 将 Spring Data JDBC 与 Azure Database for MySQL 配合使用
description: 了解如何将 Spring Data JDBC 与 Azure Database for MySQL 数据库配合使用。
documentationcenter: java
ms.date: 04/07/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 481b3fce5d7d5f62cf4639d53d86c092e364a74f
ms.sourcegitcommit: 04ee2325e3efd9b7797102b4cd9d5db009c38a42
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/22/2020
ms.locfileid: "86945805"
---
# <a name="use-spring-data-jdbc-with-azure-database-for-mysql"></a>将 Spring Data JDBC 与 Azure Database for MySQL 配合使用

本主题演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data JDBC](https://spring.io/projects/spring-data-jdbc) 在 [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) 中存储和检索信息。

[JDBC](https://jcp.org/en/jsr/detail?id=221) 是标准的 Java API，用于连接到传统的关系数据库。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>示例应用程序

在本文中，我们将对示例应用程序进行编码。 如果希望加快进程，可通过 [https://github.com/Azure-Samples/quickstart-spring-data-jdbc-mysql](https://github.com/Azure-Samples/quickstart-spring-data-jdbc-mysql) 获得已编码的应用程序。

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

通过在命令行中输入以下命令，生成此应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jdbc,mysql -d baseDir=azure-database-workshop -d bootVersion=2.3.1.RELEASE -d javaVersion=8 | tar -xzvf -
```

### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>将 Spring Boot 配置为使用 Azure Database for MySQL

打开 src/main/resources/application.properties 文件，添加以下内容：

```properties
logging.level.org.springframework.jdbc.core=DEBUG

spring.datasource.url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_MYSQL_PASSWORD

spring.datasource.initialization-mode=always
```

- 将两个 `$AZ_DATABASE_NAME` 变量替换为在本文开头部分配置的值。
- 将 `$AZ_MYSQL_PASSWORD` 变量替换为在本文开头部分配置的值。

> [!WARNING]
> 配置属性 `spring.datasource.initialization-mode=always` 意味着 Spring Boot 将在每次服务器启动时使用我们稍后将创建的 `schema.sql` 文件来自动生成数据库架构。 这非常适合测试，但请记住，这会在每次重启时删除数据，因此不应在生产中使用！

> [!NOTE]
> 我们将 `?serverTimezone=UTC` 追加到配置属性 `spring.datasource.url` 中，以指示 JDBC 驱动程序在连接到数据库时使用 UTC 日期格式（或协调世界时）。 否则，Java 服务器将不使用与数据库相同的日期格式，这将导致错误。

现在，我们应该能够使用提供的 Maven 包装器启动应用程序：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-01.png#lightbox)

### <a name="create-the-database-schema"></a>创建数据库架构

Spring Boot 会自动执行 src/main/resources/`schema.sql`，以便创建数据库架构。 创建包含以下内容的文件：

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

停止正在运行的应用程序并重启它。 现在，该应用程序将使用之前创建的 `demo` 数据库，并在其中创建一个 `todo` 表。

```bash
./mvnw spring-boot:run
```

## <a name="code-the-application"></a>编写应用程序代码

接下来添加 Java 代码，以便使用 JDBC 在 MySQL 服务器中存储并检索数据。

[!INCLUDE [spring-data-jdbc-create-application.md](includes/spring-data-jdbc-create-application.md)]

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-jdbc-with-azure-mysql/create-mysql-02.png#lightbox)

祝贺你！ 你已创建了一个 Spring Boot 应用程序，该应用程序使用 JDBC 在 Azure Database for MySQL 中存储和检索数据。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>其他资源

有关 Spring Data JDBC 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](/azure/developer/java/) 和[使用 Azure DevOps 和 Java](/azure/devops/)。
