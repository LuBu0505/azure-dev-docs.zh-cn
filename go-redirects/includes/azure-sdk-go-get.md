---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 09/05/2018
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: a51c8667c74a2611ae8769aa42fd1a94f9253bc8
ms.sourcegitcommit: 9cd460ee16b637e701aa30078932878c0d0a7945
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2019
ms.locfileid: "70180804"
---
[Azure SDK for Go](https://github.com/Azure/azure-sdk-for-go) 与 Go 1.8 版和更高版本兼容。 对于使用 [Azure Stack 配置文件](/azure/azure-stack/user/azure-stack-version-profiles-go)的环境，最低需要 Go 版本 1.9。
如果需要安装 Go，请按照 [Go 安装说明](https://golang.org/doc/install)进行操作。

可以通过 `go get` 下载 Azure SDK for Go 及其依赖项。

```bash
go get -u -d github.com/Azure/azure-sdk-for-go/...
```

> [!WARNING]
> 请务必在 URL 中将 `Azure` 大写。 否则，在使用该 SDK 时，可能导致大小写相关的导入问题。 此外，还需要在导入语句中将 `Azure` 大写。
