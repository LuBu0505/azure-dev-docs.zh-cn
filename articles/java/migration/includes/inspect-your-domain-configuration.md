---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 750a8cdd34ead8390a0c4c2cb028f4624bd256a9
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82988920"
---
### <a name="inspect-your-domain-configuration"></a>检查域配置

WebLogic Server 中的主要配置单元是域。 因此，*config.xml* 文件包含了大量的配置，进行迁移时必须仔细考虑这些配置。 此文件包含对存储在子目录中的其他 XML 文件的引用。 Oracle 建议你正常情况下使用**管理控制台**来配置 WebLogic Server 的可管理对象和服务，并允许 WebLogic Server 保留 *config.xml* 文件。 有关详细信息，请参阅 [Domain Configuration Files](https://docs.oracle.com/en/middleware/fusion-middleware/weblogic-server/12.2.1.4/domcf/config_files.html)（域配置文件）。

#### <a name="inside-your-application"></a>在应用程序中

检查 *WEB-INF/weblogic.xml* 文件和/或 *WEB-INF/web.xml* 文件。
