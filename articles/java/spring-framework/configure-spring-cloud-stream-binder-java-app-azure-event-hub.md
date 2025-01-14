---
title: 如何使用 Azure 事件中心创建Spring Cloud Stream Binder 应用程序
description: 了解如何配置基于 Java 的 Spring Cloud Stream Binder 应用程序，该应用程序是在 Azure 事件中心使用 Spring Boot Initializr 创建的。
services: event-hubs
documentationcenter: java
ms.date: 10/13/2020
ms.service: event-hubs
ms.tgt_pltfrm: na
ms.topic: article
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 0cc3289243c1a146cf59ecb15c5150327f49c236
ms.sourcegitcommit: 8e1d3a384ccb0e083589418d65a70b3a01afebff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2020
ms.locfileid: "94560300"
---
# <a name="how-to-create-a-spring-cloud-stream-binder-application-with-azure-event-hubs"></a>如何使用 Azure 事件中心创建Spring Cloud Stream Binder 应用程序

本文介绍如何配置基于 Java 的 Spring Cloud Stream Binder 应用程序，该应用程序是在 Azure 事件中心使用 Spring Boot Initializer 创建的。

## <a name="prerequisites"></a>必备条件

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

> [!IMPORTANT]
> 完成本文中的步骤需要 Spring Boot 2.2 或更高版本。

## <a name="create-an-azure-event-hub-using-the-azure-portal"></a>使用 Azure 门户创建 Azure 事件中心

以下过程创建 Azure 事件中心。

### <a name="create-an-azure-event-hub-namespace"></a>创建 Azure 事件中心命名空间

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户并登录。

1. 选择“+创建资源”，然后搜索“事件中心”。

1. 选择“创建”。

   >[!div class="mx-imgBorder"]
   >![创建 Azure 事件中心命名空间][IMG01]

1. 在“创建命名空间”页上，输入以下信息  ：

   * 选择需要用于命名空间的“订阅”。
   * 指定是为命名空间创建新的“资源组”，还是选择现有资源组。
   * 输入一个唯一的 **命名空间名称**，该名称将成为事件中心命名空间 URI 的一部分。 例如，如果输入 *wingtiptoys-space* 作为 **命名空间名称**，则 URI 将为 `wingtiptoys-space.servicebus.windows.net`。
   * 指定事件中心命名空间的“位置”。
   * 定价层。
   * 也可指定命名空间的“吞吐量单位”。 
   
   >[!div class="mx-imgBorder"]
   >![指定 Azure 事件中心命名空间选项][IMG02]

1. 指定上面列出的选项后，选择“查看 + 创建”，查看规范并选择“创建”以创建命名空间。

## <a name="create-an-azure-event-hub-in-your-namespace"></a>在命名空间中创建 Azure 事件中心

部署命名空间后，请选择“转到资源”以打开“事件中心命名空间”页，在其中可以创建命名空间中的事件中心。

1. 导航到上一步创建的命名空间。

1. 选择顶部菜单栏中的“+ 事件中心”。

1. 为事件中心命名。

1. 选择“创建”。

   >[!div class="mx-imgBorder"]
   >![创建事件中心][IMG05]

### <a name="create-an-azure-storage-account-for-your-event-hub-checkpoints"></a>为事件中心检查点创建 Azure 存储帐户

以下过程创建用于事件中心检查点的存储帐户。

1. 浏览到 <https://portal.azure.com/> 上的 Azure 门户。

1. 依次选择“创建资源”、“存储”和“存储帐户”。

1. 在“创建存储帐户”  页中，输入以下信息：

   * 选择需要用于存储帐户的“订阅”  。
   * 指定是为存储帐户创建新的“资源组”，还是选择现有资源组  。
   * 输入存储帐户的唯一“名称”。 
   * 指定存储帐户的“位置”。 

   >[!div class="mx-imgBorder"]
   >![指定 Azure 存储帐户选项][IMG08]

1. 指定上面列出的选项后，请选择“查看 + 创建”以创建存储帐户。

1. 查看具体细节，然后选择“创建”。  部署将需要数分钟。

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>使用 Spring Initializr 创建简单的 Spring Boot 应用程序

以下过程创建 Spring Boot 应用程序。

1. 浏览到 <https://start.spring.io/>。

1. 指定以下选项：

   * 使用 **Java** 生成一个 **Maven** 项目。
   * 指定一个其值大于或等于 2.2 的 **Spring Boot** 版本。
   * 指定应用程序的“组”和“项目”名称。  
   * 对于 Java 版本，选择“8”。
   * 添加 *Web* 依赖项。

   >[!div class="mx-imgBorder"]
   >![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   > Spring Initializr 使用“组”名称和“项目”名称创建包名称，例如：com.contoso.eventhubs.sample 。

1. 指定上面列出的选项后，选择“生成”。

1. 出现提示时，将项目下载到本地计算机中的路径。

1. 在本地系统中提供文件后，就可以对简单的 Spring Boot 应用程序进行编辑。

## <a name="configure-your-spring-boot-app-to-use-the-azure-event-hub-starter"></a>配置 Spring Boot 应用，以使用 Azure Event Hub Starter

1. 在应用的根目录中找到 pom.xml 文件，例如  ：

   *C:\SpringBoot\eventhubs-sample\pom.xml*

   - 或 -

   */users/example/home/eventhubs-sample/pom.xml*

1. 在文本编辑器中打开 *pom.xml* 文件，将 Spring Cloud Azure Event Hub Stream Binder Starter 添加到 `<dependencies>` 列表：

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>spring-cloud-azure-eventhubs-stream-binder</artifactId>
      <version>1.2.7</version>
   </dependency>
   ```

1. 如果使用的是 JDK 版本 9 或更高版本，请添加以下依赖项：

   ```xml
   <dependency>
       <groupId>javax.xml.bind</groupId>
       <artifactId>jaxb-api</artifactId>
       <version>2.3.1</version>
   </dependency>
   <dependency>
       <groupId>org.glassfish.jaxb</groupId>
       <artifactId>jaxb-runtime</artifactId>
       <version>2.3.1</version>
       <scope>runtime</scope>
   </dependency>
   ```

1. 保存并关闭 pom.xml 文件。

## <a name="create-an-azure-credential-file"></a>创建 Azure 凭据文件

1. 打开命令提示符。

1. 导航到 Spring Boot 应用的 *resources* 目录，例如：

   ```cmd
   cd C:\SpringBoot\eventhubs-sample\src\main\resources
   ```

   -或-

   ```bash
   cd /users/example/home/eventhubs-sample/src/main/resources
   ```

1. 请登录到 Azure 帐户：

   ```azurecli
   az login
   ```

1. 列出订阅：

   ```azurecli
   az account list
   ```
   Azure 将返回订阅列表；需要复制想要使用的订阅的 GUID，例如：

   ```json
   [
     {
       "cloudName": "AzureCloud",
       "id": "11111111-1111-1111-1111-111111111111",
       "isDefault": true,
       "name": "Converted Windows Azure MSDN - Visual Studio Ultimate",
       "state": "Enabled",
       "tenantId": "22222222-2222-2222-2222-222222222222",
       "user": {
         "name": "user@contoso.com",
         "type": "user"
       }
     }
   ]
   ```
   
1. 指定要用于 Azure 的订阅的 GUID；例如：

   ```azurecli
   az account set -s 11111111-1111-1111-1111-111111111111
   ```

1. 创建 Azure 凭据文件：

   ```azurecli
   az ad sp create-for-rbac --sdk-auth > my.azureauth
   ```

   此命令将在 *resources* 目录中创建一个 *my.azureauth* 文件，其内容类似于以下示例：

   ```json
   {
     "clientId": "33333333-3333-3333-3333-333333333333",
     "clientSecret": "44444444-4444-4444-4444-444444444444",
     "subscriptionId": "11111111-1111-1111-1111-111111111111",
     "tenantId": "22222222-2222-2222-2222-222222222222",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

## <a name="configure-your-spring-boot-app-to-use-your-azure-event-hub"></a>配置 Spring Boot 应用以使用 Azure 事件中心

1. 在应用的 *resources* 目录中找到 *application.properties*，例如：

   *C:\SpringBoot\eventhubs-sample\src\main\resources\application.properties*

   - 或 -

   */users/example/home/eventhubs-sample/src/main/resources/application.properties*

2. 在文本编辑器中打开 application.properties 文件，添加以下行，然后将示例值替换为事件中心的相应属性：

   ```yaml
   spring.cloud.azure.credential-file-path=my.azureauth
   spring.cloud.azure.resource-group=wingtiptoysresources
   spring.cloud.azure.region=West US
   spring.cloud.azure.eventhub.namespace=wingtiptoysnamespace
   spring.cloud.azure.eventhub.checkpoint-storage-account=wingtiptoysstorage
   spring.cloud.stream.bindings.input.destination=wingtiptoyshub
   spring.cloud.stream.bindings.input.group=$Default
   spring.cloud.stream.eventhub.bindings.input.consumer.checkpoint-mode=MANUAL
   spring.cloud.stream.bindings.output.destination=wingtiptoyshub
   ```
   其中：

   |                          字段                           |                                                                                   说明                                                                                    |
   |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   |        `spring.cloud.azure.credential-file-path`         |                                                    指定之前在本教程中创建的 Azure 凭据文件。                                                    |
   |           `spring.cloud.azure.resource-group`            |                                                      指定包含 Azure 事件中心的 Azure 资源组。                                                      |
   |               `spring.cloud.azure.region`                |                                           指定你在创建 Azure 事件中心时指定的地理区域。                                            |
   |         `spring.cloud.azure.eventhub.namespace`          |                                          指定你在创建 Azure 事件中心命名空间时指定的唯一名称。                                           |
   | `spring.cloud.azure.eventhub.checkpoint-storage-account` |                                                    指定之前在本教程中创建的 Azure 存储帐户。                                                    |
   |     `spring.cloud.stream.bindings.input.destination`     |                            指定输入目标 Azure 事件中心。在本教程中，它是你此前在本教程中创建的中心。                            |
   |       `spring.cloud.stream.bindings.input.group `        | 在 Azure 事件中心指定一个使用者组，该组可以设置为“$Default”，以便使用你在创建 Azure 事件中心时创建的基本使用者组。 |
   |    `spring.cloud.stream.bindings.output.destination`     |                               指定输出目标 Azure 事件中心。在本教程中，它与输入目标相同。                               |

3. 保存并关闭 application.properties 文件。

## <a name="add-sample-code-to-implement-basic-event-hub-functionality"></a>添加示例代码以实现事件中心的基本功能

在本部分，请创建所需的 Java 类，以便将事件发送到事件中心。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

   *C:\SpringBoot\eventhubs-sample\src\main\java\com\wingtiptoys\eventhub\EventhubApplication.java*

   - 或 -

   */users/example/home/eventhubs-sample/src/main/java/com/wingtiptoys/eventhub/EventhubApplication.java*

1. 在文本编辑器中打开主应用程序 Java 文件，然后将以下行添加到文件中：

   ```java
    package com.contoso.eventhubs.sample;
    
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class EventhubsSampleApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(EventhubsSampleApplication.class, args);
        }
    
    }
   ```

1. 保存并关闭主应用程序 Java 文件。

### <a name="create-a-new-class-for-the-source-connector"></a>为源连接器创建新类

1. 在应用的包目录中创建名为 *EventhubSource.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

    ```java
    package com.contoso.eventhubs.sample;
    
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestBody;
    import org.springframework.web.bind.annotation.RestController;
    
    @EnableBinding(Source.class)
    @RestController
    public class EventhubSource {
    
        @Autowired
        private Source source;
    
        @PostMapping("/messages")
        public String postMessage(@RequestBody String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```
1. 保存 *EventhubSource.java* 文件后将其关闭。

### <a name="create-a-new-class-for-the-sink-connector"></a>为接收器连接器创建新类

1. 在应用的包目录中创建名为 *EventhubSink.java* 的新 Java 文件，然后在文本编辑器中打开该文件并添加以下行：

   ```java
   package com.contoso.eventhubs.sample;

   import com.microsoft.azure.spring.integration.core.AzureHeaders;
   import com.microsoft.azure.spring.integration.core.api.reactor.Checkpointer;
   import org.slf4j.Logger;
   import org.slf4j.LoggerFactory;
   import org.springframework.cloud.stream.annotation.EnableBinding;
   import org.springframework.cloud.stream.annotation.StreamListener;
   import org.springframework.cloud.stream.messaging.Sink;
   import org.springframework.messaging.handler.annotation.Header;

   @EnableBinding(Sink.class)
   public class EventhubSink {

      private static final Logger LOGGER = LoggerFactory.getLogger(EventhubSink.class);

      @StreamListener(Sink.INPUT)
      public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
        LOGGER.info("New message received: '{}'", message);
        checkpointer.success()
                .doOnSuccess(s -> LOGGER.info("Message '{}' successfully checkpointed", message))
                .doOnError((msg) -> {
                    LOGGER.error(String.valueOf(msg));
                })
                .subscribe();
      }
   }
   ```

1. 保存 *EventhubSink.java* 文件后将其关闭。

## <a name="build-and-test-your-application"></a>生成和测试应用程序

使用以下过程生成并测试应用程序。

1. 打开命令提示符并将目录更改为 pom.xml 文件所在的文件夹位置，例如：

   ```cmd
    cd C:\SpringBoot\eventhubs-sample
   ```
   -或-

   ```bash
   cd /users/example/home/eventhubs-sample
   ```

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```bash
   mvn clean package -Dmaven.test.skip=true
   mvn spring-boot:run
   ```

1. 应用程序运行以后，即可使用 `curl` 对其进行测试，例如：

   ```bash
   curl -X POST -H "Content-Type: text/plain" -d "hello" http://localhost:8080/messages
   ```
   此时会看到“hello”发布到应用程序的日志中。 例如：

   ```output
   2020-09-11 15:11:12.138  INFO 7616 --- [      elastic-4] c.contoso.eventhubs.sample.EventhubSink  : New message received: 'hello'
   2020-09-11 15:11:12.406  INFO 7616 --- [ctor-http-nio-1] c.contoso.eventhubs.sample.EventhubSink  : Message 'hello' successfully checkpointed
   ```

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](./index.yml)

### <a name="additional-resources"></a>其他资源

有关 Event Hub Stream Binder 的 Azure 支持的详细信息，请参阅以下文章：

* [什么是 Azure 事件中心？](/azure/event-hubs/event-hubs-about)

* [使用 Azure 门户创建事件中心命名空间和事件中心](/azure/event-hubs/event-hubs-create)

* [如何将适用于 Apache Kafka 的 Spring Boot Starter 与 Azure 事件中心配合使用](configure-spring-cloud-stream-binder-java-app-kafka-azure-event-hub.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序  。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[面向 Java 开发人员的 Azure]: ../index.yml
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/

<!-- IMG List -->

[IMG01]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-01.png
[IMG02]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-02.png
[IMG05]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-05.png
[IMG08]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-event-hub-08.png
[SI01]: media/configure-spring-cloud-stream-binder-java-app-azure-event-hub/create-project-01.png
