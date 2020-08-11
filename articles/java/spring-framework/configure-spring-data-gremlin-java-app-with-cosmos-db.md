---
title: 如何将 Spring Data Gremlin Starter 与 Azure Cosmos DB SQL API 配合使用
description: 了解如何为使用 Spring Boot Initializer 创建的应用程序配置 Azure Cosmos DB SQL API。
services: cosmos-db
documentationcenter: java
ms.date: 01/10/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.workload: data-services
ms.custom: devx-track-java
ms.openlocfilehash: 81a80d14e4cf371801cf75af1618048dda8775d7
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810620"
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


## <a name="create-resource"></a>创建资源

### <a name="create-azure-cosmos-db"></a>创建 Azure Cosmos DB

1. 浏览转到 Azure 门户 (<https://portal.azure.com/>)，然后单击 `+Create a resource`。

   >[!div class="mx-imgBorder"]
   >![create-a-resource][create-a-resource-01]

1. 单击 `Databases`，然后单击 `Azure Cosmos DB`。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db][create-a-resource-02]

1. 在 `Azure Cosmos DB` 页面上，输入以下信息：

   * 选择要用于数据库的 `Subscription`。
   * 指定是为数据库创建新的 `Resource Group` 还是选择现有资源组。
   * 输入唯一 `Account Name` 用作数据库 Gremlin URI 的一部分。 例如：如果为 `Account Name` 输入 `account-sample`，则 Gremlin URI 将为 `account-samplewingtiptoysdata.gremlin.cosmosdb.azure.com`。
   * 为 API 选择 `Gremlin (Graph)` 。
   * 为数据库指定 `Location`。
   
1. 指定这些选项后，单击 `Review + create`。

   >[!div class="mx-imgBorder"]
   >![create-azure-cosmos-db-account][create-a-resource-03]

1. 查看具体细节，然后单击 `Create` 以创建数据库。

### <a name="add-a-graph-to-your-azure-cosmos-database"></a>将图形添加到 Azure Cosmos 数据库

1. 在 Cosmos DB 页面上，单击 `Data Explorer`，然后单击 `New Graph`。

   >[!div class="mx-imgBorder"]
   >![new-graph][create-a-graph-01]

1. 显示 `Add Graph` 后，输入以下信息：

   * 指定数据库的唯一 `Database id`。
   * 可选择指定 `Storage capacity`，也可接受默认值。
   * 指定图形的唯一 `Graph id`。
   * 指定 `Partition key`。 有关详细信息，请参阅[图形分区]。
单击 `OK`。
   
   指定这些选项后，单击 `OK` 以创建图形。

   >[!div class="mx-imgBorder"]
   >![add-graph][create-a-graph-02]

1. 创建图形后，可使用 `Data Explorer` 来查看它。

   >[!div class="mx-imgBorder"]
   >![graph-detail][create-a-graph-03]
   
   

## <a name="create-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

1. 浏览到 <https://start.spring.io/>。

1. 填充项目元数据，然后单击 `GENERATE`：

   >[!div class="mx-imgBorder"]
   >![spring-initializr][spring-initializr-01]

1. 解压缩文件，然后导入到 IDE。


## <a name="update-code-according-to-the-sample-project"></a>根据示例项目更新代码

如示例项目一样修改项目：[azure-spring-data-sample-gremlin]。

1. 添加 `azure-spring-data-gremlin` 的依赖项

1. 删除 `src/test/` 中的所有内容

1. 如本示例所述将所有 Java 文件添加到 `src/main/java` 中。

1. 更新 `src/main/resorces/application.properties` 中的配置，其中：

   | 字段              | 说明                                                                                                                                                                                                             |
   |--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | `endpoint`         | 指定数据库的 Gremlin URI，此 URI 派生自前面在本教程中创建 Azure Cosmos DB 时指定的唯一“ID”。****                                                 |
   | `port`             | 指定 TCP/IP 端口，对于 HTTPS 通信，应是 **443**。                                                                                                                                                           |
   | `username`         | 指定前面在本教程中添加图形时使用的唯一“数据库 ID”和“图形 ID”；必须使用以下语法输入这些值："/dbs/**{数据库ID}**/colls/**{图形 ID}**"。******** |
   | `password`         | 指定前面在本教程中复制的主要或辅助“访问密钥”。****                                                                                                                      |
   | `sslEnabled`       | 指定是否启用 SSL。                                                                                                                                                                                           |
   | `telemetryAllowed` | 若要启用遥测，请指定 **true**；否则指定 **false**。
   | `maxContentLength` | 指定最大内容长度。                                                                                                                                                                                           |

1. 关于如何获取密码：

   >[!div class="mx-imgBorder"]
   >![get-password][get-password-01]

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

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

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
