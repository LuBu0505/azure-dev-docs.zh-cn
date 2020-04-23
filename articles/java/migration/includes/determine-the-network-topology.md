---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 9403614c7ae2990a35fcbcce6d2e104f87724da5
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673523"
---
### <a name="determine-the-network-topology"></a>确定网络拓扑

可以从当前的市场套餐集着手进行迁移。 如果套餐不包含需要迁移的体系结构方面的内容，则需捕获现有部署的网络拓扑，并在 Azure 中重新生成该拓扑，即使在将基本套餐与解决方案模板之一结合在一起使用后也是如此。

这是一个非常广泛的主题，但以下参考可为迁移工作提供某些指导：

* 此参考枚举将网络拓扑迁移到 Azure 时的相关高级主题：[快速跟踪部署指南](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/deploying.html#GUID-E0BE4A3E-44CD-4C95-9540-7A850BF02F6A)。
* 此参考介绍有关群集的重要问题，这会对网络拓扑产生影响：[WebLogic Server Clustering](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/clustering.html#GUID-E39A18C2-B990-485F-BFB1-0549250FABFE)（WebLogic Server 聚类分析）。
* 由于数据源是 WebLogic 系统中的独立服务器，因此必须将其视为网络拓扑分析的一部分。 [WebLogic Server Data Sources](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jdbc.html#GUID-9FD5F552-B2E4-4FEC-8C10-503A08764B52)（WebLogic Server 数据源）。
* 消息传送源也是独立的服务器。 [WebLogic Server Messaging](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/intro/jms.html#GUID-3B5F647D-E001-413B-AC6A-1E103BDBA93F)（WebLogic Server 消息传送）
* 负载均衡是一项基本要求。 此参考介绍负载均衡的 WebLogic Server 端：[Load Balancing in a Cluster](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/clust/load_balancing.html#GUID-B8F6DE4B-1AAC-428B-878B-BFDCE161C054)（在群集中进行负载均衡）。
