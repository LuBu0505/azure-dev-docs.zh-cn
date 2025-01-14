---
title: 如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB
description: 了解如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB。
services: cosmos-db
documentationcenter: java
ms.date: 10/13/2020
ms.service: cosmos-db
ms.tgt_pltfrm: multiple
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 7879a47bdcbc9b1a4cf41210fc9fb49ad28d8dd8
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560316"
---
# <a name="how-to-use-spring-data-apache-cassandra-api-with-azure-cosmos-db"></a>如何将 Spring Data Apache Cassandra API 用于 Azure Cosmos DB

本文演示了如何创建一个示例应用程序，该应用程序使用 [Spring Data] 通过 [Azure Cosmos DB Cassandra API](/azure/cosmos-db/cassandra-introduction) 存储和检索信息。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* 用来测试功能的 [Curl](https://curl.haxx.se/) 或类似的 HTTP 实用工具。
* [Git](https://git-scm.com/downloads) 客户端。

## <a name="create-an-azure-cosmos-db-account"></a>创建 Azure Cosmos DB 帐户

以下过程在 Azure 门户中创建并配置 Cosmos 帐户。

### <a name="create-a-cosmos-db-account-using-the-azure-portal"></a>使用 Azure 门户创建 Cosmos DB 帐户

> [!NOTE]
> 
> 可以在 [Azure Cosmos DB 文档](/azure/cosmos-db/)中阅读有关创建 Azure Cosmos DB 帐户的更多详细信息。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 依次选择“创建资源”、“开始使用”和“Azure Cosmos DB”  。
    
   >[!div class="mx-imgBorder"]
   >![创建 Azure Cosmos DB 帐户][COSMOSDB01]

1. 指定以下信息：

   - **订阅**：指定要使用的 Azure 订阅。
   - **资源组**：指定是要创建新资源组，还是选择现有资源组。
   - **帐户名**：为 Cosmos DB 帐户选择一个唯一名称；这将用来创建完全限定的域名，例如 *wingtiptoyscassandra.documents.azure.com*。
   - **API**：为本教程指定 Cassandra。
   - **位置**：指定最靠近你的数据库的地理区域。
   
   >[!div class="mx-imgBorder"]
   >![指定 Cosmos DB 帐户设置][COSMOSDB02]
   
1. 输入上述所有信息后，单击“查看 + 创建”  。

1. 如果查看页面上的所有信息看起来都是正确的，则单击“创建”。 
   
   >[!div class="mx-imgBorder"]
   >![查看 Cosmos DB 帐户设置][COSMOSDB03]

部署数据库需要几分钟的时间。

### <a name="add-a-keyspace-to-your-azure-cosmos-db-account"></a>向 Azure Cosmos DB 帐户添加密钥空间

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 选择“所有资源”，然后选择刚才创建的 Azure Cosmos DB 帐户。

1. 选择“数据资源管理器”，选择向下箭头，然后选择“新建密钥空间” 。 为“密钥空间 ID”输入一个唯一标识符，然后选择“确定”。 
    
   >[!div class="mx-imgBorder"]
   >选择“新建密钥”![][COSMOSDB05]
   
   >[!div class="mx-imgBorder"]
   >![创建 Cosmos DB 密钥空间][COSMOSDB05-1]

### <a name="retrieve-the-connection-settings-for-your-azure-cosmos-db-account"></a>检索 Azure Cosmos DB 帐户的连接设置

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 选择“所有资源”，然后选择刚才创建的 Azure Cosmos DB 帐户。

1. 选择“连接字符串”，复制“接触点”、“端口”、“用户名”和“主要密码”字段的值；稍后需要使用这些值来配置应用程序。    
   
   >[!div class="mx-imgBorder"]
   >![检索 Cosmos DB 连接设置][COSMOSDB06]

## <a name="configure-the-sample-application"></a>配置示例应用程序

以下过程配置测试性应用程序。

1. 打开一个命令 shell 并使用 git 命令克隆示例项目，如以下示例所示：

   ```shell
   git clone https://github.com/Azure-Samples/spring-data-cassandra-on-azure.git
   ```

1. 在示例项目的 *resources* 目录中找到 *application.properties* 文件，或者创建此文件（若此文件尚不存在）。

1. 在文本编辑器中打开 *application.properties* 文件，在文件中添加或配置以下行，并将示例值替换为上文中的相应值：

   ```yaml
   spring.data.cassandra.contact-points=wingtiptoyscassandra.cassandra.cosmos.azure.com
   spring.data.cassandra.port=10350
   spring.data.cassandra.username=wingtiptoyscassandra
   spring.data.cassandra.password=********
   ```
   其中：

   | 参数 | 描述 |
   |---|---|
   | `spring.data.cassandra.contact-points` | 指定本文上文中所述的 **接触点**。 |
   | `spring.data.cassandra.port` | 指定本文上文中所述的 **端口**。 |
   | `spring.data.cassandra.username` | 指定本文上文中所述的 **用户名**。 |
   | `spring.data.cassandra.password` | 指定本文上文中所述的 **主要密码**。 |

1. 保存并关闭 application.properties 文件。

## <a name="package-and-test-the-sample-application"></a>打包并测试示例应用程序 

浏览到 .pom 文件所在目录，以便生成并测试应用程序。

1. 使用 Maven 生成示例应用程序，例如：

   ```shell
   mvn clean package
   ```

1. 启动示例应用程序；例如：

   ```shell
   java -jar target/spring-data-cassandra-on-azure-0.1.0-SNAPSHOT.jar
   ```

1. 在命令提示符下使用 `curl` 创建新记录，如以下示例所示：

   ```shell
   curl -s -d "{\"name\":\"dog\",\"species\":\"canine\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets

   curl -s -d "{\"name\":\"cat\",\"species\":\"feline\"}" -H "Content-Type: application/json" -X POST http://localhost:8080/pets
   ```

   你的应用程序应返回如下所示的值：

   ```shell
   Added Pet{id=60fa8cb0-0423-11e9-9a70-39311962166b, name='dog', species='canine'}.

   Added Pet{id=72c1c9e0-0423-11e9-9a70-39311962166b, name='cat', species='feline'}.
   ```

1. 在命令提示符下使用 `curl` 检索所有现有记录，如以下示例所示：

   ```shell
   curl -s http://localhost:8080/pets
   ```

   你的应用程序应返回如下所示的值：

   ```json
   [{"id":"60fa8cb0-0423-11e9-9a70-39311962166b","name":"dog","species":"canine"},{"id":"72c1c9e0-0423-11e9-9a70-39311962166b","name":"cat","species":"feline"}]
   ```

## <a name="summary"></a>摘要

在本教程中，你创建了一个示例 Java 应用程序，该应用程序使用 Spring Data 通过 Azure Cosmos DB Cassandra API 存储和检索信息。

## <a name="clean-up-resources"></a>清理资源

如果不再需要，请使用 [Azure 门户](https://portal.azure.com/)删除本文中创建的资源，以避免产生意外的费用。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](./index.yml)

### <a name="additional-resources"></a>其他资源

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: ../index.yml
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Data]: https://spring.io/projects/spring-data
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[COSMOSDB01]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-01.png
[COSMOSDB02]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-02.png
[COSMOSDB03]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-03.png
[COSMOSDB05]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05.png
[COSMOSDB05-1]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-05-1.png
[COSMOSDB06]: media/configure-spring-data-apache-cassandra-with-cosmos-db/create-cosmos-db-06.png