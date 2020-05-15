---
title: 将 Spring Data R2DBC 与 Azure Database for PostgreSQL 配合使用
description: 了解如何将 Spring Data R2DBC 与 Azure Database for PostgreSQL 数据库配合使用。
documentationcenter: java
ms.date: 03/18/2020
ms.service: postgresql
ms.tgt_pltfrm: multiple
ms.author: judubois
ms.topic: article
ms.openlocfilehash: 1cc74fd296eeef8cf033fcf304ea577e04dddafd
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82801855"
---
# <a name="use-spring-data-r2dbc-with-azure-database-for-postgresql"></a>将 Spring Data R2DBC 与 Azure Database for PostgreSQL 配合使用

本主题演示了如何通过使用 [r2dbc-postgresql GitHub 存储库](https://github.com/r2dbc/r2dbc-postgresql)中适用于 PostgreSQL 的 R2DBC 实现创建一个示例应用程序，该应用程序使用 [Spring Data R2DBC](https://spring.io/projects/spring-data-r2dbc) 在 [Azure Database for PostgreSQL](https://docs.microsoft.com/azure/postgresql/) 中存储和检索信息。

[R2DBC](https://r2dbc.io/) 将响应式 API 引入传统的关系数据库。 可以将它与 Spring WebFlux 配合使用，创建使用非阻止式 API 的完全响应式 Spring Boot 应用程序。 它提供的可伸缩性优于经典的“一个连接一个线程”方法。

## <a name="prerequisites"></a>先决条件

- 一个 Azure 帐户。 如果没有帐户，可[获取一个免费试用帐户](https://azure.microsoft.com/free/)。
- [Azure Cloud Shell](/azure/cloud-shell/quickstart) 或 [Azure CLI](/cli/azure/install-azure-cli)。 建议使用 Azure Cloud Shell，这样你便可自动登录且有权访问所需的所有工具。
- [Java 8](https://www.azul.com/downloads/zulu/)（Azure Cloud Shell 中已随附）。
- 用来测试功能的 [cURL](https://curl.haxx.se) 或类似的 HTTP 实用工具。

## <a name="prepare-the-working-environment"></a>准备工作环境

首先，使用以下命令设置一些环境变量：

```bash
AZ_RESOURCE_GROUP=r2dbc-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_POSTGRESQL_USERNAME=r2dbc
AZ_POSTGRESQL_PASSWORD=<YOUR_POSTGRESQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

使用以下值替换占位符，在本文中将使用这些值：

- `<YOUR_DATABASE_NAME>`：PostgreSQL 服务器的名称。 它在 Azure 中应是唯一的。
- `<YOUR_AZURE_REGION>`：将使用的 Azure 区域。 默认情况下可以使用 `eastus`，但我们建议你配置一个离居住位置更近的区域。 可输入 `az account list-locations`，获取可用区域的完整列表。
- `<YOUR_POSTGRESQL_PASSWORD>`：PostgreSQL 数据库服务器的密码。 该密码应该至少有八个字符。 这些字符应该属于以下类别中的三个类别：英文大写字母、英文小写字母、数字 (0-9)和非字母数字字符（!, $, #, % 等）。
- `<YOUR_LOCAL_IP_ADDRESS>`：你将在其中运行 Spring Boot 应用程序的本地计算机的 IP 地址。 若要找到该地址，一种简便方法是使浏览器指向 [whatismyip.akamai.com](http://whatismyip.akamai.com/)。

接下来，创建一个资源组：

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> 我们使用默认安装在 [Azure Cloud Shell](https://shell.azure.com/) 上的 `jq` 实用工具来显示 JSON 数据，提高其可读性。
> 如果你不喜欢该实用工具，可安全地删除要使用的所有命令的 `| jq` 部分。

## <a name="create-an-azure-database-for-postgresql-instance"></a>创建 Azure Database for PostgreSQL 实例

首先，我们将创建一个托管 PostgreSQL 服务器。

> [!NOTE]
> 可以在[使用 Azure 门户创建 Azure Database for PostgreSQL 服务器](/azure/postgresql/quickstart-create-server-database-portal)中阅读有关创建 PostgreSQL 服务器的更多详细信息。

在 [Azure Cloud Shell](https://shell.azure.com/) 中运行以下脚本：

```azurecli
az postgres server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --sku-name B_Gen5_1 \
    --storage-size 5120 \
    --admin-user $AZ_POSTGRESQL_USERNAME \
    --admin-password $AZ_POSTGRESQL_PASSWORD \
    | jq
```

此命令创建小型 PostgreSQL 服务器。

### <a name="configure-a-firewall-rule-for-your-postgresql-server"></a>为 PostgreSQL 服务器配置防火墙规则

Azure Database for PostgreSQL 实例在默认情况下受保护。 它们有不允许任何传入连接的防火墙。 为了能够使用数据库，需要添加一项防火墙规则，允许本地 IP 地址访问数据库服务器。

由于我们已在本文开头配置了本地 IP 地址，因此可通过运行以下命令来打开服务器的防火墙：

```azurecli
az postgres server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-postgresql-database"></a>配置 PostgreSQL 数据库

你在前面创建的 PostgreSQL 服务器为空。 它没有任何可以与 Spring Boot 应用程序配合使用的数据库。 创建一个名为 `r2dbc`的新数据库：

```azurecli
az postgres db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name r2dbc \
    --server-name $AZ_DATABASE_NAME \
    | jq
```

## <a name="create-a-reactive-spring-boot-application"></a>创建响应式 Spring Boot 应用程序

若要创建一个响应式 Spring Boot 应用程序，可使用 [Spring Initializr](https://start.spring.io/)。 要创建的应用程序使用：

- Spring Boot 2.3.0 M4。
- Java 8（但它也可与 Java 11 等新版本一起使用）。
- 这些依赖项：Spring Reactive Web（也称为 Spring WebFlux）和 Spring Data R2DBC。

### <a name="generate-the-application-by-using-spring-initializr"></a>使用 Spring Initializr 生成应用程序

通过在命令行中输入以下命令，生成此应用程序：

```bash
curl https://start.spring.io/starter.tgz -d dependencies=webflux,data-r2dbc -d baseDir=azure-r2dbc-workshop -d bootVersion=2.3.0.M4 -d javaVersion=8 | tar -xzvf -
```

### <a name="add-the-reactive-postgresql-driver-implementation"></a>添加响应式 PostgreSQL 驱动程序实现

打开所生成项目的 pom.xml  文件，以添加来自 [GitHub 上的 r2dbc-postgresql 存储库](https://github.com/r2dbc/r2dbc-postgresql)的响应式 PostgreSQL 驱动程序。

在 `spring-boot-starter-webflux` 依赖项后，添加以下代码片段：

```xml
<dependency>
    <groupId>io.r2dbc</groupId>
    <artifactId>r2dbc-postgresql</artifactId>
    <scope>runtime</scope>
</dependency>
```

### <a name="configure-spring-boot-to-use-azure-database-for-postgresql"></a>将 Spring Boot 配置为使用 Azure Database for PostgreSQL

打开 src/main/resources/application.properties  文件，添加以下内容：

```properties
logging.level.org.springframework.data.r2dbc=DEBUG

spring.r2dbc.url=r2dbc:pool:postgres://$AZ_DATABASE_NAME.postgres.database.azure.com:5432/r2dbc
spring.r2dbc.username=r2dbc@$AZ_DATABASE_NAME
spring.r2dbc.password=$AZ_POSTGRESQL_PASSWORD
spring.r2dbc.properties.sslMode=REQUIRE
```

> [!WARNING]
> 出于安全原因，Azure Database for PostgreSQL 要求使用 SSL 连接。 这就是你需要添加 `spring.r2dbc.properties.sslMode=REQUIRE` 配置属性的原因，否则，R2DBC PostgreSQL 驱动程序将尝试使用不安全的连接进行连接，而这将会失败。

- 将两个 `$AZ_DATABASE_NAME` 变量替换为在本文开头部分配置的值。
- 将 `$AZ_POSTGRESQL_PASSWORD` 变量替换为在本文开头部分配置的值。

> [!NOTE]
> 为了提高性能，`spring.r2dbc.url` 属性配置为通过 [r2dbc-pool](https://github.com/r2dbc/r2dbc-pool) 使用连接池。

现在，我们应该能够使用提供的 Maven 包装器启动应用程序：

```bash
./mvnw spring-boot:run
```

下面是首次运行的应用程序的屏幕截图：

[![正在运行的应用程序](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-01.png#lightbox)

### <a name="create-the-database-schema"></a>创建数据库架构

在主 `DemoApplication` 类中配置新的 Spring Bean，它将创建数据库架构：

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

此 Spring Bean 使用名为 schema.sql  的文件，因此请在 src/main/resources  文件夹中创建该文件：

```sql
DROP TABLE IF EXISTS todo;
CREATE TABLE todo (id SERIAL PRIMARY KEY, description VARCHAR(255), details VARCHAR(4096), done BOOLEAN);
```

使用以下命令停止应用程序并再次运行它。 现在，该应用程序将使用之前创建的 `r2dbc` 数据库，并在其中创建一个 `todo` 表。

```bash
./mvnw spring-boot:run
```

下面是正在创建的数据库表的屏幕截图：

[![创建数据库表](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-02.png#lightbox)

## <a name="code-the-application"></a>编写应用程序代码

接下来添加 Java 代码，以便使用 R2DBC 在 PostgreSQL 服务器中存储和检索数据。

在 `DemoApplication` 类之后创建一个新的 `Todo` Java 类：

```java
package com.example.demo;

import org.springframework.data.annotation.Id;

public class Todo {

    public Todo() {
    }

    public Todo(String description, String details, boolean done) {
        this.description = description;
        this.details = details;
        this.done = done;
    }

    @Id
    private Long id;

    private String description;

    private String details;

    private boolean done;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }

    public boolean isDone() {
        return done;
    }

    public void setDone(boolean done) {
        this.done = done;
    }
}
```

此类是映射在之前创建的 `todo` 表上的域模型。

若要管理该类，你需要一个存储库。 在同一包中定义一个新的 `TodoRepository` 接口：

```java
package com.example.demo;

import org.springframework.data.repository.reactive.ReactiveCrudRepository;

public interface TodoRepository extends ReactiveCrudRepository<Todo, Long> {
}
```

此存储库是 Spring Data R2DBC 管理的响应式存储库。

创建可存储和检索数据的控制器，完成该应用程序。 在同一包中实现 `TodoController` 类，并添加以下代码：

```java
package com.example.demo;

import org.springframework.http.HttpStatus;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;
import reactor.core.publisher.Mono;

@RestController
@RequestMapping("/")
public class TodoController {

    private final TodoRepository todoRepository;

    public TodoController(TodoRepository todoRepository) {
        this.todoRepository = todoRepository;
    }

    @PostMapping("/")
    @ResponseStatus(HttpStatus.CREATED)
    public Mono<Todo> createTodo(@RequestBody Todo todo) {
        return todoRepository.save(todo);
    }

    @GetMapping("/")
    public Flux<Todo> getTodos() {
        return todoRepository.findAll();
    }
}
```

最后，暂停应用程序并再次启动它：

```bash
./mvnw spring-boot:run
```

## <a name="test-the-application"></a>测试应用程序

若要测试应用程序，可使用 cURL。

首先，在数据库中创建一个新的 todo 项：

```bash
curl  --header "Content-Type: application/json" \
          --request POST \
          --data '{"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done": "true"}' \
          http://127.0.0.1:8080
```

此命令应返回创建的项：

```json
{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}
```

接下来，使用新的 cURL 请求检索数据：

```bash
curl http://127.0.0.1:8080
```

此命令会返回“todo”项列表，其中包括已创建的项：

```json
[{"id":1,"description":"configuration","details":"congratulations, you have set up R2DBC correctly!","done":true}]
```

下面是这些 cURL 请求的屏幕截图：

[![使用 cURL 进行测试](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png)](media/configure-spring-data-r2dbc-with-azure-postgresql/create-postgresql-03.png#lightbox)

祝贺你！ 你已创建了一个完全响应式 Spring Boot 应用程序，该应用程序使用 R2DBC 在 Azure Database for PostgreSQL 中存储和检索数据。

## <a name="clean-up-resources"></a>清理资源

若要清理本快速入门中使用的所有资源，请删除该资源组：

```azurecli
az group delete \
    --name $AZ_RESOURCE_GROUP \
    --yes
```

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关 Spring Data R2DBC 的详细信息，请参阅 Spring 的[参考文档](https://docs.spring.io/spring-data/r2dbc/docs/1.0.x/reference/html/#reference)。

若要详细了解如何将 Azure 与 Java 配合使用，请参阅[面向 Java 开发人员的 Azure](/azure/developer/java/) 和[使用 Azure DevOps 和 Java](/azure/devops/)。