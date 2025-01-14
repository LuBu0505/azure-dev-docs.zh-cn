---
title: 创建 Spring Boot Initializer 应用 - Azure Redis 缓存
description: 将使用 Spring Initializer 创建的 Spring Boot 应用程序配置为使用云中的 Redis 和 Azure Redis 缓存。
services: redis-cache
documentationcenter: java
ms.date: 10/13/2020
ms.service: cache
ms.tgt_pltfrm: cache-redis
ms.topic: conceptual
ms.custom: devx-track-java
ms.openlocfilehash: 7d8ee875339adb741fbddeba6d4328eb22cd3e46
ms.sourcegitcommit: 5c7f5fef798413b1a304cc9ee31c8518b73f27eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93066267"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-redis-in-the-cloud-with-azure-redis-cache"></a>将 Spring Boot Initializer 应用配置为使用云中的 Redis 和 Azure Redis 缓存

本文提供以下分步指导：使用 Azure 门户在云中创建 Redis 缓存，使用 **[Spring Initializr]** 创建自定义应用程序，然后创建使用 Redis 缓存存储并检索数据的 Java Web 应用程序。

## <a name="prerequisites"></a>必备条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅；如果没有 Azure 订阅，可激活 [MSDN 订阅者权益]或注册[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。

## <a name="create-a-custom-application-using-the-spring-initializr"></a>使用 Spring Initializr 创建自定义应用程序

1. 浏览到 <https://start.spring.io/>。

1. 指定希望使用 Java 生成一个 Maven 项目，选择 Java 版本“8”，并为应用程序输入“组”和“项目”名称    。

1. 添加“Spring Web”部分的依赖项，选中“Web”框，然后向下滚动到“NoSQL”部分，选中“Spring Data Reactive Redis”框     。 
1. 滚动到页面底部，单击“生成项目”对应的按钮。 

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 会使用“组”和“项目”名称来创建包名称，例如：com.contoso.myazuredemo    。
   >

1. 出现提示时，将项目下载到本地计算机中的路径。

   ![下载自定义 Spring Boot 项目][SI03]

1. 提取本地系统上的文件之后，自定义 Spring Boot 应用程序便可进行编辑。

   ![自定义 Spring Boot 项目文件][SI04]

## <a name="create-a-redis-cache-on-azure"></a>在 Azure 上创建 Redis 缓存

1. 浏览到位于 <https://portal.azure.com/> 的 Azure 门户，然后单击“+ 新建”  。

1. 单击“数据库”，然后单击“Redis 缓存”   。

   ![在 Azure 门户中选择 Redis 缓存。][AZ02]

1. 在“新建 Redis 缓存”页上，指定以下信息  ：

   * 输入缓存的“DNS 名称”  。
   * 指定“订阅”、“资源组”、“位置”和“定价层”     。
   * 对于本教程，选择“取消阻止端口 6379”  。

   > [!NOTE]
   >
   > 可以通过 Redis 缓存使用 SSL，但需要使用其他 Redis 客户端，如 Jedis。 有关详细信息，请参阅[如何将 Azure Redis 缓存与 Java 配合使用][Redis Cache with Java]。
   >

   指定这些选项后，单击“创建”以创建缓存  。

   ![在 Azure 门户中创建缓存。][AZ03]

1. 创建缓存完成后，会看到其列在 Azure“仪表板”  上，并显示在“所有资源”  和“Redis 缓存”  页下。 可在任何上述位置上单击缓存，打开缓存的属性页。

   ![Azure 门户中预配的资源。][AZ04]

1. 显示包含缓存属性列表的页面后，单击“访问密匙”，然后复制缓存的访问密钥  。

   ![复制“访问密钥”部分下的访问密钥。][AZ05]

## <a name="configure-your-custom-spring-boot-to-use-your-redis-cache"></a>配置自定义 Spring Boot 以使用 Redis 缓存

1. 在应用的“资源”目录中找到 application.properties 文件，或创建此文件（若此文件不存在）   。

   ![找到 application.properties 文件][RE01]

1. 在文本编辑器中打开 application.properties 文件，将以下行添加到文件中，然后将示例值替换为缓存中的相应属性： 

   ```yaml
   # Specify the DNS URI of your Redis cache.
   spring.redis.host=myspringbootcache.redis.cache.windows.net

   # Specify the port for your Redis cache.
   spring.redis.port=6379

   # Specify the access key for your Redis cache.
   spring.redis.password=57686f6120447564652c2049495320526f636b73=
   ```

   ![编辑 application.properties 文件][RE02]

   > [!NOTE] 
   > 
   > 如果使用其他启用了 SSL 的 Redis 客户端（如 Jedis），则需要在 *application.properties* 文件中指定要使用 SSL，并使用端口 6380。 例如：
   > 
   > ```yaml
   > # Specify the DNS URI of your Redis cache.
   > spring.redis.host=myspringbootcache.redis.cache.windows.net
   > # Specify the access key for your Redis cache.
   > spring.redis.password=57686f6120447564652c2049495320526f636b73=
   > # Specify that you want to use SSL.
   > spring.redis.ssl=true
   > # Specify the SSL port for your Redis cache.
   > spring.redis.port=6380
   > ```
   > 
   > 有关详细信息，请参阅[如何将 Azure Redis 缓存与 Java 配合使用][Redis Cache with Java]。 
   > 

1. 保存并关闭 application.properties 文件  。

1. 在包的源文件夹中创建名为“控制器”的文件夹，例如： 

   `C:\SpringBoot\myazuredemo\src\main\java\com\contoso\myazuredemo\controller`

   -或-

   `/users/example/home/myazuredemo/src/main/java/com/contoso/myazuredemo/controller`

1. 在 controller 文件夹中创建一个名为 HelloController.java 的新文件。 在文本编辑器中打开该文件，然后向其添加以下代码：

   ```java
   package com.contoso.myazuredemo;

   import org.springframework.web.bind.annotation.RequestMapping;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.data.redis.core.StringRedisTemplate;
   import org.springframework.data.redis.core.ValueOperations;

   @RestController
   public class HelloController {
   
      @Autowired
      private StringRedisTemplate template;

      @RequestMapping("/")
      // Define the Hello World controller.
      public String hello() {
      
         ValueOperations<String, String> ops = this.template.opsForValue();

         // Add a Hello World string to your cache.
         String key = "greeting";
         if (!this.template.hasKey(key)) {
             ops.set(key, "Hello World!");
         }

         // Return the string from your cache.
         return ops.get(key);
      }
   }
   ```
   
   需要将 `com.contoso.myazuredemo` 替换为项目的包名称的地方。

1. 保存并关闭 HelloController.java 文件  。

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序，例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 使用 Web 浏览器浏览到 `http://localhost:8080` 以测试 Web 应用；如果有可用的 Curl，也可使用以下示例所示的语法：

   ```shell
   curl http://localhost:8080
   ```

   应会看到“Hello World!” 消息在示例控制器中显示，这是从 Redis 缓存中动态检索到的。

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](./index.yml)

### <a name="additional-resources"></a>其他资源

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Linux 的 Azure 应用服务上](deploy-spring-boot-java-app-on-linux.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

若要详细了解如何在 Azure 上开始将 Redis 缓存与 Java 配合使用，请参阅[如何将 Azure Redis 缓存与 Java 配合使用][Redis Cache with Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，<https://github.com/spring-guides/> 上提供了几个 Spring Boot 示例。 除了从基本的 Spring Boot 项目列表中选择之外，[Spring Initializr] 也可帮助开发人员开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: ../index.yml
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/java/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Redis Cache with Java]: /azure/redis-cache/cache-java-get-started

<!-- IMG List -->

[AZ01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ01.png
[AZ02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ02.png
[AZ03]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ03.png
[AZ04]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ04.png
[AZ05]: media/configure-spring-boot-initializer-java-app-with-redis-cache/AZ05.png

[SI01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI01.png
[SI02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI02.png
[SI03]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI03.png
[SI04]: media/configure-spring-boot-initializer-java-app-with-redis-cache/SI04.png

[RE01]: media/configure-spring-boot-initializer-java-app-with-redis-cache/RE01.png
[RE02]: media/configure-spring-boot-initializer-java-app-with-redis-cache/RE02.png
