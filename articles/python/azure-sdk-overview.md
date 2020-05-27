---
title: 使用 Azure SDK for Python
description: 概述 Azure SDK for Python 的特性和功能，这些特性和功能可提高开发人员预配、使用和管理 Azure 资源时的工作效率。
ms.date: 05/13/2020
ms.topic: conceptual
ms.openlocfilehash: 856487b06e7a84b0659126d8959ce45b5309a103
ms.sourcegitcommit: fbbc341a0b9e17da305bd877027b779f5b0694cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/19/2020
ms.locfileid: "83631548"
---
# <a name="use-the-azure-sdk-for-python"></a>使用 Azure SDK for Python

开源 Azure SDK for Python 简化了通过 Python 应用程序代码预配、管理和使用 Azure 资源的过程。

## <a name="the-details-you-really-want-to-know"></a>你真正想要了解的详细信息

- SDK 支持 Python 2.7 和 Python 3.5.3 或更高版本，并使用 PyPy 5.4 以上版本进行了测试。

- SDK 由 180 多个与特定 Azure 服务相关的 Python 库组成。

- 使用[发布列表](https://azure.github.io/azure-sdk/releases/latest/all/python.html)上的库名称，用 `pip install <library_name>` 安装所需的库。 有关更多详细信息，请参阅[安装 Azure SDK 库](azure-sdk-install.md)。

- 有不同的“管理”库和“客户端”库（有时称为“管理平面”库和“数据平面”库）。 每个集都有不同的用途，由不同类型的代码使用。 有关详细信息，请参阅本文后面的以下部分：
  - [使用管理库预配和管理 Azure 资源](#provision-and-manage-azure-resources-with-management-libraries)
  - [通过客户端库连接并使用 Azure 资源](#connect-to-and-use-azure-resources-with-client-libraries)

- SDK 的文档可在 [Azure SDK for Python 参考](/python/api/overview/azure/?view=azure-python)中找到，该参考按 Azure 服务进行组织，也可在 [Python API 浏览器](/python/api/?view=azure-python)中找到，该参考按包名称进行组织。 目前，经常需要单击多个层，才能找到你想要了解的类和方法。 对此造成的不便，我们深表歉意。 我们正在努力改进！

- 若要亲自尝试这些库，请先从[示例：创建资源组](azure-sdk-example-resource-group.md)开始，然后再查看[示例：使用 Azure 存储](azure-sdk-example-storage.md)。

### <a name="non-essential-but-still-interesting-details"></a>不重要但仍很有趣的详细信息

- 由于 Azure CLI 是使用 SDK 管理库用 Python 编写的，因此使用 Azure CLI 命令执行的任何操作也可以通过 Python 脚本来执行。 也就是说，CLI 命令提供了许多有用的功能，如同时执行多项任务，自动处理异步操作、格式化输出（如连接字符串）等。 因此，使用 CLI（或其等效的 Azure PowerShell）进行自动预配和管理脚本比编写等效的 Python 代码要方便得多，除非你希望对过程进行更为严格的控制。

- Azure SDK for Python 是底层 Azure REST API 之上的 Python 层，允许通过熟悉的 Python 模式使用这些 API。 不过，在需要时，始终可以直接通过 Python 代码使用 REST API。

- 可在 [https://github.com/Azure/azure-sdk-for-python](https://github.com/Azure/azure-sdk-for-python) 找到 SDK 的源代码。 作为一个开源项目，我们欢迎你的贡献！

- 尽管可将此 SDK 与我们未测试的解释器（例如 IronPython 和 Jython）配合使用，但可能会遇到孤立的问题和不兼容问题。

- SDK 文档的源存储库位于 [https://github.com/MicrosoftDocs/azure-docs-sdk-python/](https://github.com/MicrosoftDocs/azure-docs-sdk-python/)。

- 我们目前正在更新 Azure SDK for Python 库，以共享常见的云模式，如身份验证协议、日志记录、跟踪、传输协议、缓冲响应和重试。

  - 该共享功能包含在 [azure-core](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/core/azure-core) 库中。

  - [Azure SDK for Python 最新版本](https://azure.github.io/azure-sdk/releases/latest/#python)上列出了当前与 Core 库兼容的库。 这些库（主要是客户机库）有时称为“跟踪 2”。

  - 尚未更新的管理库有时称为“跟踪 1”。

- 若要详细了解我们应用于 SDK 的指南，请参阅 [Python 指南：Introduction](https://azure.github.io/azure-sdk/python_introduction.html)（Python 指南：简介）。

## <a name="provision-and-manage-azure-resources-with-management-libraries"></a>使用管理库预配和管理 Azure 资源

SDK 的管理（或“管理平面”）库（其名称都以 `azure-mgmt-` 开头）可帮助你通过 Python 脚本创建、预配和管理 Azure 资源。 所有 Azure 服务都有相应的管理库。

借助管理库，可以编写配置和部署脚本，以执行可通过 [Azure 门户](https://portal.azure.com)或 [Azure CLI](/cli/azure/install-azure-cli) 执行的相同任务。 （如前文所述，Azure CLI 是用 Python 编写的，并使用管理库来实现其各种命令。）

若要详细了解如何使用每个管理库，请参阅 *README.md* 或 *README.rst* 文件，该文件位于 SDK 的 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)中找到其他代码片段。

## <a name="connect-to-and-use-azure-resources-with-client-libraries"></a>通过客户端库连接并使用 Azure 资源

SDK 的客户端（或“数据平面”）库可帮助你编写 Python 应用程序代码，以与已预配的服务进行交互。 SDK 只为支持客户端 API 的服务提供客户端库。

若要详细了解如何使用每个客户端库，请参阅 README.md 或 README.rst 文件，该文件位于 SDK 的 [GitHub 存储库](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk)的库项目文件夹中。 也可在[参考文档](/python/api?view=azure-python)和 [Azure 示例](https://docs.microsoft.com/samples/browse/?languages=python&products=azure)中找到其他代码片段。

## <a name="inline-json-pattern-for-object-arguments"></a>对象参数的内联 JSON 模式

Azure SDK 中的许多操作都支持常用模式，使你能够将对象参数表示为离散对象或内联 JSON。

例如，假设你有一个 [`ResourceManagementClient`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.resourcemanagementclient?view=azure-python) 对象，并通过该对象的 [`create_or_update`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#create-or-update-resource-group-name--parameters--custom-headers-none--raw-false----operation-config-)) 方法创建了资源组。 此方法的第二个参数的类型为 [`ResourceGroup`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.models.resourcegroup?view=azure-python)。

若要调用 `create_or_update`，可以使用其所需的参数（在本例中为 `location`）创建 `ResourceGroup` 的离散实例：

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    ResourceGroup(location="centralus")
)
```

或者，可以将相同参数传递为内联 JSON：

```python
rg_result = resource_client.resource_groups.create_or_update(
    "PythonSDKExample-rg",
    {
      "location": "centralus"
    }
)
```

使用 JSON 时，SDK 会自动将内联 JSON 转换为相关参数的相应对象类型。

对象也可以具有嵌套对象参数，在这种情况下，也可以使用嵌套 JSON。

例如，假设你有一个 [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) 对象的实例，并调用其 [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) 方法。 在这种情况下，第三个参数的类型为 [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python)，其本身包含 [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python) 类型的参数。 而 `VaultProperties` 包含类型为 [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) 和 [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python) 的对象参数。 `Sku` 包含 [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) 对象，且每个 `AccessPolicyEntry` 均包含 [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python) 对象。

若要使用嵌入对象调用 `create_or_update`，请使用如下所示的代码（假定已定义了 `tenant_id` 和 `object_id`）：

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    VaultCreateOrUpdateParameters(
        location="centralus",
        properties=VaultProperties(
            tenant_id=tenant_id,
            sku=Sku(name="standard"),
            access_policies=[
                AccessPolicyEntry(
                    tenant_id=tenant_id,
                    object_id=object_id,
                    permissions=Permissions(keys=['all'], secrets=['all'])
                )
            ]
        )
    )
)
```

使用内联 JSON 的相同调用如下所示：

```python
operation = keyvault_client.vaults.create_or_update(
    "PythonSDKExample-rg",
    "keyvault01",
    {
        'location': 'centralus',
        'properties': {
            'sku': {
                'name': 'standard'
            },
            'tenant_id': tenant_id,
            'access_policies': [{
                'object_id': object_id,
                'tenant_id': tenant_id,
                'permissions': {
                    'keys': ['all'],
                    'secrets': ['all']
                }
            }]
        }
    }
)
```

由于这两种形式是完全等效的，因此可以根据自己的喜好加以选择，甚至将它们混合使用。

如果 JSON 的格式不正确，通常会出现错误“DeserializationError：无法反序列化对象：类型，AttributeError：“str”对象没有属性“get”。 此错误的一个常见原因是，当 SDK 要求嵌套的 JSON 对象时，你为属性提供了单个字符串。 例如，在前面的示例中使用 `"sku": "standard"` 会生成此错误，因为 `sku` 参数是一个需要内联对象 JSON 的 `Sku` 对象，在本例中为 `{ "name": "standard"}`，它映射到所需的 `SkuName` 类型。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [安装 SDK 库 >>>](azure-sdk-install.md)

## <a name="get-help-and-connect-with-the-sdk-team"></a>获取帮助并与 SDK 团队联系

- 访问 [Azure SDK for Python 文档](https://aka.ms/python-docs)
- 在 [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) 社区中提问
- 在 [GitHub](https://github.com/Azure/azure-sdk-for-python/issues) 上提出针对此 SDK 的问题
- 在 Twitter 上提及 [@AzureSDK](https://twitter.com/AzureSdk/)
