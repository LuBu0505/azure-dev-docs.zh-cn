---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a771d0689e83e0b61bf9b8c31e0cbf36949fdc20
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830755"
---
### <a name="inventory-all-secrets"></a>清点所有机密

在出现“配置即服务”技术（如 Azure Key Vault）之前，没有有关“机密”的明确定义的概念。 你只能使用一组不同的配置设置，这些设置可以有效地充当我们现在所称的“机密”。 在应用服务器（如 WebLogic Server）中，这些机密位于许多不同的配置文件和配置存储中。 检查生产服务器上的所有属性和配置文件中是否有机密和密码。 请确保在 WAR 中检查 *weblogic.xml*。 还可以在应用程序中查找包含密码或凭据的配置文件。 有关详细信息，请参阅 [Azure Key Vault 基本概念](/azure/key-vault/basic-concepts)。
