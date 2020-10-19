---
title: 将 Spring Data R2DBC 用于 Azure SQL 数据库
description: 了解如何将 Spring Data R2DBC 用于 Azure SQL 数据库。
documentationcenter: java
ms.date: 10/10/2020
ms.service: sql-database
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 557480c4edf2d53cb04b3aa75ce829be3fe13760
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92009988"
---
# <a name="use-spring-data-r2dbc-with-azure-sql-database"></a>将 Spring Data R2DBC 用于 Azure SQL 数据库

本主题演示了如何通过使用 [r2dbc-mssql GitHub 存储库](https://github.com/r2dbc/r2dbc-mssql)中适用于 Microsoft SQL Server 的 R2DBC 实现创建一个示例应用程序，该应用程序使用 [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) 在 [Azure SQL 数据库](/azure/sql-database/)中存储和检索信息。

[R2DBC](https://r2dbc.io/) 将反应式 API 引入传统的关系数据库。 可以将它与 Spring WebFlux 配合使用，创建使用非阻止式 API 的完全响应式 Spring Boot 应用程序。 它提供的可伸缩性优于经典的“一个连接一个线程”方法。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>示例应用程序

在本文中，我们将对示例应用程序进行编码。 如果希望加快进程，可通过 [https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server](https://github.com/Azure-Samples/quickstart-spring-data-r2dbc-sql-server) 获得已编码的应用程序。

[!INCLUDE [spring-data-sql-server-setup.md](includes/spring-data-sql-server-setup.md)]

[!INCLUDE [spring-data-create-reactive.md](includes/spring-data-create-reactive.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

运行以下命令在命令行上生成应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```

> [!NOTE]
> Spring Initializr 使用 Java 11 作为默认版本。 若要使用本主题中所述的 Spring Boot 起动器，必须改为选择 Java 8。

### <a name="add-the-reactive-azure-sql-database-driver-implementation"></a>添加响应式 Azure SQL 数据库驱动程序实现

打开所生成项目的 pom.xml 文件，以添加来自 [r2dbc-mssql GitHub 存储库](https://github.com/r2dbc/r2dbc-mssql)的响应式 Azure SQL 数据库驱动程序。

在 `spring-boot-starter-webflux` 依赖项后，添加以下文本：

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-mssql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-sql-database"></a>将 Spring Boot 配置为使用 Azure SQL 数据库

打开 src/main/resources/application.properties 文件，添加以下文本：

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:mssql://$AZ_DATABASE_NAME.database.windows.net:1433/demo
spring.r2dbc.username=spring@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_SQL_SERVER_PASSWORD
```

将这两个 `$AZ_DATABASE_NAME` 变量和 `$AZ_SQL_SERVER_PASSWORD` 变量替换为在本文开头部分配置的值。

> [!NOTE]
> 为了提高性能，`spring.r2dbc.url` 属性配置为通过 [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) 使用连接池。

现在，你应能够使用提供的 Maven 包装器启动应用程序，如下所示：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-01.png#lightbox)

### <a name="create-the-database-schema"></a>创建数据库架构

[!INCLUDE [spring-data-r2dbc-create-schema.md](includes/spring-data-r2dbc-create-schema.md)]

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id INT IDENTITY PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BIT);
```

停止正在运行的应用程序并使用以下命令重启。 现在，该应用程序将使用之前创建的 `demo` 数据库，并在其中创建一个 `todo` 表。

```bash
./mvnw spring-boot:run
```

下面是正在创建的数据库表的屏幕截图：

[![创建数据库表](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-02.png#lightbox)

## <a name="code-the-application"></a>编写应用程序代码

接下来添加 Java 代码，以便使用 R2DBC 在 Azure SQL 数据库服务器中存储和检索数据。

[!INCLUDE [spring-data-r2dbc-create-application.md](includes/spring-data-r2dbc-create-application.md)]

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png)](media/configure-spring-data-r2dbc-with-azure-azure-sql/create-azure-sql-03.png#lightbox)

祝贺你！ 你已创建了一个完全响应式 Spring Boot 应用程序，该应用程序使用 R2DBC 在 Azure SQL 数据库中存储和检索数据。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>其他资源

有关 Spring Data R2DBC 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/r2dbc/docs/current/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](../index.yml) 和[使用 Azure DevOps 和 Java](/azure/devops/)。