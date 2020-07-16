---
title: 如何使用适用于 Azure 服务总线 JMS 的 Spring Boot Starter
description: 本文介绍如何使用 Spring JMS Starter 从 Azure 服务总线收发消息。
author: seanli1988
manager: kyliel
ms.author: seal
ms.date: 08/21/2019
ms.topic: article
ms.custom: devx-track-java
ms.openlocfilehash: efccf07733cb4ae509753f5e384453a46e2bf678
ms.sourcegitcommit: 44016b81a15b1625c464e6a7b2bfb55938df20b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86379211"
---
# <a name="how-to-use-the-spring-boot-starter-for-azure-service-bus-jms"></a>如何使用适用于 Azure 服务总线 JMS 的 Spring Boot Starter

[!INCLUDE [spring-boot-20-note.md](includes/spring-boot-20-note.md)]

Azure 提供了一个异步消息平台，称为 [Azure 服务总线](/azure/service-bus-messaging/service-bus-messaging-overview)（“服务总线”），该平台基于[高级消息队列协议 1.0](http://www.amqp.org/)（“AMQP 1.0”）标准。 服务总线可用于各种受支持的 Azure 平台。

适用于 Azure 服务总线 JMS 的 Spring Boot Starter 可用于将 Spring 与服务总线相集成。

本文介绍如何使用适用于 Azure 服务总线 JMS 的 Spring Boot Starter 从服务总线 `queues` 和 `topics` 收发消息。

## <a name="prerequisites"></a>必备条件

在本文中，需要满足以下先决条件：

1. 如果还没有 Azure 订阅，可以激活 [MSDN 订户权益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)或注册获取[免费帐户](https://azure.microsoft.com/free/)。

1. 支持的 Java 开发工具包 (JDK) 8 或更高版本。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。

1. Apache 的 [Maven](http://maven.apache.org/) 3.2 或更高版本。

1. 如果已有一个已配置的服务总线队列或服务总线主题，请确保服务总线命名空间满足以下要求：

    1. 允许来自所有网络的访问
    1. 是高级版（或更高版本）
    1. 具有一个有关队列和主题的读/写访问权限的访问策略

1. 如果没有已配置的服务总线队列或服务总线主题，请使用 Azure 门户[创建服务总线队列](/azure/service-bus-messaging/service-bus-quickstart-portal)或[创建服务总线主题](/azure/service-bus-messaging/service-bus-quickstart-topics-subscriptions-portal)。 确保该命名空间满足上一步中指定的要求。 另外，请记下该命名空间中的连接字符串，因为本教程的测试应用需要用到它。

1. 如果没有 Spring Boot 应用程序，请[使用 Spring Initializer 创建一个 Maven  项目](https://start.spring.io/)。 请记得选择“Maven 项目”  ，然后在“依赖项”  下方，添加“Web”  依赖项。

## <a name="use-the-azure-service-bus-jms-starter"></a>使用 Azure 服务总线 JMS Starter

1. 在应用的父目录中找到 pom.xml  文件，例如：

    `C:\SpringBoot\servicebus\pom.xml`

    -或-

    `/users/example/home/servicebus/pom.xml`

1. 在文本编辑器中打开 pom.xml 文件  。

1. 将 Spring Boot Azure 服务总线 JMS Starter 添加到 `<dependencies>` 的列表：

    ```xml
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus-jms-spring-boot-starter</artifactId>
        <version>2.1.7</version>
    </dependency>
    ```

    ![将依赖项部分添加到 pom.xml 文件。](media/configure-spring-boot-starter-java-app-with-azure-service-bus/add-dependency-section-new.png)

1. 保存并关闭 pom.xml 文件  。

## <a name="configure-the-app-for-your-service-bus"></a>针对服务总线配置应用 

在本部分中，了解如何将应用配置为使用服务总线队列或服务总线主题。

### <a name="use-a-service-bus-queue"></a>使用服务总线队列

1. 在应用的 *resources* 目录中找到 *application.properties*，例如：

    `C:\SpringBoot\servicebus\application.properties`

    -或-

    `/users/example/home/servicebus/application.properties`

1. 在文本编辑器中打开 application.properties 文件  。

1. 将以下代码追加到 application.properties  文件的末尾。 将示例值替换为适用于你的服务总线的值：

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **字段说明**

    | 字段                                     | 说明                                                                                     |
    |-------------------------------------------|-------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | 指定从 Azure 门户内你的服务总线命名空间中获取的连接字符串。 |
    | `spring.jms.servicebus.idle-timeout`      | 指定空闲超时时间（以毫秒为单位）。 在本教程中，建议的值为 1800000。   |

1. 保存并关闭 application.properties 文件  。

### <a name="use-service-bus-topic"></a>使用服务总线主题

1. 在应用的 *resources* 目录中找到 *application.properties*，例如：

    `C:\SpringBoot\servicebus\application.properties`

    -或-

    `/users/example/home/servicebus/application.properties`

1. 在文本编辑器中打开 application.properties 文件  。

1. 将以下代码追加到 application.properties  文件的末尾。 将示例值替换为适用于你的服务总线的值：

    ```yml
    spring.jms.servicebus.connection-string=<ServiceBusNamespaceConnectionString>
    spring.jms.servicebus.topic-client-id=<ServiceBusTopicClientId>
    spring.jms.servicebus.idle-timeout=<IdleTimeout>
    ```

    **字段说明**

    | 字段                                     | 说明                                                                                       |
    |-------------------------------------------|---------------------------------------------------------------------------------------------------|
    | `spring.jms.servicebus.connection-string` | 指定从 Azure 门户内你的服务总线命名空间中获取的连接字符串。   |
    | `spring.jms.servicebus.topic-client-id`   | 指定 JMS 客户端 ID（如果使用的是包含持久订阅的 Azure 服务总线主题）。 |
    | `spring.jms.servicebus.idle-timeout`      | 指定空闲超时时间（以毫秒为单位）。 在本教程中，建议的值为 1800000。     |

1. 保存并关闭 application.properties 文件  。

## <a name="implement-basic-service-bus-functionality"></a>实现基本的服务总线功能

在本部分中，创建所需的 Java 类，用于将消息发送到服务总线队列或服务总线主题，并从相应的队列或主题订阅接收消息。

### <a name="modify-the-main-application-class"></a>修改主应用程序类

1. 在应用的程序包目录中找到主应用程序 Java 文件，例如：

    `C:\SpringBoot\servicebus\src\main\java\com\wingtiptoys\servicebus\ServiceBusJmsStarterApplication.java`

    -或-

    `/users/example/home/servicebus/src/main/java/com/wingtiptoys/servicebus/ServiceBusJmsStarterApplication.java`

1. 在文本编辑器中打开主应用程序 Java 文件。

1. 将以下代码添加到该文件：

   ```java
    package com.wingtiptoys.servicebus;

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;

    @SpringBootApplication
    public class ServiceBusJmsStarterApplication {

        public static void main(String[] args) {
            SpringApplication.run(ServiceBusJmsStarterApplication.class, args);
        }
    }
    ```

1. 保存并关闭该文件。

### <a name="define-a-test-java-class"></a>定义测试 Java 类

1. 使用文本编辑器，在应用的包目录中创建名为 User.java  的 Java 文件。

1. 定义一个泛型用户类，以存储和检索用户名：

    ```java
    package com.wingtiptoys.servicebus;

    import java.io.Serializable;

    // Define a generic User class.
    public class User implements Serializable {

        private static final long serialVersionUID = -295422703255886286L;

        private String name;

        public User() {
        }

        public User(String name) {
            setName(name);
        }

        public String getName() {
            return name;
        }

        public void setName(String name) {
            this.name = name;
        }

    }
    ```

    实现 `Serializable` 是为了使用 Spring 框架的 `JmsTemplate` 中的 `send` 方法。 否则，应定义自定义的 `MessageConverter` bean，以将内容序列化为文本格式的 json。 有关 `MessageConverter` 的详细信息，请参阅官方的 [Spring JMS Starter 项目](https://spring.io/guides/gs/messaging-jms/)。

1. 保存并关闭 User.java 文件  。

### <a name="create-a-new-class-for-the-message-send-controller"></a>为消息发送控制器创建新类

1. 使用文本编辑器，在应用的包目录中创建名为 SendController.java  的 Java 文件

1. 将以下代码添加到新文件：

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.jms.core.JmsTemplate;
    import org.springframework.web.bind.annotation.PostMapping;
    import org.springframework.web.bind.annotation.RequestParam;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    public class SendController {

        private static final String DESTINATION_NAME = "<DestinationName>";

        private static final Logger logger = LoggerFactory.getLogger(SendController.class);

        @Autowired
        private JmsTemplate jmsTemplate;

        @PostMapping("/messages")
        public String postMessage(@RequestParam String message) {
            logger.info("Sending message");
            jmsTemplate.convertAndSend(DESTINATION_NAME, new User(message));
            return message;
        }
    }
    ```

    > [!NOTE]
    > 将 `<DestinationName>` 替换为在服务总线命名空间中配置的你自己的队列名称或主题名称。

1. 保存并关闭 SendController.java  。

### <a name="create-a-class-for-the-message-receive-controller"></a>为消息接收控制器创建类

#### <a name="receive-messages-from-a-service-bus-queue"></a>从服务总线队列接收消息

1. 使用文本编辑器，在应用的包目录中创建名为 QueueReceiveController.java  的 Java 文件

1. 将以下代码添加到新文件：

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class QueueReceiveController {

        private static final String QUEUE_NAME = "<ServiceBusQueueName>";

        private final Logger logger = LoggerFactory.getLogger(QueueReceiveController.class);

        @JmsListener(destination = QUEUE_NAME, containerFactory = "jmsListenerContainerFactory")
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

    > [!NOTE]
    > 将 `<ServiceBusQueueName>` 替换为在服务总线命名空间中配置的你自己的队列名称。

1. 保存并关闭 QueueReceiveController.java  文件。

#### <a name="receive-messages-from-a-service-bus-subscription"></a>从服务总线订阅接收消息

1. 使用文本编辑器，在应用的包目录中创建名为 TopicReceiveController.java  的 Java 文件。 

1. 将以下代码添加到新文件。 将 `<ServiceBusTopicName>` 占位符替换为在服务总线命名空间中配置的你自己的主题名称。 将 `<ServiceBusSubscriptionName>` 占位符替换为服务总线主题中的你自己的订阅名称。

    ```java
    package com.wingtiptoys.servicebus;

    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.jms.annotation.JmsListener;
    import org.springframework.stereotype.Component;

    @Component
    public class TopicReceiveController {

        private static final String TOPIC_NAME = "<ServiceBusTopicName>";

        private static final String SUBSCRIPTION_NAME = "<ServiceBusSubscriptionName>";

        private final Logger logger = LoggerFactory.getLogger(TopicReceiveController.class);

        @JmsListener(destination = TOPIC_NAME, containerFactory = "topicJmsListenerContainerFactory",
                subscription = SUBSCRIPTION_NAME)
        public void receiveMessage(User user) {
            logger.info("Received message: {}", user.getName());
        }
    }
    ```

1. 保存并关闭 TopicReceiveController.java  文件。

## <a name="build-and-test-your-application"></a>生成和测试应用程序

1. 打开命令提示符，然后将目录更改为 pom.xml  的位置；例如：

    `cd C:\SpringBoot\servicebus`

    -或-

    `cd cd /users/example/home/servicebus`

1. 使用 Maven 构建 Spring Boot 应用程序，然后运行该应用程序：

    ```shell
    mvn clean spring-boot:run
    ```

1. 在应用程序运行后，你可以使用 curl  对其进行测试：

    ```shell
    curl -X POST localhost:8080/messages?message=hello
    ```

    此时会显示“发送消息”和“hello”已发布到应用程序日志：

    ```shell
    [nio-8080-exec-1] com.wingtiptoys.servicebus.SendController : Sending message
    [enerContainer-1] com.wingtiptoys.servicebus.ReceiveController : Received message: hello
    ```

## <a name="clean-up-resources"></a>清理资源

如果不再需要，请使用 [Azure 门户](https://portal.azure.com/)删除本文中创建的资源，以避免产生意外的费用。

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [如何将 JMS API 与服务总线和 AMQP 1.0 结合使用](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)