---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a521396292214fd4e68b4625a0e5f5bcfca8be92
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672913"
---
### <a name="determine-whether-java-message-service-jms-queues-or-topics-are-in-use"></a>确定 Java 消息服务 (JMS) 队列或主题是否正在使用中

如果应用程序使用 JMS 队列或主题，则需将其迁移到外部托管的 JMS 服务器。 Azure 服务总线和高级消息队列协议 (AMQP) 可成为那些使用 JMS 的项目的理想迁移策略。 有关详细信息，请参阅[将 JMS 与 Azure 服务总线和 AMQP 1.0 配合使用](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)。

如果已配置 JMS 持久存储，则必须捕获其配置，并在迁移后应用它。
