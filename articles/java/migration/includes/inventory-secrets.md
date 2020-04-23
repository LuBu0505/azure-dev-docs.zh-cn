---
author: yevster
ms.author: yebronsh
ms.date: 1/20/2020
ms.openlocfilehash: e37d0337361ce742ce7f7994ab634e63bc4b2ead
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81671643"
---
### <a name="inventory-secrets"></a>清点机密

#### <a name="passwords-and-secure-strings"></a>密码和安全字符串

检查生产服务器上的所有属性和配置文件中是否有机密字符串和密码。 务必检查 *$CATALINA_BASE/conf* 中的 *server.xml* 和 *context.xml*。 还可以在应用程序中查找包含密码或凭据的配置文件。 其中可能包括 *META-INF/context.xml*。对于 Spring Boot 应用程序，可能包括 *application.properties* 或 *application.yml* 文件。
