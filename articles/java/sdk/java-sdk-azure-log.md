---
title: 通过用于 Java 的 Azure SDK 配置日志记录
description: 了解如何为适用于 Java 的 Azure SDK 客户端库配置记录框架
keywords: Azure, Java, SDK, 日志记录
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.custom: devx-track-java
ms.openlocfilehash: 927f20601ded9a0ea6b2793ef1b0c8e1b5e6ac19
ms.sourcegitcommit: ae2fa266a36958c04625bb0ab212e6f2db98e026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857765"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>通过用于 Java 的 Azure SDK 配置日志记录

本文提供适用于 Java 的 [Azure SDK](https://azure.microsoft.com/downloads/) 的示例日志记录配置。 有关配置选项的更多详细信息（例如设置日志级别或按类列出自定义日志记录），请参阅所选记录框架的文档。

适用于 Java 的 Azure SDK 客户端库使用[适用于 Java 的简单日志门面](https://www.slf4j.org/) (SLF4J)。 通过 SLF4J，你可使用首选的记录框架，该框架在应用程序部署时调用。 如果客户端生成器提供了设置 [HttpLogOptions](/java/api/com.azure.core.http.policy.httplogoptions?view=azure-java-stable) 的功能，则还必须指定 [HttpLogDetailLevel](/java/api/com.azure.core.http.policy.httplogdetaillevel?view=azure-java-stable) 以及任何允许的标头和查询参数，以便输出任何日志。

> [!NOTE]
> 本文适用于最新版本的 Azure SDK 客户端库。 若要查看库是否受支持，请参阅 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/java.html)列表。 如果应用程序使用旧版 Azure SDK 客户端库，请参阅适用服务文档中的具体说明。

## <a name="declare-a-logging-framework"></a>声明记录框架

在你实现这些记录器之前，必须将相关框架声明为项目中的一个依赖项。 有关详细信息，请参阅 [SLF4J 用户手册](https://www.slf4j.org/manual.html#projectDep)。

以下各节提供了常见记录框架的配置示例。

## <a name="use-log4j"></a>使用 Log4j

以下示例显示了 Log4j 记录框架的配置。 有关详细信息，请参阅 [Log4j 文档](https://logging.apache.org/log4j/1.2/)。

**通过添加 Maven 依赖项启用 Log4j**

在项目的 pom.xml 文件中，添加以下内容：

```xml
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-log4j12 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>[1.0,)</version> <!-- Version number 1.0 and above -->
</dependency>
```

**使用属性文件启用 Log4j**

在项目的 ./src/main/resource 目录中创建 log4j.properties 文件，然后添加以下内容 ：

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

**使用 XML 文件启用 Log4j**

在项目的 ./src/main/resource 目录中创建 log4j.xml 文件，然后添加以下内容 ：

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

    <appender name="console" class="org.apache.log4j.ConsoleAppender">
        <param name="Target" value="System.out"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%m%n" />
        </layout>
    </appender>
    <logger name="com.azure.core">
        <level value="ERROR" />
        <appender-ref ref="console" />
    </logger>

    <root>
        <level value="info" />
        <appender-ref ref="console" />
    </root>

</log4j:configuration>
```

## <a name="use-log4j-2"></a>使用 Log4j 2

以下示例显示了 Log4j 2 记录框架的配置。 有关详细信息，请参阅 [Log4j 2 文档](https://logging.apache.org/log4j/2.x/manual/configuration.html)。

通过添加 Maven 依赖项启用 Log4j 2

在项目的 pom.xml 文件中，添加以下内容：

```
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>[2.0,)</version> <!-- Version number 2.0 and above -->
</dependency>
```

使用属性文件启用 Log4j 2

在项目的 ./src/main/resource 目录中创建 log4j2.properties 文件，然后添加以下内容 ：

```properties
appender.console.type = Console
appender.console.name = STDOUT
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR

rootLogger.level = info
rootLogger.appenderRefs = stdout
rootLogger.appenderRef.stdout.ref = STDOUT
```

使用 XML 文件启用 Log4j 2

在项目的 ./src/main/resource 目录中创建 log4j2.xml 文件，然后添加以下内容 ：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="INFO">
    <Appenders>
        <Console name="console" target="SYSTEM_OUT">
            <PatternLayout
                pattern="%msg%n" />
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.azure.core" level="error" additivity="true">
            <appender-ref ref="console" />
        </Logger>
        <Root level="info" additivity="false">
            <appender-ref ref="console" />
        </Root>
     </Loggers>
</Configuration>
```

## <a name="use-logback"></a>使用 Logback

以下示例显示了 Logback 记录框架的基本配置。 有关详细信息，请参阅 [Logback 文档](https://logback.qos.ch/manual/configuration.html)。

通过添加 Maven 依赖项启用 Logback

在项目的 pom.xml 文件中，添加以下内容：

```
<!-- https://mvnrepository.com/artifact/ch.qos.logback/logback-classic -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>[0.2.5,)</version> <!-- Version number 0.2.5 and above -->
</dependency>
```

使用 XML 文件启用 Logback

在项目的 ./src/main/resource 目录中创建 logback.xml 文件，然后添加以下内容 ：

```xml
<configuration>
  <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
    <encoder>
      <pattern>%message%n</pattern>
    </encoder>
  </appender>

  <logger name="com.azure.core" level="ERROR" />

  <root level="INFO">
    <appender-ref ref="STDOUT" />
  </root>
</configuration>
```

## <a name="use-logback-in-a-spring-boot-application"></a>在 Spring Boot 应用程序中使用 Logback

以下示例显示了在 Spring 中使用 Logback 的一些配置。 你通常会将日志记录配置添加到项目的 ./src/main/resources 目录的 logback.xml 文件中 。 Spring 查看此文件的各种配置，包括日志记录。 有关详细信息，请参阅 [Logback 文档](https://logback.qos.ch/manual/configuration.html)。

可配置应用程序以从任何文件中读取 Logback 配置。 若要将 logback.xml 文件链接到 Spring 应用程序，请在项目的 ./src/main/resources 目录中创建 application.properties 文件，然后添加以下内容  ：

```properties
logging.config=classpath:logback.xml
```

若要创建 Logback 配置以使日志记录到控制台，请将以下内容添加到 logback.xml 文件：

```xml 
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <appender name="Console"
    class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      <Pattern>
        %black(%d{ISO8601}) %highlight(%-5level) [%blue(%t)] %blue(%logger{100}): %msg%n%throwable
      </Pattern>
    </layout>
  </appender>

  <root level="INFO">
    <appender-ref ref="Console" />
  </root>
</configuration>
```

若要将日志配置为记录到每小时滚动更新一次并以 gzip 格式存档的文件中，请将以下内容添加到 logback.xml 文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <property name="LOGS" value="./logs" />
  <appender name="RollingFile" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <file>${LOGS}/spring-boot-logger.log</file>
    <encoder
      class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
      <Pattern>%d %p %C{1.} [%t] %m%n</Pattern>
    </encoder>

    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
      <!-- rollover hourly and gzip logs -->
      <fileNamePattern>${LOGS}/archived/spring-boot-logger-%d{yyyy-MM-dd-HH}.log.gz</fileNamePattern>
    </rollingPolicy>
  </appender>

  <!-- LOG everything at INFO level -->
  <root level="info">
    <appender-ref ref="RollingFile" />
  </root>
</configuration>
```

## <a name="configure-fallback-logging-for-temporary-debugging"></a>为临时调试配置回退日志记录

在无法使用 SLF4J 记录器重新部署应用程序的情况下，你可在 Azure Core 1.3.0 或更高版本中使用内置于 Java 适用的 Azure 客户端库的回退记录器。 若你要启用此记录器，则必须首先确认你没有 SLF4J 记录器（因为优先使用该记录器），然后设置 `AZURE_LOG_LEVEL` 环境变量。 设置该环境变量后，你需重启应用程序以开始生成日志。

下表显示了此环境变量的允许值。

|日志级别   |允许的环境变量值   |
|----------|-----------|
|详细   |verbose、debug     |
|信息|info、information、informational  |
|WARNING     |warn、warning       |
|ERROR    |err、error  |

## <a name="setting-an-httplogdetaillevel"></a>设置 HttpLogDetailLevel
无论使用哪种日志记录机制，如果客户端生成器提供了设置 [HttpLogOptions](/java/api/com.azure.core.http.policy.httplogoptions?view=azure-java-stable) 的功能，则还必须额外配置这些选项以输出任何日志。 必须指定 [HttpLogDetailLevel](/java/api/com.azure.core.http.policy.httplogdetaillevel?view=azure-java-stable) 以指示应记录哪些信息。  此值默认为 `NONE`，因此，如果未指定，即使正确配置了日志记录框架或回退日志记录，也不会输出任何日志。 出于安全原因，系统会默认修正标头和查询参数，因此还必须为日志选项提供一个 `Set<String>`，以指示哪些标头和查询参数可以安全地打印。 可按下面所示配置这些值。 日志记录设置为同时打印正文内容和标头值，除用户指定的与键 `"foo"` 相对应的元数据的值外，所有标头值都将被修正，并且除 sas 令牌查询参数 `"sv"`（指示可能存在的任何 sas 的已签名版本）外，所有查询参数都将被修正。 

```
new BlobClientBuilder().endpoint(<endpoint>)
            .httpLogOptions(new HttpLogOptions()
                .setLogLevel(HttpLogDetailLevel.BODY_AND_HEADERS)
                .setAllowedHeaderNames(Set.of("x-ms-meta-foo"))
                .setAllowedQueryParamNames(Set.of("sv")))
            .buildClient();
```
> [!NOTE]
> 此示例使用存储客户端生成器，但此原则适用于任何接受 `HttpLogOptions` 的生成器。 此外，此示例不会演示客户端的完整配置，仅会说明日志记录的配置。 有关配置客户端的详细信息，请参阅各自生成器的相关文档。

## <a name="next-steps"></a>后续步骤

- [在 Azure 应用服务中为应用启用诊断日志记录](/azure/app-service/troubleshoot-diagnostic-logs) 
- [查看 Azure 安全日志记录和审核选项](/azure/security/fundamentals/log-audit)
- [了解如何使用 Azure 平台日志](/azure/azure-monitor/platform/platform-logs-overview)
