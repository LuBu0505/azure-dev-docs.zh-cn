---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 33146c309d8fdaf21dc218c11054460cfc3dc8f4
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673583"
---
### <a name="domain-configuration"></a>域配置

WebLogic Server 中的主要配置单元是域。 因此，*config.xml* 文件包含了大量的配置，进行迁移时必须仔细考虑这些配置。 此文件包含对存储在子目录中的其他 XML 文件的引用。 Oracle 建议你正常情况下使用**管理控制台**来配置 WebLogic Server 的可管理对象和服务，并允许 WebLogic Server 保留 *config.xml* 文件。 有关详细信息，请参阅 [Domain Configuration Files](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html)（域配置文件）。

#### <a name="inside-your-application"></a>在应用程序中

检查 *WEB-INF/weblogic.xml* 文件和/或 *WEB-INF/web.xml* 文件。
