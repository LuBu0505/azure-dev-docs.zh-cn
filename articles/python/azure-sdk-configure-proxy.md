---
title: 使用 Azure 库时配置代理
description: 使用 HTTP[S]_PROXY 环境变量为整个脚本或应用定义代理，或者对客户端构造函数或操作方法使用可选的命名参数。
ms.date: 06/16/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 86eaa5b8dc0ddfb529643a5c015938344582e62b
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87982699"
---
# <a name="how-to-configure-proxies-for-the-azure-libraries"></a>如何为 Azure 库配置代理

代理服务器 URL 的格式为 `http[s]://[username:password@]<ip_address_or_domain>:<port>/`，其中 username:password 组合是可选的。

然后，可以使用环境变量来全局配置代理，也可以通过将名为 `proxies` 的参数传递给单个客户端构造函数或操作方法来指定代理。

## <a name="global-configuration"></a>全局配置

若要为脚本或应用全局配置代理，请使用服务器 URL 定义 `HTTP_PROXY` 或 `HTTPS_PROXY` 环境变量。 这些变量可用于任何版本的 Azure 库。

如果将参数 `use_env_settings=False` 传递给客户端对象构造函数或操作方法，则将忽略这些环境变量。

### <a name="from-python-code"></a>从 Python 代码

```python
import os
os.environ["HTTP_PROXY"] = "http://10.10.1.10:1180"

# Alternate URL and variable forms:
# os.environ["HTTP_PROXY"] = "http://username:password@10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://10.10.1.10:1180"
# os.environ["HTTPS_PROXY"] = "http://username:password@10.10.1.10:1180"
```

### <a name="from-the-cli"></a>从 CLI

# <a name="cmd"></a>[cmd](#tab/cmd)

```cmd
rem Non-authenticated HTTP server:
set HTTP_PROXY=http://10.10.1.10:1180

rem Authenticated HTTP server:
set HTTP_PROXY=http://username:password@10.10.1.10:1180

rem Non-authenticated HTTPS server:
set HTTPS_PROXY=http://10.10.1.10:1180

rem Authenticated HTTPS server:
set HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

# <a name="bash"></a>[bash](#tab/bash)

```bash
# Non-authenticated HTTP server:
HTTP_PROXY=http://10.10.1.10:1180

# Authenticated HTTP server:
HTTP_PROXY=http://username:password@10.10.1.10:1180

# Non-authenticated HTTPS server:
HTTPS_PROXY=http://10.10.1.10:1180

# Authenticated HTTPS server:
HTTPS_PROXY=http://username:password@10.10.1.10:1180
```

---

## <a name="per-client-or-per-method-configuration"></a>每客户端或每方法配置

若要为特定客户端对象或操作方法配置代理，请指定包含名为 `proxies` 的参数的代理服务器。

例如，文章[示例：使用 Azure 存储](azure-sdk-example-storage.md)中的以下代码使用 `BlobClient` 构造函数指定了具有用户凭据的 HTTPS 代理。 在此案例中，对象来自 azure.storage.blob 库，该库基于 azure.core。

```python
# Earlier code omitted for brevity

blob_client = BlobClient(storage_url, container_name="blob-container-01",
    blob_name="sample-blob.txt", credential=credential,
    proxies={ "https": "https://username:password@10.10.1.10:1180" }
)

# Other forms that the proxy URL might take:
# proxies={ "http": "http://10.10.1.10:1180" }
# proxies={ "http": "http://username:password@10.10.1.10:1180" }
# proxies={ "https": "https://10.10.1.10:1180" }
```
