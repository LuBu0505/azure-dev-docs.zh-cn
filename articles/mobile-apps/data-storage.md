---
title: 通过 Visual Studio App Center 和 Azure 服务在云中存储、管理和保存应用程序数据
description: 了解可用于在云中存储、管理和保存移动应用程序数据的 Visual Studio App Center 等服务。
author: codemillmatt
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/08/2020
ms.author: masoucou
ms.openlocfilehash: 1fc4fa3118368bd459024d894094ac2eac4b1b71
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85790661"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>在云中存储、同步和查询移动应用程序数据

无论生成何种类型的应用程序，都可能会生成和处理数据。 应用程序的用户具有较高的期望。 他们希望应用程序在所有情况下都能快速、无缝地工作。 大多数应用程序也可跨多个设备工作。 可从桌面或移动设备访问应用程序。 多个用户可以同时使用应用程序并共享数据，以便即时且实时访问数据。

应用程序用户不会始终具有 Internet 连接。 应用程序设计为使用或不使用 Internet 连接。 开发人员必须选择正确的解决方案来存储数据并将其同步到云，以便为客户提供出色的应用程序体验，这可能包括开发自己的脱机数据存储。

Microsoft 提供了各种服务，无需启动服务器、选择数据库或者担心规模或安全性，即可提供尽可能多的体验。 这些服务提供了出色的开发人员体验，可让你使用 SQL 或 NoSQL API 在云中存储应用程序数据。 还可以在所有设备上同步数据，并使应用程序可以使用或不使用网络连接来帮助生成可缩放且可靠的应用程序。

使用以下服务在云中管理和存储移动应用程序数据。

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) 是一种全球分布式多模型数据库服务。 可用于生成全球范围内的应用程序。 使用 Azure Cosmos DB 可跨任意数量的全球 Azure 区域弹性且独立地缩放吞吐量和存储。 可以通过最喜爱的 API 外围应用利用个位数毫秒级的快速数据访问。 这些外围应用包括 SQL、MongoDB、Cassandra、表或 Gremlin。 Azure Cosmos DB 为吞吐量、延迟、可用性和一致性唯一提供综合服务级别协议 (SLA)。

### <a name="azure-cosmos-db-features"></a>Azure Cosmos DB 功能

- 支持各种 API，其中包括 SQL（核心）API、Cassandra API、MongoDB API、Gremlin API 和表 API。
- 不管用户位于何处，统包式全局分发都会复制数据。 用户可以与距离最近的数据副本交互。
- 无需架构或索引管理，因为数据库引擎与架构完全无关。
- 无处不在、分布广泛，因为 Azure Cosmos DB 在全球范围的所有 Azure 区域中推出，在公有云中包括 54 个以上的区域。
- 精确定义的多个一致性选择，因为 Azure Cosmos DB 的多主数据库复制协议精心设计为提供五个明确定义的一致性选择。 这五个选择为“强”、“有限过期”、“会话”、“一致前缀”和“最终”。
- 99.999% 可用于读取和写入。
- 以编程方式（或通过 Azure 门户）调用 Azure Cosmos DB 帐户的区域故障转移，以确保应用程序设计为可承受区域性灾难。
- 保证第 99 个百分位为低延迟（全球范围内）。

### <a name="azure-cosmos-db-references"></a>Azure Cosmos DB 参考

- [Azure 门户](https://portal.azure.com) 
- [Azure Cosmos DB 文档](/azure/cosmos-db/introduction)

## <a name="azure-sql-database"></a>Azure SQL Database

 [Azure SQL 数据库](https://azure.microsoft.com/services/sql-database/)是通用关系数据库管理服务。 可以使用该数据库为 Azure 云中的应用程序和解决方案创建高度可用且高性能的数据存储层。

### <a name="azure-sql-database-features"></a>Azure SQL 数据库功能

- 弹性数据库模型和工具：使用弹性数据库，开发人员可以在一组数据库中集中资源以进行缩放。 若要管理这些资源，请将脚本作为作业提交。 然后，SQL 数据库会跨数据库执行脚本。
- 高性能：高吞吐量应用程序可以利用最新版本。 它提供了 25% 以上的高级数据库能力。
- 备份、复制和高可用性：数据库级别的内置复制和支持 Microsoft 的 SLA 提供应用程序的连续性，并防范灾难性事件。 活动异地复制可让你配置故障转移和自助服务还原，以便完全控制“事故恢复”。 可从最多 35 天的数据备份还原数据。
- 几乎无需维护：自动软件是服务的一部分。 内置系统副本有助于提供固有的数据保护、数据库运行时间和系统稳定性。 系统副本会自动转移到新计算机。 由于旧计算机发生故障，因此会动态预配新计算机。
- **安全性：** Azure SQL 数据库提供一系列安全功能，以满足组织或行业要求的符合性策略：

- 审核使开发人员能够执行与符合性相关的任务并了解相关活动。
- 开发人员和 IT 人员可以在数据库级别实施策略，以帮助使用 Azure SQL 数据库的行级安全性、动态数据掩码和透明数据加密来限制对敏感数据的访问权限。
- Azure SQL 数据库是由作为重要 Azure 符合性认证和批准范围（例如，HIPAA BAA、ISO/IEC 27001:2005、FedRAMP 和欧盟模式条款）的一部分的重要云审核员验证。

### <a name="azure-sql-database-references"></a>Azure SQL 数据库参考

- [Azure 门户](https://portal.azure.com) 
- [Azure SQL 数据库文档](/azure/sql-database/)
