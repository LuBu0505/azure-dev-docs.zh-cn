---
title: 用于 Python 的 Azure SDK
description: 概述 Azure SDK for Python 的特性和功能，这些特性和功能可提高开发人员使用 Azure 服务时的工作效率。
author: kraigb
ms.author: kraigb
manager: barbkess
ms.service: multiple
ms.date: 10/30/2019
ms.topic: conceptual
ms.devlang: python
ms.openlocfilehash: dda3044bcf80ff45d4a39c9186c6ad44a153218e
ms.sourcegitcommit: 54d34557bb83f52a215bf9020263cb9f9782b41d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2019
ms.locfileid: "74118013"
---
# <a name="azure-sdk-for-python"></a>用于 Python 的 Azure SDK

Azure SDK for Python 简化了通过 Python 应用程序代码使用和管理 Azure 资源的过程。 此 SDK 支持 Python 2.7 和 Python 3.5.3 或更高版本。

安装此 SDK 时，只需使用 `pip install <library>` 安装它的任何单个组件库即可。 可以在 [Azure SDK for Python 包索引](https://github.com/Azure/azure-sdk-for-python/blob/master/packages.md)上看到库列表

若要更详细地了解如何安装库并将其导入项目中，请参阅[安装 SDK](python-sdk-azure-install.md)。 然后阅读 [SDK 入门](python-sdk-azure-get-started.yml)，了解如何针对自己的 Azure 订阅设置身份验证并运行示例代码。

> [!TIP]
> 若要了解 SDK 中的变更，请参阅 [SDK 发行说明](https://azure.github.io/azure-sdk/)。

## <a name="connect-and-use-azure-services"></a>连接并使用 Azure 服务

 可以通过 SDK 中的许多客户端库连接到现有 Azure 资源并在应用中使用它们，用途包括：上传文件、访问表数据，或者使用各种 Azure 认知服务。 有了此 SDK，就可以通过熟悉的 Python 编程范例而不是服务的通用 REST API 来使用这些资源。

例如，假设要将 blob 上传到以前预配的 Azure 存储帐户。 第一步是安装适当的库：

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

若要详细了解如何使用每个特定的库，请参阅 *README.md* 或 *README.rst* 文件，该文件位于 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。 另请参阅提供的 [Azure 示例](https://docs.microsoft.com/samples/browse/?languages=python)。

也可在[参考文档](/python/api?view=azure-python)中找到其他代码片段

### <a name="the-azure-core-library"></a>Azure Core 库

我们目前正更新 Python 客户端库，以便共享核心功能，例如重试、日志记录、传输协议、身份验证协议等。该共享功能包含在 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 库中。 若要详细了解库及其指南，请参阅 [Python Guidelines:Introduction](https://azure.github.io/azure-sdk/python_introduction.html)（Python 指南：简介）。

目前兼容 Core 库的库如下所示：

- `azure-storage-blob`
- `azure-storage-queue`
- `azure-keyvault-keys`
- `azure-keyvault-secrets`

## <a name="manage-azure-resources"></a>管理 Azure 资源

Azure SDK for Python 还包括许多可以用来创建、预配并以其他方式管理 Azure 资源本身的库。 我们将这些库称为“管理库”  。 每个管理库的名称为 `azure-mgmt-<service name>`。 有了管理库，就可以编写 Python 代码来完成通过 [Azure 门户](https://portal.azure.com)或 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) 完成的那些任务。

例如，假设你要创建 SQL Server 实例。 首先，安装适当的管理库：

```bash
pip install azure-mgmt-sql
```

在 Python 代码中，导入库：

```python
from azure.mgmt.sql import SqlManagementClient

```

接下来，使用凭据和 Azure 订阅 ID 创建管理客户端对象：

```python
sql_client = SqlManagementClient(credentials, subscription_id)
```

最后，使用相应的资源组名称、服务器名称、位置和管理员凭据通过该客户端对象创建资源：

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

就像使用客户端库一样，若要详细了解如何使用每个管理库，可参阅 *README.md* 或 *README.rst* 文件，该文件位于 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。

也可在[参考文档](/python/api?view=azure-python)中找到其他代码片段。 

## <a name="get-help-and-give-feedback"></a>获取帮助和提供反馈

- 访问 [Azure SDK for Python 文档](https://aka.ms/python-docs)
- 在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问
- 在 [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) 上提出针对此 SDK 的问题
- Tweet [@AzureSDK](https://twitter.com/AzureSdk/)
