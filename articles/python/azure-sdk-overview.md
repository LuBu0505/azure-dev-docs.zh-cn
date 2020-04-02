---
title: 用于 Python 的 Azure SDK
description: 概述 Azure SDK for Python 的特性和功能，这些特性和功能可提高开发人员预配、使用和管理 Azure 资源时的工作效率。
ms.date: 03/17/2020
ms.topic: conceptual
ms.openlocfilehash: a7fa744884972c6e25ac85b69c6b8317d7cb5190
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441672"
---
# <a name="azure-sdk-for-python"></a>用于 Python 的 Azure SDK

开源 Azure SDK for Python 简化了通过 Python 应用程序代码预配、使用和管理 Azure 资源的过程。 此 SDK 支持 Python 2.7 和 Python 3.5.3 或更高版本。

此 SDK 由单独的组件库组成，每个组件库都使用 `pip install <library>` 进行安装。 如需更详细的说明，请参阅[安装 SDK](azure-sdk-install.md)。 [Azure SDK for Python 文档](https://azure.github.io/azure-sdk-for-python/)提供了这些库的名称。

也可按照 [Azure SDK for Python 入门](azure-sdk-get-started.yml)演练的步骤操作，亲自体验这些库。

> [!TIP]
> 若要了解 SDK 中的变更，请参阅 [SDK 发行说明](https://azure.github.io/azure-sdk/)。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="management-and-client-libraries"></a>管理和客户端库

Azure SDK for Python 包含两种类型的库：

- 管理  库：有助于预配和管理 Azure 资源，就像你可以通过 [Azure 门户](https://portal.azure.com)或命令行工具（例如 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 和 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/)）这样做一样。 管理库最常用在配置和部署脚本中。 （实际上，Azure CLI 本身使用 Python 管理库编写。）

- 客户端  库：为应用程序代码提供了一种使用自然的 Python 惯用语与已预配的服务交互的方法。 实际上，客户端库使用 Azure 的 REST API，但使用 SDK 则无需担心 REST 细节。

以下部分提供了每个类别的更多详细信息和示例。

## <a name="manage-azure-resources-with-management-libraries"></a>使用管理库管理 Azure 资源

Azure SDK for Python 的管理库（每个库命名为 `azure-mgmt-<service name>`）有助于创建和预配 Azure 资源，以及以其他方式管理这些资源。

例如，假设你要创建 SQL Server 实例。 首先，安装适当的管理库：

```bash
pip install azure-mgmt-sql
```

在 Python 代码中，导入库：

```python
from azure.mgmt.sql import SqlManagementClient
```

接下来，使用凭据（请参阅[使用 SDK 进行身份验证](azure-sdk-authenticate.md)）和 Azure 订阅 ID 创建管理客户端对象：

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

最后，使用相应的资源组名称、服务器名称、位置和管理员凭据通过该管理客户端对象创建资源：

```python
server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

若要详细了解如何使用每个管理库，请参阅 *README.md* 或 *README.rst* 文件，该文件位于 SDK 的 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)中找到其他代码片段。

## <a name="connect-and-use-azure-services-with-client-libraries"></a>通过客户端库连接并使用 Azure 服务

 可以通过 Azure SDK 的客户端库连接到现有 Azure 资源并在应用中使用它们，用途包括：上传文件、访问表数据，或者使用各种 Azure 认知服务。 有了此 SDK，就可以通过熟悉的 Python 编程范例而不是服务的通用 REST API 来使用这些资源。 （没有 REST API 的服务不由客户端库表示。）

例如，假设你已将 Web 应用部署到 Azure 应用服务，但需添加将文件上传到 Azure 存储帐户的功能。 第一步是安装适当的库：

```bash
pip install azure-storage-blob
```

接下来，将该库导入代码中：

```python
from azure.storage.blob import BlobClient
```

最后，使用该库的 API 连接到数据并将其上传。 在以下示例中，连接字符串和容器名称已在存储帐户中预配。 blob 名称是分配给已上传数据的名称：

```python
blob = BlobClient.from_connection_string("my_connection_string", container="mycontainer", blob="my_blob")

with open("./SampleSource.txt", "rb") as data:
    blob.upload_blob(data)
```

若要详细了解如何使用每个管理库，请参阅 *README.md* 或 *README.rst* 文件，该文件位于 SDK 的 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)中找到其他代码片段。

### <a name="the-azure-core-library"></a>Azure Core 库

我们目前正在更新 Azure SDK for Python 客户端库，以共享常见的云模式，如身份验证协议、日志记录、跟踪、传输协议、缓冲响应和重试。 该共享功能包含在 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 库中。 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/#python-packages)页上列出了当前与 Core 库兼容的库。

若要详细了解 Azure Core 库及其指南，请参阅 [Python Guidelines:Introduction](https://azure.github.io/azure-sdk/python_introduction.html)（Python 指南：简介）。

## <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

- 访问 [Azure SDK for Python 文档](https://aka.ms/python-docs)
- 在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问
- 在 [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) 上提出针对此 SDK 的问题
- 在 Twitter 上提及 [@AzureSDK](https://twitter.com/AzureSdk/)
