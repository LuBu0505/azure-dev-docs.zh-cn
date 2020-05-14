---
author: yevster
ms.author: yebronsh
ms.date: 1/24/2020
ms.openlocfilehash: 975c6ce1011f76a34f33ec6093f70f281eefa6c3
ms.sourcegitcommit: 226ebca0d0e3b918928f58a3a7127be49e4aca87
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82990209"
---
#### <a name="inventory-certificates"></a>清点证书

记录用于公共 SSL 终结点或与后端数据库以及其他系统通信的所有证书。 可以通过运行以下命令来查看生产服务器上的所有证书：

```bash
keytool -list -v -keystore <path to keystore>
```
