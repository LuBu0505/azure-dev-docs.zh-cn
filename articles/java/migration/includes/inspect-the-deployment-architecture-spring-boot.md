---
author: mnriem
ms.author: manriem
ms.date: 5/8/2020
ms.openlocfilehash: 82cf9553654e1efc884542ecdd52d294688291df
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738117"
---
### <a name="inspect-the-deployment-architecture"></a>检查部署体系结构

#### <a name="document-hardware-requirements-for-each-service"></a>记录每个服务的硬件要求

记录 Spring Boot 应用程序的以下信息：

* 正在运行的实例数。
* 为每个实例分配的 CPU 数量。
* 为每个实例分配的 RAM 量。

#### <a name="document-geo-replicationdistribution"></a>记录异地复制/分发

确定 Spring Boot 应用程序实例当前是否分布在多个区域或数据中心。 记录要迁移的应用程序的运行时间要求/SLA。
