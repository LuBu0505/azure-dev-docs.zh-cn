---
title: 通过用于 Java 的 Azure SDK 配置日志记录
description: 了解如何为适用于 Java 的 Azure SDK 客户端库配置记录框架
keywords: Azure, Java, SDK, 日志记录
author: bmitchell287
ms.author: brendm
ms.date: 03/25/2020
ms.topic: article
ms.service: multiple
ms.openlocfilehash: 9f7ad8d772b7de1335ebc3ba77cdea8aa1e8b683
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81671943"
---
# <a name="configure-logging-with-the-azure-sdk-for-java"></a>通过用于 Java 的 Azure SDK 配置日志记录

本文提供适用于 Java 的 [Azure SDK](https://azure.microsoft.com/downloads/) 的示例日志记录配置。 有关配置选项的更多详细信息（例如设置日志级别或按类列出自定义日志记录），请参阅所选记录框架的文档。

适用于 Java 的 Azure SDK 客户端库使用[适用于 Java 的简单日志门面](https://www.slf4j.org/) (SLF4J)。 通过 SLF4J，你可使用首选的记录框架，该框架在应用程序部署时调用。

> [!NOTE]
> 本文适用于最新版本的 Azure SDK 客户端库。 若要查看库是否受支持，请参阅 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/java.html)列表。 如果应用程序使用旧版 Azure SDK 客户端库，请参阅适用服务文档中的具体说明。

## <a name="declare-a-logging-framework"></a>声明记录框架

在你实现这些记录器之前，必须将相关框架声明为项目中的一个依赖项。 有关详细信息，请参阅 [SLF4J 用户手册](http://www.slf4j.org/manual.html#projectDep)。

## <a name="configure-log4j-or-log4j-2"></a>配置 Log4j 或 Log4j 2

可在属性文件或 XML 文件中配置 Log4j 和 Log4j 2 日志记录。 有关 Log4j 和 Log4j 2 日志记录的详细信息，请参阅 [Apache Log4j 2 手册](https://logging.apache.org/log4j/2.x/manual/configuration.html)。

### <a name="use-a-properties-file"></a>使用属性文件

在项目的 ./src/main/resource  目录中，创建一个名为 log4j.properties 或 log4j2.properties 的新文件（后者用于 Logj4 2）   。 使用以下示例开始操作。

Log4j 示例：

```properties
log4j.rootLogger=INFO, A1
log4j.appender.A1=org.apache.log4j.ConsoleAppender
log4j.appender.A1.layout=org.apache.log4j.PatternLayout
log4j.appender.A1.layout.ConversionPattern=%m%n
log4j.logger.com.azure.core=ERROR
```

Log4j2 示例：

```properties
appender.console.type = Console
appender.console.name = LogToConsole
appender.console.layout.type = PatternLayout
appender.console.layout.pattern = %msg%n
logger.app.name=com.azure.core
logger.app.level=ERROR
```

### <a name="use-an-xml-file"></a>使用 XML 文件

你也可使用 XML 文件来配置 Log4j 和 Log4j2。 在项目的 ./src/main/resource  目录中，创建一个名为 log4j.xml 或 log4j2.xml 的新文件（后者用于 Logj4 2）   。 使用以下示例开始操作。

Log4j 示例：

```xml
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration debug="true" xmlns:log4j='http://jakarta.apache.org/log4j/'>

  <appender name="console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out"/>
    <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%m%n" />
    </layout>
  </appender>
  <logger name="com.azure.core" additivity="true">
    <level value="ERROR" />
    <appender-ref ref="console" />
  </logger>

  <root>
    <priority value ="info"></priority>
    <appender-ref ref="console"></appender>
  </root>

</log4j:configuration>
```

Log4j2 示例：

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

## <a name="configure-logback"></a>配置 Logback

[Logback](https://logback.qos.ch/manual/introduction.html) 是一种常用的记录框架，也是 SLF4J 的本机实现。 若要配置 Logback，在项目的 ./src/main/resources 目录中创建一个名为 logback.xml  的新 XML 文件。 可在 [Logback 项目网站](https://logback.qos.ch/manual/configuration.html)上查找有关配置选项的详细信息。

下面是一个 Logback 配置示例：

```xml
<?xml version="1.0" encoding="UTF-8"?>
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

下面是一个控制台日志记录的简单 Logback 配置：

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

下面是一个文件的日志记录配置，该文件每小时滚动更新一次并以 GZIP 文件格式存档：

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

### <a name="configure-logback-for-a-spring-boot-application"></a>为 Spring Boot 应用程序配置 Logback

Spring 在 application.properties  文件中查找项目配置（包括日志记录），该文件位于 ./src/main/resources  目录中。 在 application.properties  文件中，添加以下行，将 logback.xml  链接到 Spring Boot 应用程序：

```properties
logging.config=classpath:logback.xml
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

## <a name="next-steps"></a>后续步骤

- [为 Azure 应用服务中的应用启用诊断日志记录](/azure/app-service/troubleshoot-diagnostic-logs) 
- [查看 Azure 安全日志记录和审核选项](/azure/security/fundamentals/log-audit)
- [了解如何使用 Azure 平台日志](/azure/azure-monitor/platform/platform-logs-overview)
