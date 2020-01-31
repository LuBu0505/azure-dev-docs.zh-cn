---
ms.date: 09/05/2018
ms.technology: azure-cli
ms.openlocfilehash: af03f2efbb74e55dfcd14b6a2ac894a74eba321f
ms.sourcegitcommit: 4cf22356d6d4817421b551bd53fcba76bdb44cc1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76871871"
---
[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) 与 Go 1.8 版和更高版本兼容。 对于使用 [Azure Stack 配置文件](/azure/azure-stack/user/azure-stack-version-profiles-go)的环境，最低需要 Go 版本 1.9。
如果需要安装 Go，请按照 [Go 安装说明](https://golang.org/doc/install)进行操作。

可以通过 `go get` 下载 Azure SDK for Go 及其依赖项。

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> 请务必在 URL 中将 `Azure` 大写。 否则，在使用该 SDK 时，可能导致大小写相关的导入问题。 此外，还需要在导入语句中将 `Azure` 大写。
