---
title: 如何使用适用于 Azure 服务总线的 Spring Cloud Azure Stream Binder
description: 本文介绍如何使用 Spring Cloud Stream Binder 通过 Azure 服务总线收发消息。
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 10/10/2020
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: 170d3727b661f18252edb39396739e2791101534
ms.sourcegitcommit: 709fa38a137b30184a7397e0bfa348822f3ea0a7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96441805"
---
# <a name="how-to-use-spring-cloud-azure-stream-binder-for-azure-service-bus"></a>如何使用适用于 Azure 服务总线的 Spring Cloud Azure Stream Binder

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

本文介绍如何使用 Spring Cloud Stream Binder 通过服务总线 `queues` 和 `topics` 收发消息。

Azure 提供了一个异步消息平台，称为 [Azure 服务总线](/azure/service-bus-messaging/service-bus-messaging-overview)（“服务总线”），该平台基于[高级消息队列协议 1.0](http://www.amqp.org/)（“AMQP 1.0”）标准。 服务总线可用于各种受支持的 Azure 平台。

## <a name="prerequisites"></a>必备条件

在本文中，需要满足以下先决条件：

1. Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)或注册[免费帐户](https://azure.microsoft.com/free/)。

1. 支持的 Java 开发工具包 (JDK) 8 或更高版本。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。

1. Apache 的 [Maven](http://maven.apache.org/) 3.2 或更高版本。

1. 如果已有一个已配置的服务总线队列或服务总线主题，请确保服务总线命名空间满足以下要求：

    1. 允许来自所有网络的访问
    1. 为 Standard（或更高版本）
    1. 具有一个有关队列和主题的读/写访问权限的访问策略

1. 如果没有已配置的服务总线队列或服务总线主题，请使用 Azure 门户[创建服务总线队列](/azure/service-bus-messaging/service-bus-quickstart-portal)或[创建服务总线主题](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)。 确保该命名空间满足上一步中指定的要求。 另外，请记下该命名空间中的连接字符串，因为本教程的测试应用需要用到它。

1. 如果没有 Spring Boot 应用程序，请使用 [Spring Initializr](https://start.spring.io/) 创建一个 Maven项目。 请记得选择“Maven 项目”，在“依赖项”下添加“Web”依赖项，然后选择 Java 版本 8   。


## <a name="use-the-spring-cloud-stream-binder-starter"></a>使用 Spring Cloud Stream Binder 入门版

1. 在应用的父目录中找到 pom.xml  文件，例如：

    `C:\SpringBoot\servicebus\pom.xml`

    -或-

    `/users/example/home/servicebus/pom.xml`

1. 在文本编辑器中打开 pom.xml 文件  。

1. 将以下代码块添加到 **&lt;dependencies>** 元素的下方，具体取决于使用的是服务总线队列还是服务总线主题：

    **服务总线队列**

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-cloud-stream-binder-servicebus-queue</artifactId>
        <version>2.0.0-beta.1</version> <!-- {x-version-update;com.azure.spring:azure-spring-cloud-stream-binder-servicebus-queue;current} -->
    </dependency>
    ```

    **服务总线主题**

    ```xml
    <dependency>
        <groupId>com.azure.spring</groupId>
        <artifactId>azure-spring-cloud-stream-binder-servicebus-topic</artifactId>
        <version>2.0.0-beta.1</version> <!-- {x-version-update;com.azure.spring:azure-spring-cloud-stream-binder-servicebus-topic;current} -->
    </dependency>
    ```


1. 保存并关闭 pom.xml 文件。

## <a name="configure-the-app-for-your-service-bus"></a>针对服务总线配置应用

可以基于连接字符串或凭据文件配置应用。 本教程使用连接字符串。 有关使用凭据文件的详细信息，请参阅[适用于服务总线队列的 Spring Cloud Azure Stream Binder 代码示例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-samples/azure-spring-cloud-sample-servicebus-queue-binder
)和[适用于服务总线主题的 Cloud Azure Stream Binder 代码示例](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-cloud-stream-binder-servicebus-topic)。

1. 在应用的“资源”目录中找到 application.properties 文件，例如 ：

   `C:\SpringBoot\servicebus\src\main\resources\application.properties`

   -或-

   `/users/example/home/servicebus/src/main/resources/application.properties`

1. 在文本编辑器中打开 application.properties 文件。

1. 将适当的代码追加到 application.properties 文件的末尾，具体取决于使用的是服务总线队列还是服务总线主题。 使用[字段说明表](#fd)将示例值替换为服务总线的相应属性。

    **服务总线队列**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=examplequeue
    spring.cloud.stream.bindings.output.destination=examplequeue
    spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **服务总线主题**

    ```yaml
    spring.cloud.azure.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.cloud.stream.bindings.input.destination=exampletopic
    spring.cloud.stream.bindings.input.group=examplesubscription
    spring.cloud.stream.bindings.output.destination=exampletopic
    spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode=MANUAL
    ```

    **<a name="fd">字段说明</a>**

    |                                        字段                                   |                                                                                   说明                                                                                    |
    |--------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    |               `spring.cloud.azure.servicebus.connection-string`                |                                        指定从 Azure 门户内你的服务总线命名空间中获取的连接字符串。                                   |
    |               `spring.cloud.stream.bindings.input.destination`                 |                            指定在本教程中使用的服务总线队列或服务总线主题。                         |
    |                  `spring.cloud.stream.bindings.input.group`                    |                                            如果使用了服务总线主题，请指定主题订阅。                                |
    |               `spring.cloud.stream.bindings.output.destination`                |                               指定用于输入目标的相同值。                        |
    | `spring.cloud.stream.servicebus.queue.bindings.input.consumer.checkpoint-mode` |                                                       指定 `MANUAL`。                                                   |
    | `spring.cloud.stream.servicebus.topic.bindings.input.consumer.checkpoint-mode` |                                                       指定 `MANUAL`。                                                   |

1. 保存并关闭 application.properties 文件。

## <a name="implement-basic-service-bus-functionality"></a>实现基本的服务总线功能

在本部分中，创建所需的 Java 类，以将消息发送到服务总线。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

    `C:\SpringBoot\servicebus\src\main\java\com\example\ServiceBusBinderApplication.java`

   -或-

   `/users/example/home/servicebus/src/main/java/com/example/ServiceBusBinderApplication.java`

1. 在文本编辑器中打开主应用程序 Java 文件。

1. 将以下代码添加到该文件：

    ```java
    package com.example;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusBinderApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusBinderApplication.class, args);
        }
    }
    ```

1. 保存并关闭该文件。

### <a name="create-a-new-class-for-the-source-connector"></a>为源连接器创建新类

1. 使用文本编辑器，在应用的包目录中创建名为 StreamBinderSource.java 的 Java 文件。

1. 将以下代码添加到新文件：

    ```java
    package com.example;

    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.messaging.Source;
    import org.springframework.messaging.support.GenericMessage;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @EnableBinding(Source.class)
    @RestController
    public class StreamBinderSource {

        @Autowired
        private Source source;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            this.source.output().send(new GenericMessage<>(message));
            return message;
        }
    }
    ```

1. 保存并关闭 StreamBinderSources.java 文件。

### <a name="create-a-new-class-for-the-sink-connector"></a>为接收器连接器创建新类

1. 使用文本编辑器，在应用的包目录中创建名为 StreamBinderSink.java 的 Java 文件。

1. 将以下代码行添加到新文件：

    ```java
    package com.example;

    import com.microsoft.azure.spring.integration.core.AzureHeaders;
    import com.microsoft.azure.spring.integration.core.api.Checkpointer;
    import org.springframework.cloud.stream.annotation.EnableBinding;
    import org.springframework.cloud.stream.annotation.StreamListener;
    import org.springframework.cloud.stream.messaging.Sink;
    import org.springframework.messaging.handler.annotation.Header;

    @EnableBinding(Sink.class)
    public class StreamBinderSink {

        @StreamListener(Sink.INPUT)
        public void handleMessage(String message, @Header(AzureHeaders.CHECKPOINTER) Checkpointer checkpointer) {
            System.out.println(String.format("New message received: '%s'", message));
            checkpointer.success().handle((r, ex) -> {
                if (ex == null) {
                    System.out.println(String.format("Message '%s' successfully checkpointed", message));
                }
                return null;
            });
        }
    }
    ```

1. 保存并关闭 StreamBinderSink.java 文件。

## <a name="build-and-test-your-application"></a>生成和测试应用程序

1. 打开命令提示符。

1. 将目录更改为 pom.xml 文件的位置；例如：

    `cd C:\SpringBoot\servicebus`

    或

    `cd /users/example/home/servicebus`

2. 使用 Maven 构建 Spring Boot 应用程序，然后运行该应用程序：

    ```shell
    mvn clean spring-boot:run
    ```

3. 在应用程序运行后，你可以使用 curl 对其进行测试：

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    此时应显示发布到应用程序日志中的“hello”：

    ```shell
    New message received: 'hello'
    Message 'hello' successfully checkpointed
    ```

## <a name="clean-up-resources"></a>清理资源

如果不再需要，请使用 [Azure 门户](https://portal.azure.com/)删除本文中创建的资源，以避免产生意外的费用。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/java/azure/spring-framework)