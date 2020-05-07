---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 3427cc445333c9851a760a607c402e689c7b6ba4
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673153"
---
### <a name="inventory-all-certificates"></a>清点所有证书

记录用于公共 SSL 终结点的所有证书。 可以通过运行以下命令来查看生产服务器上的所有证书：

```bash
keytool -list -v -keystore <path to keystore>
```
