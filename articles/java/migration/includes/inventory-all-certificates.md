---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673153"
---
### <a name="inventory-all-certificates"></a>清点所有证书

记录用于公共 SSL 终结点的所有证书。 可以通过运行以下命令来查看生产服务器上的所有证书：

```bash
keytool -list -v -keystore <path to keystore>
```
