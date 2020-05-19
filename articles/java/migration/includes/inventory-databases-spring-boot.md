---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: 6bf4c54c8f428e3ffce79a7ba9ac49aae028e2fd
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990229"
---
#### <a name="databases"></a>数据库

确定任何 SQL 数据库的连接字符串。

Spring Boot 应用程序的连接字符串通常出现在配置文件中。 

下面是 *application.properties* 文件中的示例：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/mysql_db
spring.datasource.username=dbuser
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
```

下面是 *application.yaml* 文件中的示例：

```yaml
spring:
  data:
    mongodb:
      uri: mongodb://mongouser:deepsecret@mongoserver.contoso.com:27017
```

有关可能性更大的配置场景，请参阅 Spring 数据文档：

* [JPA 存储库](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#jpa.repositories)
* [JDBC 存储库](https://docs.spring.io/spring-data/jdbc/docs/current/reference/html/#jdbc.repositories)
* [Cassandra 存储库](https://docs.spring.io/spring-data/cassandra/docs/current/reference/html/#cassandra.repositories)
* [MongoDB 存储库](https://docs.spring.io/spring-data/mongodb/docs/current/reference/html/#mongodb.repositories)
