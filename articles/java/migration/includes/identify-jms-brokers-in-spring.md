---
author: yevster
ms.author: yebronsh
ms.date: 2/12/2020
ms.openlocfilehash: d17b69ea5299c24406a63fc6239c350b6cbcbdd4
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990139"
---
#### <a name="jms-message-brokers"></a>JMS 消息代理

通过在生成清单（通常是 pom.xml 或 build.gradle 文件）中查找相关依赖项，确定所使用的一个或多个代理 。

例如，使用 ActiveMQ 的 Spring Boot 应用程序通常会在其 pom.xml 文件中包含此依赖项：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-activemq</artifactId>
</dependency>
```

使用专用代理的 Spring Boot 应用程序通常直接在代理的 JMS 驱动程序库中包含依赖项。 下面是 *build.gradle* 文件中的示例：

```json
    dependencies {
      ...
      compile("com.ibm.mq:com.ibm.mq.allclient:9.0.4.0")
      ...
    }
```
