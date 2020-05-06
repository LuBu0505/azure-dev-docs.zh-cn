---
author: sptramer
ms.author: sttramer
manager: carmonm
ms.date: 05/28/2019
ms.topic: include
ms.prod: azure
ms.technology: azure-cli
ms.openlocfilehash: 676f33377a4e7122941bc789c51465b7f34aa1d3
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030879"
---
如果由于代理而无法连接到外部资源，请确保已在 shell 中正确设置了 `HTTP_PROXY` 和 `HTTPS_PROXY` 变量。 你需要与系统管理员联系以了解要对这些代理使用哪些主机和端口。

许多 Linux 程序（包括那些在安装过程中使用的程序）也会采用这些值。 若要设置这些值，请执行以下操作：

```bash
# No auth
export HTTP_PROXY=http://[proxy]:[port]
export HTTPS_PROXY=https://[proxy]:[port]

# Basic auth
export HTTP_PROXY=http://[username]:[password]@[proxy]:[port]
export HTTPS_PROXY=https://[username]:[password]@[proxy]:[port]
```

> [!IMPORTANT]
> 如果你位于代理后面，则必须设置这些 shell 变量以通过 CLI 连接到 Azure 服务。
> 如果不使用基本身份验证，建议将这些变量导出到 `.bashrc` 文件中。
> 请始终遵循企业的安全策略和系统管理员的要求。
