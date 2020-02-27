---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 4c20feba0a91d10846963f8697457735e5932c20
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830725"
---
### <a name="determine-whether-session-replication-is-used"></a>确定是否使用了会话复制

如果应用程序依赖于会话复制，则无论是否有 Oracle Coherence*Web，都可以使用三个选项：

1. Coherence*Web 可以在 Azure 虚拟机中与 WebLogic Server 一起运行，但你必须在预配套餐后手动配置此选项。 如果使用独立的 Coherence，也可在 Azure 虚拟机中运行它，但必须在预配套餐后手动配置此选项。
2. 重构应用程序，使用数据库进行会话管理。
3. 重构应用程序，将 Azure Redis 服务的会话外部化。 有关详细信息，请参阅 [Azure Cache for Redis](/azure/azure-cache-for-redis/cache-overview)。

对于所有这些选项，最后是掌握 WebLogic 如何执行 HTTP 会话状态复制。 有关详细信息，请参阅 Oracle 文档中的 [HTTP 会话状态复制](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/failover.html#GUID-E13D8142-66BA-46A1-854F-4FC6F82992DD)。