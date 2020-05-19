---
author: yevster
ms.author: yebronsh
ms.date: 5/4/2020
ms.openlocfilehash: 7b2ad87dfe2e2c7358737ff02ec52b97b1332ae7
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990129"
---
### <a name="inspect-the-deployment-architecture"></a>检查部署体系结构

#### <a name="document-hardware-requirements-for-each-service"></a>记录每个服务的硬件要求

对于每个 Spring Cloud 服务（不包括配置服务器、注册表或网关），记录以下信息：

* 正在运行的实例数。
* 为每个实例分配的 CPU 数量。
* 为每个实例分配的 RAM 量。

#### <a name="document-geo-replicationdistribution"></a>记录异地复制/分发

确定 Spring Cloud 应用程序当前是否分布在多个区域或数据中心。 记录要迁移的应用程序的运行时间要求/SLA。

#### <a name="identify-clients-that-bypass-the-service-registry"></a>确定绕过服务注册表的客户端

确定调用任何要迁移的服务的客户端应用程序，而无需使用 Spring Cloud 服务注册表。 迁移后，将无法再进行此类调用。 更新此类客户端，以便在迁移前使用 [Spring Cloud OpenFeign](https://spring.io/projects/spring-cloud-openfeign)。
