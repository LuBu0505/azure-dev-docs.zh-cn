---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: cc634f89170f4db6f961da91e3eb3abe7393539d
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673593"
---
### <a name="validate-that-the-supported-java-version-works-correctly"></a>验证支持的 Java 版本是否正常运行

WebLogic 到 Azure 的所有迁移路径都需要一个特定的 Java 版本，该版本因路径而异。 需验证应用程序能否使用该支持的版本正确运行。

若要获取当前的版本，请登录到生产服务器并运行以下命令：

```bash
java -version
```

> [!NOTE]
> 迁移到 Azure 虚拟机上的 WebLogic 时，特定 Java 版本的要求取决于虚拟机上预安装的 Java。
