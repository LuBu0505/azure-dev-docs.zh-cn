---
title: 将 Spring Data JPA 与 Azure Database for MySQL 配合使用
description: 了解如何将 Spring Data JPA 与 Azure Database for MySQL 数据库配合使用。
documentationcenter: java
ms.date: 10/12/2020
ms.service: mysql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 10e721b72bfbf3b5251f43996dbe6966659be2f3
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96441965"
---
# <a name="use-spring-data-jpa-with-azure-database-for-mysql"></a>将 Spring Data JPA 与 Azure Database for MySQL 配合使用

本主题演示如何创建示例应用程序，使其使用 [Spring Data JPA](https://spring.io/projects/spring-data-jpa) 在 [Azure Database for MySQL](/azure/mysql/) 中存储和检索信息。

[Java 持久性 API (JPA)](https://en.wikipedia.org/wiki/Java_Persistence_API) 是用于对象关系映射的标准 Java API。

[!INCLUDE [spring-data-prerequisites.md](includes/spring-data-prerequisites.md)]

## <a name="sample-application"></a>示例应用程序

在本文中，我们将对示例应用程序进行编码。 如果希望加快进程，可通过 [https://github.com/Azure-Samples/quickstart-spring-data-jpa-mysql](https://github.com/Azure-Samples/quickstart-spring-data-jpa-mysql) 获得已编码的应用程序。

[!INCLUDE [spring-data-mysql-setup.md](includes/spring-data-mysql-setup.md)]

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

通过在命令行中输入以下命令，生成此应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=web,data-jpa,mysql -d baseDir=azure-database-workshop -d bootVersion=2.3.4.RELEASE -d javaVersion=8 | tar -xzvf -
```


### <a name="configure-spring-boot-to-use-azure-database-for-mysql"></a>将 Spring Boot 配置为使用 Azure Database for MySQL

打开 src/main/resources/application.properties 文件，添加以下内容。 确保将这两个 `$AZ_DATABASE_NAME` 变量和 `$AZ_MYSQL_PASSWORD` 变量替换为在本文开头部分配置的值。

```properties
logging.level.org.hibernate.SQL=DEBUG

spring.datasource.url=jdbc:mysql://$AZ_DATABASE_NAME.mysql.database.azure.com:3306/demo?serverTimezone=UTC
spring.datasource.username=spring@$AZ_DATABASE_NAME
spring.datasource.password=$AZ_MYSQL_PASSWORD

spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
```

> [!WARNING]
> 配置属性 `spring.jpa.hibernate.ddl-auto=create-drop` 意味着 Spring Boot 会在应用程序启动时自动创建数据库架构，并在关闭时尝试将其删除。 此属性很适合用于测试，但不应在生产中使用！

> [!NOTE]
> 我们将 `?serverTimezone=UTC` 追加到配置属性 `spring.datasource.url` 中，以指示 JDBC 驱动程序在连接到数据库时使用 UTC 日期格式（或协调世界时）。 否则，Java 服务器将不使用与数据库相同的日期格式，这将导致错误。

现在，我们应该能够使用提供的 Maven 包装器启动应用程序：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png)](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-01.png#lightbox)

## <a name="code-the-application"></a>编写应用程序代码

接下来添加 Java 代码，以便使用 JPA 在 MySQL 服务器中存储并检索数据。

[!INCLUDE [spring-data-jpa-create-application.md](includes/spring-data-jpa-create-application.md)]

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png)](media/configure-spring-data-jpa-with-azure-mysql/create-mysql-02.png#lightbox)

祝贺你！ 你已创建了一个 Spring Boot 应用程序，该应用程序使用 JPA 在 Azure Database for MySQL 中存储和检索数据。

[!INCLUDE [spring-data-conclusion.md](includes/spring-data-conclusion.md)]

### <a name="additional-resources"></a>其他资源

有关 Spring Data JPA 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](../index.yml) 和[使用 Azure DevOps 和 Java](/azure/devops/)。
