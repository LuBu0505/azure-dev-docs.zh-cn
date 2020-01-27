---
author: yevster
ms.author: yebronsh
ms.topic: include
ms.date: 1/20/2020
ms.openlocfilehash: 4228d1db44ac16da1c05fe97d20a3491cc9c5f51
ms.sourcegitcommit: 3585b1b5148e0f8eb950037345bafe6a4f6be854
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/21/2020
ms.locfileid: "76288566"
---
### <a name="inventory-secrets"></a>清点机密

#### <a name="passwords-and-secure-strings"></a>密码和安全字符串

检查生产服务器上的所有属性和配置文件中是否有机密字符串和密码。 务必检查 $CATALINA_BASE/conf 中的 *server.xml* 和 *context.xml*。 还可以在应用程序中查找包含密码或凭据的配置文件。 其中可能包括 *META-INF/context.xml*。对于 Spring Boot 应用程序，可能包括 *application.properties* 或 *application.yml* 文件。

#### <a name="certificates"></a>证书

记录用于公共 SSL 终结点的所有证书。 可以通过运行以下命令来查看生产服务器上的所有证书：

```bash
keytool -list -v -keystore <path to keystore>
```
