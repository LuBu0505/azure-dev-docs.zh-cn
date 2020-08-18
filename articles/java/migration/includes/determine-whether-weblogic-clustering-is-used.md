---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: f50115be65be8e746527b3d4ee833a738109ddbc
ms.sourcegitcommit: b923aee828cd4b309ef92fe1f8d8b3092b2ffc5a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2020
ms.locfileid: "88052246"
---
### <a name="determine-whether-weblogic-clustering-is-used"></a>确定是否使用了 WebLogic 聚类分析

最可能的情况是，你已将应用程序部署到多个 WebLogic 服务器上，以实现高可用性。 可以直接将这些群集从本地安装迁移到 Azure 虚拟机中运行的 WebLogic。 有关详细信息，请参阅 Oracle 文档中的 [Domain Configuration Files](https://docs.oracle.com/middleware/12213/wls/DOMCF/config_files.htm#DOMCF127)（域配置文件）。

### <a name="account-for-load-balancing-requirements"></a>用于满足负载均衡要求的帐户

负载均衡是将 Oracle WebLogic Server 群集迁移到 Azure 时必不可少的一部分。  最简单的解决方案是使用 Oracle WebLogic Server 群集的 Azure 市场套餐中提供的 [Azure 应用程序网关](/azure/application-gateway/overview) 的内置支持。  有关本主题的教程，请参阅[教程：使用 Azure 应用程序网关作为负载均衡器将 WebLogic Server 群集迁移到 Azure](../migrate-weblogic-with-app-gateway.md)。

要查看摘要了解 Azure 应用程序网关的功能与其他 Azure 负载均衡解决方案的比较情况，请参阅 [Azure 中的负载均衡选项概述](/azure/architecture/guide/technology-choices/load-balancing-overview)。
