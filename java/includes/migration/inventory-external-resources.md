---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 8c2ada1a8a11e1f0ae108e4356434825a2cf0b76
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288556"
---
### <a name="inventory-external-resources"></a>清点外部资源

外部资源（如数据源、JMS 消息代理等）通过 Java 命名和目录接口 (JNDI) 注入。 某些此类资源可能需要迁移或重新配置。

#### <a name="inside-your-application"></a>在应用程序中

检查 *META-INF/context.xml* 文件。 查找 `<Context>` 元素中的 `<Resource>` 元素。

#### <a name="on-the-application-servers"></a>在应用程序服务器上

检查 *$CATALINA_BASE/conf/context.xml* 和 *$CATALINA_BASE/conf/server.xml* 文件，以及在 *$CATALINA_BASE/conf/[engine-name]/[host-name]* 目录中发现的 *.xml* 文件。

在 *context.xml* 文件中，JNDI 资源将由顶级 `<Context>` 元素中的 `<Resource>` 元素描述。

在 *server.xml* 文件中，JNDI 资源将由 `<GlobalNamingResources>` 元素中的 `<Resource>` 元素描述。

#### <a name="datasources"></a>Datasources

数据源是 JNDI 资源，其 `type` 属性设置为 `javax.sql.DataSource`。 对于每个数据源，请记录以下信息：

* 数据源名称是什么？
* 连接池配置是什么？
* 在哪里可以找到 JDBC 驱动程序 JAR 文件？

#### <a name="all-other-external-resources"></a>所有其他的外部资源

在本指南中，不可能记录每个可能的外部依赖项。 你的团队负责验证你是否可以在迁移之后满足应用程序的所有外部依赖项的要求。