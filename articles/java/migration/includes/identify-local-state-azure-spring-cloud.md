---
author: yevster
ms.author: yebronsh
ms.date: 5/28/2020
ms.openlocfilehash: 70f54f911d97febf30b78888e6922e2d4ac3eb54
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84512616"
---
#### <a name="identify-local-state"></a>标识本地状态

在 PaaS 环境上，无法保证应用程序在任意给定时间都刚好运行一次。 即使将应用程序配置为在单个实例中运行，在以下情况下也可以创建重复的实例：

* 由于故障或系统更新，必须将应用程序重新定位到物理主机。
* 正在更新应用程序。

在上述任一情况下，原始实例将始终保持运行状态，直到新实例完成启动。 对于你的应用程序，这会产生以下潜在的重大影响：

* 无法保证[单一实例](https://en.wikipedia.org/wiki/Singleton_pattern)真正是单一的。
* 任何未持久保留到外部存储中的数据丢失都可能远远早于保留在单个物理服务器或 VM 上的数据丢失。

迁移到 Azure Spring Cloud 之前，请确保你的代码未包含不得丢失或重复的本地状态。 如果存在本地状态，请更改代码，使其在应用程序外部存储该状态。 云就绪应用程序通常将应用程序状态存储在以下位置：

* [用于 Redis 的 Azure 缓存](/azure/azure-cache-for-redis/cache-java-get-started)
* [Azure CosmosDB](/azure/cosmos-db/create-sql-api-java)
* 其他外部数据库，如 [Azure SQL](/azure/azure-sql/azure-sql-iaas-vs-paas-what-is-overview)、[Azure DB for MySQL](/azure/mysql/overview) 或 [Azure DB for PostgreSQL](/azure/postgresql/overview)。
