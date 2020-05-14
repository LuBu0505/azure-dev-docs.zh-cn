---
author: judubois
ms.date: 05/06/2020
ms.author: judubois
ms.openlocfilehash: 1f73c46d5bc259c94e58a48a8a6bdb883a4454fd
ms.sourcegitcommit: a631b36ec1277ee9397a860c597ffdd5495d88e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/13/2020
ms.locfileid: "83369823"
---
在主 `DemoApplication` 类中配置新的 Spring Bean，它将创建数据库架构：

```java
    @Bean
    public ConnectionFactoryInitializer initializer(ConnectionFactory connectionFactory) {
        ConnectionFactoryInitializer initializer = new ConnectionFactoryInitializer();
        initializer.setConnectionFactory(connectionFactory);
        ResourceDatabasePopulator populator = new ResourceDatabasePopulator(new ClassPathResource("schema.sql"));
        initializer.setDatabasePopulator(populator);
        return initializer;
    }
```

此 Spring Bean 使用名为 schema.sql  的文件，因此请在 src/main/resources  文件夹中创建该文件：
