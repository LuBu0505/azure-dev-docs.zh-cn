---
title: 将 Spring Boot Initializer 应用配置为使用 Azure Application Insights SpringBoot Starter
description: 将使用 Spring Initializr 创建的 Spring Boot 应用程序配置为使用 Application Insights Spring Boot Starter。
services: Application-Insights
documentationcenter: java
author: dhaval24
ms.author: dhdoshi
ms.date: 11/29/2019
ms.devlang: java
ms.service: azure-monitor
ms.tgt_pltfrm: application-insights
ms.topic: article
ms.openlocfilehash: 25a036e129d6a8853059d61da7ff8565ab0ddd72
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118265"
---
# <a name="configure-a-spring-boot-initializer-app-to-use-application-insights"></a>将 Spring Boot Initializer 应用配置为使用 Application Insights

本文逐步讲解如何使用 **[Spring Initializr]** 创建 Spring Boot 应用程序。 该应用程序使用 Azure Application Insights Spring Boot Starter 对云中的 Java 应用程序进行端到端监视。

## <a name="prerequisites"></a>先决条件

为完成本文介绍的步骤，需要满足以下先决条件：

* Azure 订阅。 如果还没有 Azure 订阅，可以激活 [MSDN 订阅者权益]或注册获取[免费的 Azure 帐户]。
* 一个受支持的 Java 开发工具包 (JDK)。 有关在 Azure 上进行开发时可供使用的 JDK 的详细信息，请参阅 <https://aka.ms/azure-jdks>。
* [Apache Maven](http://maven.apache.org/) 3.0 或更高版本。
* Application Insights Spring Boot Starter **目前不支持** Web Flux 和 Netty API。

## <a name="create-a-custom-application-using-spring-initializr"></a>使用 Spring Initializr 创建自定义应用程序

使用以下过程创建应用程序。

1. 浏览到 [https://start.spring.io/](https://start.spring.io/)。

1. 指定你想要使用 **Java** 生成 **Maven** 项目，输入应用程序的“组”名称和“项目”名称，然后在依赖项部分选择 Web 依赖项。  

   ![Spring Initializr 的基本选项][SI01]

   > [!NOTE]
   >
   > Spring Initializr 将使用“组”名称和“项目”名称创建包名称，例如：*com.vged.appinsights*。  
   >

1. 单击“生成”按钮。 

1. 出现提示时，将项目下载到本地计算机中的路径。

1. 提取本地系统上的文件之后，自定义 Spring Boot 应用程序便可进行编辑。

## <a name="create-an-application-insights-resource-on-azure"></a>在 Azure 中创建 Application Insights 资源

使用以下过程创建 Application Insights 资源。

1. 浏览到位于 <https://portal.azure.com/> 的 Azure，然后单击“+ 创建新资源”  。

1. 单击“IT 和管理工具”，然后单击“Application Insights”。  

1. 在“新建 Application Insights 资源”页上，输入以下信息  ：

* 指定你的“订阅”和“资源组”。  
* 输入 Application Insights 资源的**名称**。
* 选择“区域”。 

   指定这些选项后，单击“查看并创建”  。

   ![Azure][AZ03]

* 查看具体细节，然后单击“创建”  。

创建资源后，Azure **仪表板**上以及“所有资源”页下会列出该资源。  可在上述任何位置单击该资源，打开 Application Insights 资源概述页。

请复制此概览页中的**检测密钥**。

   ![Azure][AZ04]

## <a name="configure-your-downloaded-spring-boot-application-to-use-application-insights"></a>将下载的 Spring Boot 应用程序配置为使用 Application Insights

使用以下过程配置应用程序。

1. 在应用的根目录中找到 *POM.xml* 文件，并在其 dependencies 节中添加以下依赖项。

```XML
 <dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>applicationinsights-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

1. 在应用的“资源”目录中找到 application.properties 文件，或创建此文件（若此文件不存在）   。

1. 在文本编辑器中打开 *application.properties* 文件，将以下行添加到该文件，然后将示例值替换为包含相应凭据的相应属性：

   ```yaml
   # Specify the instrumentation key of your Application Insights resource.
   azure.application-insights.instrumentation-key=[your ikey from the resource]
   # Specify the name of your springboot application. This can be any logical name you would like to give to your app.
   spring.application.name=[your app name]
   ```

   有关微调 Application Insights 的其他方法，请参阅 [Application Insights Springboot Starter 自述文件](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。

   > [!NOTE]
   > 
   > 将不同的 Application Insights 检测密钥（例如不同的资源）用于不同的配置文件（例如 PROD、DEV 等）。有关更多信息，请参阅 [Spring Boot 配置文件的特定属性]。 

1. 保存并关闭 application.properties 文件  。

1. 在包的源文件夹中创建名为“控制器”的文件夹，例如： 

   `D:\Microsoft\demo\src\main\java\com\example\demo\controller`

   -或-

   `/users/example/home/demo/src/main/java/com/example/demo/controller`

1. 在 *controller* 文件夹中创建名为 *TestController.java* 的新文件。 在文本编辑器中打开该文件，然后向其添加以下代码：

   ```java
    package com.example.demo;

    import com.microsoft.applicationinsights.TelemetryClient;
    import java.io.IOException;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;
    import com.microsoft.applicationinsights.telemetry.Duration;

    @RestController
    @RequestMapping("/sample")
    public class TestController {

        @Autowired
        TelemetryClient telemetryClient;

        @GetMapping("/hello")
        public String hello() {

            //track a custom event  
            telemetryClient.trackEvent("Sending a custom event...");

            //trace a custom trace
            telemetryClient.trackTrace("Sending a custom trace....");

            //track a custom metric
            telemetryClient.trackMetric("custom metric", 1.0);

            //track a custom dependency
            telemetryClient.trackDependency("SQL", "Insert", new Duration(0, 0, 1, 1, 1), true);

            return "hello";
        }
    }
   ```

   将 `com.example.demo` 替换为项目的包名称。

1. 保存并关闭 *TestController.java* 文件。

1. 使用 Maven 生成 Spring Boot 应用程序，然后运行该程序。 例如：

   ```shell
   mvn clean package
   mvn spring-boot:run
   ```

1. 使用 Web 浏览器浏览到 http://localhost:8080/sample/hello 以测试 Web 应用；如果有可用的 **curl**，也可使用类似以下示例的语法：

   ```shell
   curl http://localhost:8080/sample/hello
   ```

   示例控制器中应会显示 “hello!”消息。 Application Insights 会自动收集此请求，并将其作为遥测项连同控制器逻辑中指定的关联自定义事件、自定义指标、自定义依赖项和自定义跟踪一起发送。 

   几秒钟后，Azure 中会显示数据。 

   ![Azure][AZ05]

单击“应用程序映射”磁贴，查看高级组件及其彼此之间的交互。  建议在此位置获取整个应用程序的简要概述。 可根据 Spring 应用程序名称识别每个 Spring Boot 微服务。 记得设置它。

   ![Azure][AZ08] 

## <a name="configure-springboot-application-to-send-log4j-logs-to-application-insights"></a>将 Springboot 应用程序配置为向 Application Insights 发送 log4j 日志

使用以下过程配置应用程序，以便发送日志。

1. 修改项目的 POM.xml 文件，并按如下所示添加/修改 dependencies 节。 

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-logging</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-log4j2</artifactId>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>applicationinsights-logging-log4j2</artifactId>
        <version>2.1.1</version>
    </dependency>
</dependencies>
```

2. 保存并关闭 *POM.xml* 文件。

3. 在 \src\main\resources 文件夹中创建新文件 *log4j2.xml* 并对其进行配置。 例如：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Configuration packages="com.microsoft.applicationinsights.log4j.v2">
  <Properties>
    <Property name="LOG_PATTERN">
      %d{yyyy-MM-dd HH:mm:ss.SSS} %5p ${hostName} --- [%15.15t] %-40.40c{1.} : %m%n%ex
    </Property>
  </Properties>
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
    <ApplicationInsightsAppender name="aiAppender">
    </ApplicationInsightsAppender>
  </Appenders>
  <Loggers>
    <Root level="trace">
      <AppenderRef ref="Console"  />
      <AppenderRef ref="aiAppender"  />
    </Root>
  </Loggers>
</Configuration>
```

4. 再次按前面所示生成并运行 Spring Boot 应用程序。

几秒钟内，Azure 中会显示所有 Spring 日志。 可以在分析门户中查看详细的日志消息和执行分析。

![Azure][AZ07]

## <a name="next-steps"></a>后续步骤

若要了解有关 Spring 和 Azure 的详细信息，请继续访问“Azure 上的 Spring”文档中心。

> [!div class="nextstepaction"]
> [Azure 上的 Spring](/azure/java/spring-framework)

### <a name="additional-resources"></a>其他资源

有关使用 Azure 上的 Spring Boot 应用程序的详细信息，请参阅以下文章：

* [将 Spring Boot 应用程序部署到 Azure 应用服务](deploy-spring-boot-java-app-from-container-registry-using-maven-plugin.md)

* [在 Azure 容器服务中运行 Kubernetes 群集上的 Spring Boot 应用程序](deploy-spring-boot-java-app-on-kubernetes.md)

Application Insights 支持收集外部依赖项及其与传入请求之间的关联。 目前支持自动收集 Oracle、MsSQL、MySQL 和 Redis 数据。 有关启用自动收集的更多详细信息，请参阅[如何使用 Application Insights Java 代理](/azure/application-insights/app-insights-java-agent)一文。

有关 Azure Application Insights 的详细信息及其监视功能，请查看 **[Application Insights]** 主页。

有关 Application Insights Spring Boot Starter 的其他配置详细信息，请参阅[此链接](https://github.com/Microsoft/ApplicationInsights-Java/blob/master/azure-application-insights-spring-boot-starter/README.md)。

如需请求新功能和报告潜在的 bug，请在 [GitHub](https://github.com/Microsoft/ApplicationInsights-Java/issues) 存储库中提出问题。

有关如何将 Azure 与 Java 配合使用的详细信息，请参阅[面向 Java 开发人员的 Azure] 和[使用 Azure DevOps 和 Java]。

[Spring Framework] 是一种开放源代码解决方案，可帮助 Java 开发人员创建企业级应用程序  。 基于该平台构建的其中一个更常用的项目是 [Spring Boot]，该项目提供了一种用于创建独立 Java 应用程序的简化方法。 为帮助开发人员开始使用 Spring Boot，[https://github.com/spring-guides/](https://github.com/spring-guides/) 上提供了几个 Spring Boot 示例。 除了从基本 Spring Boot 项目列表中选择之外，开发人员还可以通过 **[Spring Initializr]** 来开始创建自定义 Spring Boot 应用程序。

<!-- URL List -->

[面向 Java 开发人员的 Azure]: /azure/java/
[免费的 Azure 帐户]: https://azure.microsoft.com/pricing/free-trial/
[使用 Azure DevOps 和 Java]: /azure/devops/
[MSDN 订阅者权益]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[Spring Boot]: http://projects.spring.io/spring-boot/
[Spring Boot 配置文件的特定属性]: https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html#boot-features-external-config-profile-specific-properties
[Spring Initializr]: https://start.spring.io/
[Spring Framework]: https://spring.io/
[Application Insights]: /azure/application-insights/

<!-- IMG List -->

[AZ01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource.png
[AZ02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_2.png
[AZ03]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Create_resource_3.png
[AZ04]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Get_IKey.png
[AZ05]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/OverviewBladeResults.png
[AZ06]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/Search_and_traces.png
[AZ07]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/traces_details.png
[AZ08]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/AppMap.png

[SI01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/spring_start.PNG
[SI02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/After_extract.png

[RE01]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationproperties_loc.png
[RE02]: ./media/configure-spring-boot-starter-java-app-with-azure-application-insights/applicationinsightsproperties.png
