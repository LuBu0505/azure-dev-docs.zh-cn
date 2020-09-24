---
title: 用于在 Azure 上运行的 Java 应用的 Logz.io 入门
description: 本教程介绍如何集成并配置用于在 Azure 上运行的 Java 应用的 Logz.io。
author: jdubois
manager: bborges
ms.topic: tutorial
ms.date: 11/05/2019
ms.author: judubois
ms.custom: devx-track-java
ms.openlocfilehash: 89dc2a40e39a417d33e59b22f28202c61f036c79
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831483"
---
# <a name="tutorial-getting-started-with-monitoring-and-logging-using-logzio-for-java-apps-running-on-azure"></a>教程：开始使用用于在 Azure 上运行的 Java 应用的 Logz.io 进行监视和日志记录

本教程介绍如何配置经典 Java 应用程序，以便将日志发送到 [Logz.io](https://logz.io/) 服务进行引入和分析。 Logz.io 提供基于 Elasticsearch/Logstash/Kibana (ELK) 和 Grafana 的完整监视解决方案。

本教程假设你使用 Log4J 或 Logback。 这些库是两个最广泛用于在 Java 中进行日志记录的库，因此本教程应该适用于在 Azure 上运行的大多数应用程序。 如果你已使用 Elastic Stack 监视 Java 应用程序，则可在本教程中了解如何通过重新配置将 Logz.io 终结点设为目标。

本教程介绍以下操作：

> [!div class="checklist"]
> * 将日志从现有 Java 应用程序发送到 Logz.io。
> * 将诊断日志和指标从 Azure 服务发送到 Logz.io。

## <a name="prerequisites"></a>先决条件

* [Java 开发人员工具包](./java-jdk-long-term-support.md)第 8 版或更高版本
* 来自 [Azure 市场](https://azuremarketplace.microsoft.com/marketplace/apps/logz.logzio-elk-as-a-service-pro)的 Logz.io 帐户
* 一个使用 Log4J 或 Logback 的现有 Java 应用程序

## <a name="send-java-application-logs-to-logzio"></a>将 Java 应用程序日志发送到 Logz.io

首先，我们将学习如何使用令牌来配置 Java 应用程序，使其能够访问 Logz.io 帐户。

### <a name="get-your-logzio-access-token"></a>获取 Logz.io 访问令牌

若要获取令牌，请登录到 Logz.io 帐户，选择右上/下角的齿轮图标，然后选择“设置”>“常规”。  复制帐户设置中显示的访问令牌供以后使用。

### <a name="install-and-configure-the-logzio-library-for-log4j-or-logback"></a>安装并配置用于 Log4J 或 Logback 的 Logz.io 库

Logz.io Java 库在 Maven Central 上提供，因此可将其作为依赖项添加到应用配置中。 请在 Maven Central 上检查版本号，并在以下配置设置中使用最新版本。

如果使用 Maven，请将以下依赖项添加到 `pom.xml` 文件：

**Log4J：**

```xml
<dependency>
    <groupId>io.logz.log4j2</groupId>
    <artifactId>logzio-log4j2-appender</artifactId>
    <version>1.0.11</version>
</dependency>
```

**Logback：**

```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.22</version>
</dependency>
```

如果使用 Gradle，请将以下依赖项添加到生成脚本：

**Log4J：**

```
implementation 'io.logz.log4j:logzio-log4j-appender:1.0.11'
```

**Logback：**

```
implementation 'io.logz.logback:logzio-logback-appender:1.0.22'
```

接下来，更新 Log4J 或 Logback 配置文件：

**Log4J：**

```xml
<Appenders>
    <LogzioAppender name="Logzio">
        <logzioToken><your-logz-io-token></logzioToken>
        <logzioType>java-application</logzioType>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
    </LogzioAppender>
</Appenders>

<Loggers>
    <Root level="info">
        <AppenderRef ref="Logzio"/>
    </Root>
</Loggers>
```

**Logback：**

```xml
<configuration>
    <!-- Use shutdownHook so that we can close gracefully and finish the log drain -->
    <shutdownHook class="ch.qos.logback.core.hook.DelayingShutdownHook"/>
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <token><your-logz-io-token></token>
        <logzioUrl>https://<your-logz-io-listener-host>:8071</logzioUrl>
        <logzioType>java-application</logzioType>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>INFO</level>
        </filter>
    </appender>

    <root level="debug">
        <appender-ref ref="LogzioLogbackAppender"/>
    </root>
</configuration>
```

将 `<your-logz-io-token>` 占位符替换为你的访问令牌，并将 `<your-logz-io-listener-host>` 占位符替换为区域的侦听器主机（例如，listener.logz.io）。 有关查找帐户区域的详细信息，请参阅 [帐户区域](https://docs.logz.io/user-guide/accounts/account-region.html)。

`logzioType` 元素是指 Elasticsearch 中的一个逻辑字段，用于将不同的文档互相分隔开。 必须正确配置此参数，以便充分利用 Logz.io。

Logz.io“类型”是指日志格式（例如：Apache、NGinx、MySQL），而不是源（例如：server1、server2、server3）。 在本教程中，我们将该类型称为 `java-application`，因为我们将配置 Java 应用程序，并且期望这些应用程序都会有相同的格式。

至于高级用法，我们可以将 Java 应用程序分组为不同的类型，这些类型都有自己的特定日志格式（可以通过 Log4J 和 Logback 进行配置）。 例如，可以有“spring-boot-monolith”类型和“spring-boot-microservice”类型。

### <a name="test-your-configuration-and-log-analysis-on-logzio"></a>在 Logz.io 上测试配置和日志分析

配置 Logz.io 库以后，应用程序就会直接将日志发送到其中。 若要测试是否一切正常，请转到 Logz.io 控制台，选择“Live Tail”选项卡，然后选择“运行”。   应看到类似于以下内容的消息，表明连接正常：

```output
Requesting Live Tail access...
Access granted. Opening connection...
Connected. Tailing...
````

接下来，启动应用程序或用其生成一些日志。 日志应该直接显示在屏幕上。 例如，下面是 Spring Boot 应用程序的头几条启动消息：

```output
2019-09-19 12:54:40.685Z Starting JavaApp on javaapp-default-9-5cfcb8797f-dfp46 with PID 1 (/workspace/BOOT-INF/classes started by cnb in /workspace)
2019-09-19 12:54:40.686Z The following profiles are active: prod
2019-09-19 12:54:42.052Z Bootstrapping Spring Data repositories in DEFAULT mode.
2019-09-19 12:54:42.169Z Finished Spring Data repository scanning in 103ms. Found 6 repository interfaces.
2019-09-19 12:54:43.426Z Bean 'spring.task.execution-org.springframework.boot.autoconfigure.task.TaskExecutionProperties' of type [org.springframework.boot.autoconfigure.task.TaskExecutionProperties] is not eligible for getting processed by all BeanPostProcessors (for example: not eligible for auto-proxying)
```

由于日志由 Logz.io 处理，因此可以利用该平台的所有服务。

## <a name="send-azure-services-data-to-logzio"></a>将 Azure 服务数据发送到 Logz.io

接下来介绍如何将日志和指标从 Azure 资源发送到 Logz.io。

### <a name="deploy-the-template"></a>部署模板

第一步是部署 Logz.io - Azure 集成模板。 集成基于现成的 Azure 部署模板，该模板可设置管道的所有必需构建基块。 该模板创建一个事件中心命名空间、一个事件中心、两个存储 blob，以及所有必需的正确权限和连接。 通过自动部署设置的资源可以收集单个 Azure 区域的数据并将该数据传送到 Logz.io。

请找到在[存储库的自述文件的第一步](https://github.com/logzio/logzio-azure-serverless)中显示的“部署到 Azure”按钮。 

选择“部署到 Azure”时，   Azure 门户中的“自定义部署”页就会显示，其中有一个预先填充字段的列表。

可以让大部分字段保留原样，但务必输入以下设置：

* **资源组**：选择现有组或创建新组。
* **Logzio 日志/指标主机**：输入 Logz.io 侦听器的 URL。 如果不确定该 URL 是什么，请检查登录 URL。 如果它是 app.logz.io，请使用 listener.logz.io（默认设置）。 如果它是 app-eu.logz.io，请使用 listener-eu.logz.io。
* **Logzio 日志/指标令牌**：输入要将 Azure 日志或指标传送到其中的 Logz.io 帐户的令牌。 可以在 Logz.io UI 中的帐户页上找到该令牌。

同意页面底部的条款，然后选择“购买”。  然后，Azure 就会部署该模板，可能需要一到两分钟。 最终会在门户顶部看到“部署成功”消息。

可以通过访问定义的资源组来查看部署的资源。

若要了解如何配置 logzio-azure-serverless 以将数据备份到 Azure Blob 存储，请参阅 [Ship Azure activity logs](https://docs.logz.io/shipping/log-sources/azure-activity-logs.html)（传送 Azure 活动日志）。

### <a name="stream-azure-logs-and-metrics-to-logzio"></a>将 Azure 日志和指标流式传输到 Logz.io

由于已部署集成模板，因此需配置 Azure，以便将诊断数据流式传输到刚部署的事件中心。 当数据进入事件中心以后，函数应用随后会将该数据转发到 Logz.io。

1. 在搜索栏中键入“诊断”，然后选择“诊断设置”  。

2. 从资源列表中选择一项资源，然后选择“添加诊断设置”，打开该资源的“诊断设置”面板   。

    ![“诊断设置”面板](media/java-get-started-with-logzio/diagnostics-settings.png)

3. 为诊断设置提供一个**名称**。

4. 选择“流式传输到事件中心”  ，然后选择“配置”，以便打开“选择事件中心”面板。  

5. 选择事件中心：

    * **选择事件中心命名空间**：选择以 **Logzio** 开头的命名空间（例如 `LogzioNS6nvkqdcci10p`）。
    * **选择事件中心名称**：对于日志，请选择 **insights-operational-logs**；对于指标，请选择 **insights-operational-metrics**。
    * **选择事件中心策略名称**：选择 **LogzioSharedAccessKey**。

6. 选择“确定”，返回到“诊断设置”面板。  

7. 在“日志”部分，选择要流式传输的数据，然后选择“保存”。 

所选数据现在会流式传输到事件中心。

### <a name="visualize-your-data"></a>可视化数据

接下来，为数据留出一些时间从你的系统传送到 Logz.io，然后打开 Kibana。 应该会看到数据（类型为 _eventhub_）填充仪表板。 有关如何创建仪表板的详细信息，请参阅 [Creating the Perfect Kibana Dashboard](https://logz.io/blog/perfect-kibana-dashboard/)（创建完美的 Kibana 仪表板）。

可以在该仪表板的“发现”选项卡中查询特定数据，或者创建  Kibana 对象，以便在“可视化”选项卡中将数据可视化。 

## <a name="clean-up-resources"></a>清理资源

使用完本教程中创建的 Azure 资源后，可以用以下命令将其删除：

```azurecli-interactive
az group delete --name <resource group>
```

## <a name="next-steps"></a>后续步骤

本教程介绍了如何配置 Java 应用程序和 Azure 服务，以便将日志和指标发送到 Logz.io。

接下来会详细介绍如何使用事件中心来监视应用程序：

> [!div class="nextstepaction"]
> [将 Azure 监视数据流式传输到事件中心供外部工具使用](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)