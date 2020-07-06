---
title: 在用于 Python 的 Azure 库中配置日志记录
description: Azure 库使用标准 Python 日志记录模块，该模块在每个库或每个操作的基础上进行配置。
ms.date: 06/04/2020
ms.topic: conceptual
ms.openlocfilehash: b6456a1bf4bdbf8469483eb181b3df603263aa55
ms.sourcegitcommit: 39da5bec7ef824a34aa04514afc1141b75466547
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2020
ms.locfileid: "84741422"
---
# <a name="configure-logging-in-the-azure-libraries-for-python"></a>在用于 Python 的 Azure 库中配置日志记录

[基于 azure.core](azure-sdk-library-package-index.md#libraries-using-azurecore) 页面的用于 Python 的 Azure 库使用标准 Python [日志记录](https://docs.python.org/3/library/logging.html)库提供日志记录输出。

默认情况下，不启用记录器输出。 启用日志记录：

1. 获取所需库的日志记录对象，并设置日志记录级别。
1. 注册日志记录流的处理程序。
1. 通过将 `logging_enable=True` 参数传递到客户端对象构造函数或特定方法来启用日志记录。

一般来说，了解库中日志记录使用情况的最佳方法是在 [github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) 中浏览 SDK 源代码。 建议你在本地克隆此存储库，以便可以在需要时轻松搜索详细信息，如以下部分所示。

## <a name="set-logging-levels"></a>设置日志记录级别

```python
import logging

# ...

# Acquire the logger for a library (azure.storage.blob in this example)
logger = logging.getLogger('azure.storage.blob')

# Set the desired logging level
logger.setLevel(logging.DEBUG)
```

- 此示例获取 `azure.storage.blob` 库的记录器，然后将日志记录级别设置为 `logging.DEBUG`。

- 你可以随时调用 `logger.setLevel` 以更改不同代码片段的日志记录级别。

若要设置不同库的级别，请在 `logging.getLogger` 调用中使用该库的名称。 例如，azure eventhubs 库提供名为 `azure.eventhubs` 的记录器，azure-storage-queue 库提供名为 `azure.storage.queue` 的记录器，依此类推。 （SDK 源代码经常使用 `logging.getLogger(__name__)` 语句，该语句使用包含模块的名称获取记录器。）

你还可以使用更常见的命名空间。 例如，

```python
import logging

# Set the logging level for all azure-storage-* libraries
logger = logging.getLogger('azure.storage')
logger.setLevel(logging.INFO)

# Set the logging level for all azure-* libraries
logger = logging.getLogger('azure')
logger.setLevel(logging.ERROR)
```

可使用 `logger.isEnabledFor` 方法来检查是否已启用任何给定的日志记录级别：

```python
print(f"Logger enabled for ERROR={logger.isEnabledFor(logging.ERROR)}, " \
    f"WARNING={logger.isEnabledFor(logging.WARNING)}, " \
    f"INFO={logger.isEnabledFor(logging.INFO)}, " \
    f"DEBUG={logger.isEnabledFor(logging.DEBUG)}")
```

日志记录级别与[标准日志记录库级别](https://docs.python.org/3/library/logging.html#levels)相同。 下表描述了这些用于 Python 的 Azure 库中的日志记录级别的一般用法：

| 日志记录级别             | 典型用法 |
| ---                       | ---         |
| logging.ERROR             | 应用程序不太可能恢复的故障（如内存不足）。 |
| logging.WARNING（默认） | 函数无法执行其预期任务（但不是在函数可以恢复时，如重试 REST API 调用）。 函数通常会在引发异常时记录警告。 警告级别会自动启用错误级别。 |
| logging.INFO              | 函数正常运行，或者服务调用被取消。 信息事件通常包括请求、响应和标头。 信息级别会自动启用错误和警告级别。 |
| logging.DEBUG             | 通常用于疑难解答的详细信息。 调试是唯一包括敏感信息（如标头中的帐户密钥）的日志记录级别。 调试输出通常包含异常的堆栈跟踪。 调试级别会自动启用信息、警告和错误级别。 |
| logging.NOTSET            | 禁用所有日志记录。 |

### <a name="library-specific-logging-level-behavior"></a>特定于库的日志记录级别行为

每个级别的确切日志记录行为取决于相关的库。 某些库（如 azure.eventhub）会执行大量日志记录，而其他库执行的记录则非常少。

检查库的确切日志记录的最佳方式是在 Azure SDK for Python 源代码中搜索日志记录级别：

1. 在存储库文件夹中，导航到“sdk”文件夹，然后导航到所需特定服务的文件夹。

1. 在该文件夹中，搜索以下任何字符串：

    - `_LOGGER.error`
    - `_LOGGER.warning`
    - `_LOGGER.info`
    - `_LOGGER.debug`

## <a name="register-a-log-stream-handler"></a>注册日志流处理程序

若要捕获日志记录输出，必须在代码中注册至少一个日志流处理程序：

```python
import logging

# Direct logging output to stdout. Without adding a handler,
# no logging output is captured.
handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)
```

此示例注册的处理程序可将日志输出定向到 stdout。 可以使用 Python 文档中 [logging.handlers](https://docs.python.org/3/library/logging.handlers.html) 部分所述的其他类型的处理程序。

## <a name="enable-logging-for-a-client-object-or-operation"></a>为客户端对象或操作启用日志记录

即使设置了日志记录级别并注册了处理程序，仍必须指示 Azure 库为客户端对象构造函数或操作方法启用日志记录。

### <a name="enable-logging-for-a-client-object"></a>为客户端对象启用日志记录

```python
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

# endpoint is the Blob storage URL.
client = BlobClient(endpoint, DefaultAzureCredential(), logging_enable=True)
```

为客户端对象启用日志记录可为通过该对象调用的所有操作启用日志记录。

### <a name="enable-logging-for-an-operation"></a>为操作启用日志记录

```python
from azure.storage.blob import BlobClient
from azure.identity import DefaultAzureCredential

# Logging is not enabled on this client.
# endpoint is the Blob storage URL.
client = BlobClient(endpoint, DefaultAzureCredential())

# Enable logging for only this operation
client.create_container("container01", logging_enable=True)
```

## <a name="example-logging-output"></a>日志记录输出示例

以下代码显示在[示例：使用存储帐户](azure-sdk-example-storage-use.md)中，并且附带有启用调试日志记录的代码（为了简洁起见，已省略注释）：

```python
import os, sys, logging
from azure.identity import DefaultAzureCredential
from azure.storage.blob import BlobClient

logger = logging.getLogger('azure.storage.blob')
logger.setLevel(logging.DEBUG)

handler = logging.StreamHandler(stream=sys.stdout)
logger.addHandler(handler)

credential = DefaultAzureCredential()
storage_url = os.environ["AZURE_STORAGE_BLOB_URL"]

blob_client = BlobClient(storage_url, container_name="blob-container-01",
    blob_name="sample-blob.txt", credential=credential)

with open("./sample-source.txt", "rb") as data:
    blob_client.upload_blob(data, logging_enable=True)
```

日志记录输出如下所示：

<pre>
Request URL: 'https://pythonsdkstorage12345.blob.core.windows.net/blob-container-01/sample-blob.txt'
Request method: 'PUT'
Request headers:
    'Content-Type': 'application/octet-stream'
    'Content-Length': '79'
    'x-ms-version': '2019-07-07'
    'x-ms-blob-type': 'BlockBlob'
    'If-None-Match': '*'
    'x-ms-date': 'Mon, 01 Jun 2020 22:54:14 GMT'
    'x-ms-client-request-id': 'd081f88e-a45a-11ea-b9eb-0c5415dfd03a'
    'User-Agent': 'azsdk-python-storage-blob/12.3.1 Python/3.8.3 (Windows-10-10.0.18362-SP0)'
    'Authorization': '*****'
Request body:
b"Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.\r\n"
Response status: 201
Response headers:
    'Content-Length': '0'
    'Content-MD5': 'kvMIzjEi6O8EqTVnZJNakQ=='
    'Last-Modified': 'Mon, 01 Jun 2020 22:54:14 GMT'
    'ETag': '"0x8D8067EB52FF7BC"'
    'Server': 'Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0'
    'x-ms-request-id': '5df479b1-f01e-00d0-5b67-382916000000'
    'x-ms-client-request-id': 'd081f88e-a45a-11ea-b9eb-0c5415dfd03a'
    'x-ms-version': '2019-07-07'
    'x-ms-content-crc64': 'QmecNePSHnY='
    'x-ms-request-server-encrypted': 'true'
    'Date': 'Mon, 01 Jun 2020 22:54:14 GMT'
Response content:
</pre>
