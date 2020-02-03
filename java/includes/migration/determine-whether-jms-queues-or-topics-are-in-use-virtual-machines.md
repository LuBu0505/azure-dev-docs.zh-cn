---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: e36fae7adfda8c05e222151872b137fa600cdd97
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830765"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>确定 JMS 队列或主题是否正在使用中

如果应用程序使用 JMS 队列或主题，则需将其迁移到外部托管的 JMS 服务器。 Azure 服务总线和高级消息队列协议可成为那些使用 JMS 的项目的理想迁移策略。 有关详细信息，请参阅[将 Java 消息服务 (JMS) 与 Azure 服务总线和 AMQP 1.0 配合使用](/azure/service-bus-messaging/service-bus-java-how-to-use-jms-api-amqp)。

如果已配置 JMS 持久存储，则必须捕获其配置，并在迁移后应用它。

如果使用 Oracle Message Broker，则可将此软件迁移到 Azure 虚拟机并按原样使用。
