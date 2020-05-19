---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: e9d6993c96fc90c065cda5f8b3177147c35d0242
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990259"
---
下面是 application.properties 文件中的 ActiveMQ 示例：

```properties
spring.activemq.brokerurl=broker:(tcp://localhost:61616,network:static:tcp://remotehost:61616)?persistent=false&useJmx=true
spring.activemq.user=admin
spring.activemq.password=tryandguess
```

有关 ActiveMQ 配置的详细信息，请参阅 [Spring Boot 消息文档](https://docs.spring.io/spring-boot/docs/2.0.x/reference/html/boot-features-messaging.html)。

下面是 application.yaml 文件中的 IBM MQ 示例：

```yaml
ibm:
  mq:
    queueManager: qm1
    channel: dev.ORDERS
    connName: localhost(14)
    user: admin
    password: big$ecr3t
```

有关 IBM MQ 配置的详细信息，请参阅 [IBM MQ Spring 组件文档](https://github.com/ibm-messaging/mq-jms-spring#ibm-mq-jms-spring-components)。
