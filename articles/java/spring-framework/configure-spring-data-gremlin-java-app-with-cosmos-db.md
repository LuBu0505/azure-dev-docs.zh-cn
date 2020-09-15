---
title: 如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializr 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
ms.date: 08/03/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 4a19b6dff945cb04d2b726b546e362c261a00595
ms.sourcegitcommit: 5ab6e90e20a87f9a8baea652befc74158a9b6613
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614332"
---
# <a name="how-to-use-the-spring-data-gremlin-starter-with-the-azure-cosmos-db-sql-api"></a>如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用

## <a name="overview"></a>概述

Spring Data Gremlin Starter 为 Apache 中的 Gremlin 查询语言提供 Spring Data 支持，开发人员可对 Gremlin 兼容的任何数据存储使用这项支持。

本文演示如何使用 Azure 门户创建可与 Gremlin API 配合使用的 Azure Cosmos DB，使用 **[Spring Initializr]** 创建自定义的 Java 应用程序，然后将 Spring Data Gremlin Starter 功能添加到自定义应用程序用于存储数据，并使用 Gremlin 从 Azure Cosmos DB 检索数据。

## <a name="prerequisites"></a>必备条件

为遵循本文介绍的步骤，需要以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。


## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>使用 Azure 门户创建 Cosmos DB 帐户

1. 浏览转到 Azure 门户 (<https://portal.azure.com/>)，然后选择 `+Create a resource`。

   >[!div class="mx-imgBorder"]
   >![create-a-resource][create-a-resource-01]

1. 依次选择“`Databases`”、“`Azure Cosmos DB`”。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db][create-a-resource-02]

1. 在 `Azure Cosmos DB` 页面上，输入以下信息：

   * 选择要用于数据库的 `Subscription`。
   * 指定是为数据库创建新的 `Resource Group` 还是选择现有资源组。
   * 输入唯一 `Account Name` 用作数据库 Gremlin URI 的一部分。 例如：如果为 `Account Name` 输入 `account-sample`，则 Gremlin URI 将为 `account-samplewingtiptoysdata.gremlin.cosmosdb.azure.com`。
   * 为 API 选择 `Gremlin (Graph)` 。
   * 为数据库指定 `Location`。
   
1. 指定这些选项后，选择 `Review + create`。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db-account][create-a-resource-03]

1. 查看具体细节，然后选择 `Create` 以创建数据库。

1. 数据库已创建后，选择“转到资源”。 数据库将在 Azure 的“仪表板”以及“所有资源”和“Azure Cosmos DB”页面下列出  。 在任意这些位置选择数据库可打开缓存的属性页。

1. 当显示数据库的属性页时，选择“密钥”，然后复制数据库的 URI 和访问密钥；在 Spring Boot 应用程序中会用到这些值。

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>将图形添加到 Azure Cosmos 数据库

1. 在 Cosmos DB 页面上，选择 `Data Explorer`，然后选择 `New Graph`。

   >[!div class="mx-imgBorder"]
   >![new-graph][create-a-graph-01]

1. 显示 `Add Graph` 后，输入以下信息：

   * 指定数据库的唯一 `Database id`。
   * 可选择指定 `Storage capacity`，也可接受默认值。
   * 指定图形的唯一 `Graph id`。
   * 指定 `Partition key`。 有关详细信息，请参阅[图形分区]。 选择 `OK`。
   
   指定这些选项后，选择 `OK` 以创建图形。

   >[!div class="mx-imgBorder"]
   >![add-graph][create-a-graph-02]

1. 创建图形后，可使用 `Data Explorer` 来查看它。

   >[!div class="mx-imgBorder"]
   >![graph-detail][create-a-graph-03]
   
   

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定希望使用 Java 生成 Maven 项目，输入应用程序的“组”名称和“项目”名称，指定 Spring Boot 版本为版本 2.3.1，然后选择“生成”     。

> [!NOTE]
>
> Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：`com.example.wintiptoysdata` 。


   >[!div class="mx-imgBorder"]
   >![spring-initializr][spring-initializr-01]

1. 出现提示时，将项目下载到本地计算机中的路径。

1. 提取本地系统上的文件后，将其导入 IDE。


## <a name="configure-your-spring-boot-app-to-use-the-spring-data-gremlin-starter"></a>将 Spring Boot 应用配置为使用 Spring Data Gremlin Starter

我们将复制现有 [Azure Spring Data Gremlin 示例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-data-sample-gremlin)的配置。 浏览到该示例并按照本部分中的步骤配置 Spring Boot 应用。

1. 在应用的目录中找到 pom.xml 文件，例如：

   C:\SpringBoot\wingtiptoysdata\pom.xml

   - 或 -

   /users/example/home/wingtiptoysdata/pom.xml

1. 打开 pom.xml 文件，将 Spring Data Gremlin Starter 添加到 `<dependencies>` 列表：

   ```xml
   <dependency>
      <groupId>com.azure</groupId>
      <artifactId>azure-spring-data-gremlin</artifactId>
      <version>2.3.1-beta.1</version> <!-- {x-version-update;com.azure:azure-spring-data-gremlin;current} -->
    </dependency>
   ```

1. 保存并关闭 pom.xml 文件。

1. 导航到“src/test/”文件夹，然后删除所有内容。

1. 导航到示例应用中的“src/main/java”文件夹，复制同一目录并覆盖到本地的 Spring Boot 应用中。

1. 在 src/main/resources/properties 文件中，更新配置，使其包括以下内容：

   | 字段              | 说明                                                                                                                                                                                                             |
   |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `endpoint`         | 指定数据库的 Gremlin URI，此 URI 派生自前面在本快速入门中创建 Azure Cosmos DB 时指定的唯一 ID。                                                 |
   | `port`             | 指定 TCP/IP 端口，对于 HTTPS 通信，应是 **443**。                                                                                                                                                           |
   | `username`         | 指定前面在本快速入门中添加图形时使用的唯一数据库 ID 和图形 ID；必须使用以下语法输入这些值：“/dbs/{Database ID}/colls/{Graph ID}”   。 |
   | `password`         | 指定前面在本快速入门中复制的主要或辅助访问密钥。                                                                                                                      |
   | `sslEnabled`       | 指定是否启用 SSL。                                                                                                                                                                                           |
   | `telemetryAllowed` | 若要启用遥测，请指定 **true**；否则指定 **false**。
   | `maxContentLength` | 指定最大内容长度。                                                                                                                                                                                           |

## <a name="build-and-run-the-project"></a>生成并运行项目

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 如果应用成功启动，则可在 Azure 门户中检查图形：

   >[!div class="mx-imgBorder"]
   >![execute-result][execute-result-01]


## <a name="next-steps"></a>后续步骤

若要了解有关 Azure 上的 Spring 的详细信息，请继续访问“Azure 上的 Spring”文档。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/developer/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关 Gremlin 和图形 API 的 Azure 支持的详细信息，请参阅以下文章：

* [Azure Cosmos DB 简介：图形 API](/azure/cosmos-db/graph-introduction)

* [Azure Cosmos DB Gremlin 图形支持](/azure/cosmos-db/gremlin-support)

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建图形数据库](/azure/cosmos-db/create-graph-java)

* [教程：使用 Gremlin 查询 Azure Cosmos DB 图形 API](/azure/cosmos-db/tutorial-query-graph)

* [Spring Data Gremlin Starter]

有关使用 Azure Cosmos DB 和 Java 的详细信息，请参阅以下文章：

* [Azure Cosmos DB 文档]。

* [Azure Cosmos DB：使用 Java 和 Azure 门户创建文档数据库][Build a SQL API app with Java]

* [Spring Data for Azure Cosmos DB SQL API]

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[Azure Cosmos DB 文档]: /azure/cosmos-db/
[面向 Java 开发人员的 Azure]: /azure/developer/java/
[Build a SQL API app with Java]: /azure/cosmos-db/create-sql-api-java 
[Spring Data for Azure Cosmos DB SQL API]: https://azure.microsoft.com/blog/spring-data-azure-cosmos-db-nosql-data-access-on-azure/
[Spring Data Gremlin Starter]: https://github.com/Microsoft/spring-data-gremlin
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[图形分区]: https://docs.microsoft.com/azure/cosmos-db/graph-partitioning
[azure-spring-data-sample-gremlin]: https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-data-sample-gremlin

<!-- IMG List -->

[create-a-resource-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-01.png
[create-a-resource-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-02.png
[create-a-resource-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-resource-03.png

[create-a-graph-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-01.png
[create-a-graph-02]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-02.png
[create-a-graph-03]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/create-a-graph-03.png

[spring-initializr-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/spring-initializr-01.png

[get-password-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/get-password-01.png

[execute-result-01]: media/configure-spring-data-gremlin-java-app-with-cosmos-db/execute-result-01.png
