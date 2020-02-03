---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 3cf4edcf12d7fe72c0fa5a0216d1a8c97a2f2e59
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76825861"
---
### <a name="inventory-certificates"></a>清点证书

记录用于公共 SSL 终结点或与后端数据库以及其他系统通信的所有证书。 可以通过运行以下命令来查看生产服务器上的所有证书：

```bash
keytool -list -v -keystore <path to keystore>
```
