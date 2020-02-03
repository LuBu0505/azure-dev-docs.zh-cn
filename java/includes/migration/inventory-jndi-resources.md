---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 171dac6ad7e2761d58f63d173cc78464cedb6dc5
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830805"
---
### <a name="inventory-jndi-resources"></a>清点 JNDI 资源

清点所有 JNDI 资源。 例如，数据库等数据源可能具有关联的 JNDI 名称，该名称允许 JPA 正确地将 `EntityManager` 的实例绑定到特定数据库。 有关 JNDI 资源和数据库的详细信息，请参阅 Oracle 文档中的 [WebLogic Server Data Sources](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html)（WebLogic Server 数据源）。 其他 JNDI 相关资源（如 JMS 消息代理）可能需要迁移或重新配置。 有关 JMS 配置的详细信息，请参阅 [Understanding JMS Resource Configuration](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/jmsad/overview.html)（了解 JMS 资源配置）。
