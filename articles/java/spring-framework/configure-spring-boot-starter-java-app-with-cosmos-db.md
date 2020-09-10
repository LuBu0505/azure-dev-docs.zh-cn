---
title: 如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
author: KarlErickson
ms.author: karler
ms.date: 10/02/2019
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 9770720ccf5cfd396c4f478b98b7e47c786ebf6e
ms.sourcegitcommit: 5ab6e90e20a87f9a8baea652befc74158a9b6613
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614307"
---
# <a name="how-to-use-the-spring-boot-starter-with-the-azure-cosmos-db-sql-api"></a>如何将 Spring Boot Starter 与 Azure Cosmos DB SQL API 配合使用

Azure Cosmos DB 是一种全球分布式数据库服务，它允许开发人员使用各种标准 API（如 SQL、MongoDB、Graph 和表 API）处理数据。 Microsoft 的 Spring Boot Starter 使开发人员可以使用 Spring Boot 应用程序，这些应用程序通过使用 SQL API 与 Azure Cosmos DB 轻松集成。

本文演示如何使用 Azure 门户创建 Azure Cosmos DB，如何使用 **[Spring Initializr]** 创建自定义 Spring Boot 应用程序，以及如何将[用于 Azure 的 Spring Boot Cosmos DB Starter] 添加到自定义应用程序，以使用 Spring Data 和 Cosmos DB SQL API 在 Azure Cosmos DB 中执行数据的存储和检索操作。

## <a name="prerequisites"></a>先决条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>使用 Azure 门户创建 Azure Cosmos DB

1. 浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“创建资源”。

1. 单击“数据库”，然后单击“Azure Cosmos DB” 。

    ![Azure 门户][AZ02]

1. 在“Azure Cosmos DB”页面上，输入以下信息：

    * 选择要用于数据库的订阅。
    * 指定是否为数据库创建新的“资源组”，或选择现有资源组。
    * 输入一个唯一的帐户名，作为数据库的 URI。 例如：*wingtiptoysdata*。
    * 为 API 选择 **Core (SQL)** 。
    * 为数据库指定“位置”。

    指定这些选项后，单击“查看 + 创建”，查看具体细节，然后单击“创建”。

    ![Azure 门户][AZ03]

1. 创建数据库之后，它将在 Azure 的“仪表板”、“所有资源”和“Azure Cosmos DB”页面下列出  。 在任意这些位置单击数据库可打开缓存的属性页面。

1. 当显示数据库的属性页面时，单击“密钥”，然后复制数据库的 URI 和访问密钥；在 Spring Boot 应用程序中会用到这些值。

    ![Azure 门户][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

请执行以下步骤，通过 Azure 支持创建一个新的 Spring Boot 应用程序项目。 也可使用 [azure-sdk-for-java](https://github.com/Azure/azure-sdk-for-java) 存储库中的 [azure-spring-boot-sample-cosmosdb](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-boot-sample-cosmosdb) 示例， 然后即可直接跳到[生成并测试应用](#build-and-test-your-app)。

1. 浏览到 <https://start.spring.io/>。

1. 指定你希望使用 Java 生成 Maven 项目，指定 Spring Boot 版本，输入应用程序的“组”名称和“项目”名称，在依赖项中添加“Azure 支持”，然后单击“生成项目”按钮      。

    ![Spring Initializr 的基本选项][SI01]

    > [!NOTE]
    > Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.example.wingtiptoysdata 。

1. 出现提示时，将项目下载到本地计算机中的路径，然后提取文件。

现在可以对简单的 Spring Boot 应用程序进行编辑了。

## <a name="configure-your-spring-boot-application-to-use-the-azure-spring-boot-starter"></a>配置 Spring Boot 应用程序，以使用 Azure Spring Boot Starter

1. 在应用的目录中找到 pom.xml 文件，例如：

    `C:\SpringBoot\wingtiptoysdata\pom.xml`

    -或-

    `/users/example/home/wingtiptoysdata/pom.xml`

1. 在文本编辑器中打开 pom.xml 文件，然后将以下内容添加到 `<dependencies>` 元素中：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-cosmosdb-spring-boot-starter</artifactId>
        <version>2.3.3</version>
    </dependency>
    ```

1. 验证 *properties* 元素是否指示 Java 和 Azure 的必需版本：

    ```xml
    <properties>
       <java.version>1.8</java.version>
       <azure.version>2.2.0</azure.version>
    </properties>
    ```

1. 保存并关闭 pom.xml 文件。

## <a name="configure-your-spring-boot-application-to-use-your-azure-cosmos-db"></a>配置 Spring Boot 应用程序以使用 Azure Cosmos DB

1. 在应用的“资源”目录中找到 application.properties 文件，例如 ：

    `C:\SpringBoot\wingtiptoysdata\src\main\resources\application.properties`

    -或-

    `/users/example/home/wingtiptoysdata/src/main/resources/application.properties`

1. 在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为数据库的相应属性：

    ```text
    # Specify the DNS URI of your Azure Cosmos DB.
    azure.cosmosdb.uri=https://wingtiptoys.documents.azure.com:443/

    # Specify the access key for your database.
    azure.cosmosdb.key=57686f6120447564652c20426f6220526f636b73==

    # Specify the name of your database.
    azure.cosmosdb.database=wingtiptoysdata
    ```

1. 保存并关闭 application.properties 文件。

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>添加示例代码以实现数据库的基本功能

本部分会创建两个存储用户数据的 Java 类，然后修改主应用程序类以创建 *User* 类的实例并将其保存到数据库。

### <a name="define-a-base-class-for-storing-user-data"></a>定义一个用于存储用户数据的基类

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 User.java 的新文件。

1. 在文本编辑器中打开 User.java 文件，然后将以下行添加到文件中，以定义在数据库中存储和检索值的通用用户类：

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document;
    import com.microsoft.azure.spring.data.cosmosdb.core.mapping.PartitionKey;
    import org.springframework.data.annotation.Id;

    @Document(collection = "mycollection")
    public class User {

        @Id
        private String id;
        private String firstName;

        @PartitionKey
        private String lastName;
        private String address;

        public User(String id, String firstName, String lastName, String address) {
            this.id = id;
            this.firstName = firstName;
            this.lastName = lastName;
            this.address = address;
        }

        public User() {
        }

        public String getId() {
            return id;
        }

        public void setId(String id) {
            this.id = id;
        }

        public String getFirstName() {
            return firstName;
        }

        public void setFirstName(String firstName) {
            this.firstName = firstName;
        }

        public String getLastName() {
            return lastName;
        }

        public void setLastName(String lastName) {
            this.lastName = lastName;
        }

        public String getAddress() {
            return address;
        }

        public void setAddress(String address) {
            this.address = address;
        }

        @Override
        public String toString() {
            return String.format("%s %s, %s", firstName, lastName, address);
        }
    }
    ```

1. 保存并关闭 User.java 文件。

### <a name="define-a-data-repository-interface"></a>定义数据存储库接口

1. 在与主应用程序 Java 文件相同的目录中创建一个名为 UserRepository.java 的新文件。

1. 在文本编辑器中打开 UserRepository.java 文件，然后将以下行添加到文件中，以定义可扩展默认 `ReactiveCosmosRepository` 接口的用户存储库接口：

    ```java
    package com.example.wingtiptoysdata;

    import com.microsoft.azure.spring.data.cosmosdb.repository.ReactiveCosmosRepository;
    import org.springframework.stereotype.Repository;
    import reactor.core.publisher.Flux;

    @Repository
    public interface UserRepository extends ReactiveCosmosRepository<User, String> {
        Flux<User> findByFirstName(String firstName);
    }
    ```

    `ReactiveCosmosRepository` 接口替换上一 Starter 版本中的 `DocumentDbRepository` 接口。 新接口提供同步和响应式 API，用于基本的保存、删除和查找操作。

1. 保存并关闭 UserRepository.java 文件。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用程序的程序包目录中找到主应用程序 Java 文件，例如：

    `C:\SpringBoot\wingtiptoysdata\src\main\java\com\example\wingtiptoysdata\WingtiptoysdataApplication.java`

    -或-

    `/users/example/home/wingtiptoysdata/src/main/java/com/example/wingtiptoysdata/WingtiptoysdataApplication.java`

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

    ```java
    package com.example.wingtiptoysdata;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.boot.CommandLineRunner;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.util.Assert;
    import reactor.core.publisher.Flux;
    import reactor.core.publisher.Mono;

    import javax.annotation.PostConstruct;
    import javax.annotation.PreDestroy;
    import java.util.Optional;

    @SpringBootApplication
    public class WingtiptoysdataApplication implements CommandLineRunner {

        private static final Logger LOGGER = LoggerFactory.getLogger(WingtiptoysdataApplication.class);

        @Autowired
        private UserRepository repository;

        public static void main(String[] args) {
            SpringApplication.run(WingtiptoysdataApplication.class, args);
        }

        public void run(String... var1) throws Exception {
            final User testUser = new User("1", "Tasha", "Calderon", "4567 Main St Buffalo, NY 98052");

            LOGGER.info("Saving user: {}", testUser);

            // Save the User class to Azure CosmosDB database.
            final Mono<User> saveUserMono = repository.save(testUser);

            final Flux<User> firstNameUserFlux = repository.findByFirstName("testFirstName");

            //  Nothing happens until we subscribe to these Monos.
            //  findById will not return the user as user is not present.
            final Mono<User> findByIdMono = repository.findById(testUser.getId());
            final User findByIdUser = findByIdMono.block();
            Assert.isNull(findByIdUser, "User must be null");

            final User savedUser = saveUserMono.block();
            Assert.state(savedUser != null, "Saved user must not be null");
            Assert.state(savedUser.getFirstName().equals(testUser.getFirstName()), "Saved user first name doesn't match");

            LOGGER.info("Saved user");

            firstNameUserFlux.collectList().block();

            final Optional<User> optionalUserResult = repository.findById(testUser.getId()).blockOptional();
            Assert.isTrue(optionalUserResult.isPresent(), "Cannot find user.");

            final User result = optionalUserResult.get();
            Assert.state(result.getFirstName().equals(testUser.getFirstName()), "query result firstName doesn't match!");
            Assert.state(result.getLastName().equals(testUser.getLastName()), "query result lastName doesn't match!");

            LOGGER.info("Found user by findById : {}", result);
        }

        @PostConstruct
        public void setup() {
            LOGGER.info("Clear the database");
            this.repository.deleteAll().block();
        }

        @PreDestroy
        public void cleanup() {
            LOGGER.info("Cleaning up users");
            this.repository.deleteAll().block();
        }
    }
    ```

1. 保存并关闭主应用程序 Java 文件。

## <a name="build-and-test-your-app"></a>生成并测试应用

1. 打开命令提示符并导航到 pom.xml 文件所在的文件夹，例如：

    `cd C:\SpringBoot\wingtiptoysdata`

    -或-

    `cd /users/example/home/wingtiptoysdata`

1. 使用以下命令生成并运行应用程序：

    ```console
    mvnw clean test
    ```

    该命令在测试阶段自动运行应用程序。 也可使用以下命令：

    ```console
    mvnw clean spring-boot:run
    ```

    在一些生成和测试输出后，控制台窗口会显示一条类似于以下内容的消息：

    ```console
      .   ____          _            __ _ _
     /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
    ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
     \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
      '  |____| .__|_| |_|_| |_\__, | / / / /
     =========|_|==============|___/=/_/_/_/
     :: Spring Boot ::            (v2.2.0.RC1)
    >
    > 2019-10-04 15:19:06.817  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Starting WingtiptoysdataApplicationTests on devmachine03 with PID 30013 (started by <user> in /d/source/repos/wingtiptoysdata)
    > 2019-10-04 15:19:06.818  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : No active profile set, falling back to default profiles: default
    > 2019-10-04 15:19:08.329  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.720  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 1369ms. Found 1 repository interfaces.
    > 2019-10-04 15:19:09.734  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data repositories in DEFAULT mode.
    > 2019-10-04 15:19:09.748  INFO 30013 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 13ms. Found 0 repository interfaces.

    ... (omitting Cosmos DB connection output) ...

    > 2019-10-04 15:19:46.584  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplicationTests    : Started WingtiptoysdataApplicationTests in 40.702 seconds (JVM running for 44.647)
    > 2019-10-04 15:19:46.587  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saving user: Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > 2019-10-04 15:19:47.122  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Saved user
    > 2019-10-04 15:19:47.289  INFO 30013 --- [           main] c.e.w.WingtiptoysdataApplication         : Found user by findById : Tasha Calderon, 4567 Main St Buffalo, NY 98052
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 44.003 s - in com.example.wingtiptoysdata.WingtiptoysdataApplicationTests
    > 2019-10-04 15:19:48.124  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.194  INFO 30013 --- [extShutdownHook] c.a.d.c.internal.RxDocumentClientImpl    : Shutting down ...
    > 2019-10-04 15:19:48.200  INFO 30013 --- [extShutdownHook] c.e.w.WingtiptoysdataApplication         : Cleaning up users
    > [INFO]
    > [INFO] Results:
    > [INFO]
    > [INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    > [INFO]
    > [INFO]
    > [INFO] --- maven-jar-plugin:3.1.2:jar (default-jar) @ wingtiptoysdata ---
    > [INFO] Building jar: /d/source/repos/wingtiptoysdata/target/wingtiptoysdata-0.0.1-SNAPSHOT.jar
    > [INFO]
    > [INFO] --- spring-boot-maven-plugin:2.2.0.RC1:repackage (repackage) @ wingtiptoysdata ---
    > [INFO] Replacing main artifact with repackaged archive
    > [INFO] ------------------------------------------------------------------------
    > [INFO] BUILD SUCCESS
    > [INFO] ------------------------------------------------------------------------
    > [INFO] Total time:  02:18 min
    > [INFO] Finished at: 2019-10-04T15:20:05-07:00
    > [INFO] ------------------------------------------------------------------------
    ```

    ![成功地从应用程序输出][JV02]

    `Saved user` 和 `Found user` 消息表明已成功将数据保存到 Cosmos DB，然后再次对其进行检索。

## <a name="clean-up-resources"></a>清理资源

如果不打算继续使用此应用程序，请务必删除包含此前创建的 Cosmos DB 的资源组。 可以从 Azure 门户获取此信息。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：

* [Azure Cosmos DB 文档]。

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [用于 Azure 的 Spring Boot Cosmos DB Starter]

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /azure/developer/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[用于 Azure 的 Spring Boot Cosmos DB Starter]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: https://azure.microsoft.com/services/devops/java/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ01.png
[AZ02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ02.png
[AZ03]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ03.png
[AZ04]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ04.png
[AZ05]: media/configure-spring-boot-starter-java-app-with-cosmos-db/AZ05.png

[SI01]: media/configure-spring-boot-starter-java-app-with-cosmos-db/SI01.png

[JV02]: media/configure-spring-boot-starter-java-app-with-cosmos-db/JV02.png
