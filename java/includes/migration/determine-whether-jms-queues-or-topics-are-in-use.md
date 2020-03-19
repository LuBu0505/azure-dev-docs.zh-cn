---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: b2458a80eccfcae24a3364208334cce3aedea861
ms.sourcegitcommit: 21ddeb9bd9abd419d143dc2ca8a7c821a1758cf9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2020
ms.locfileid: "79129015"
---
### <a name="determine-whether-jms-queues-or-topics-are-in-use"></a>确定 JMS 队列或主题是否正在使用中

如果应用程序使用的是 JMS 队列或主题，则需要将其迁移到外部托管的 JMS 服务器（例如，到 Azure 服务总线；请参阅[使用服务总线作为消息代理](/azure/service-bus-messaging/message-transfers-locks-settlement)）。

如果已配置 JMS 持久存储，则必须捕获其配置，并在迁移后应用它。
