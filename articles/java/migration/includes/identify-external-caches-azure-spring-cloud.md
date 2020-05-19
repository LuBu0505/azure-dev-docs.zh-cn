---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: f2bc0442aafcfffc963d4e5a6da18085d9205a22
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990159"
---
#### <a name="identify-external-caches"></a>确定外部缓存

确定正在使用的任何外部缓存。 通常，Redis 是通过 Spring Data Redis 使用的。 有关配置信息，请参阅 [Spring Data Redis](https://spring.io/projects/spring-data-redis) 文档。

通过搜索相应的配置（在 [Java](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-jc) 或 [XML](https://docs.spring.io/spring-session/docs/current/reference/html5/#httpsession-redis-xml)）中，确定是否正在通过 [Spring 会话](https://spring.io/projects/spring-session)缓存会话数据。
