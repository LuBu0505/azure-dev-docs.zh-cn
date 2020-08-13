---
title: 用于 Python 的 Azure 库的使用模式
description: 用于 Python 的 Azure SDK 库的常见使用模式概述
ms.date: 06/09/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: cf44dc4458014972b6c6e16a28acab164d8f0f89
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983313"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>用于 Python 的 Azure 库的使用模式

用于 Python 的 Azure SDK 完全由 [Python SDK 包索引](azure-sdk-library-package-index.md)页上列出的多个独立库组成。

所有库共享某些常见特征和用法模式，例如，为对象参数安装和使用内联 JSON。

## <a name="library-installation"></a>库安装

若要安装特定的库包，只需使用 `pip install`：

```cmd
# Install the management library for Azure Storage
pip install azure-mgmt-storage
```

```cmd
# Install the client library for Azure Storage
pip install azure-storage-blob
```

`pip install` 在当前 Python 环境中检索库的最新版本。

还可以使用 `pip` 卸载库和安装特定版本，包括预览版。 有关详细信息，请参阅[如何安装用于 Python 的 Azure 库包](azure-sdk-install.md)。

## <a name="asynchronous-operations"></a>异步操作

通过客户端和管理客户端对象调用的许多操作（如 [`WebSiteManagementClient.web_apps.create_or_update`](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.operations.webappsoperations?view=azure-python#create-or-update-resource-group-name--name--site-envelope--custom-headers-none--raw-false--polling-true----operation-config-)）会返回一个类型 `AzureOperationPoller[<type>]` 的对象，其中 `<type>` 是特定于操作的。

[`AzureOperationPoller`](/python/api/msrestazure/msrestazure.azure_operation.azureoperationpoller?view=azure-python) 返回类型意味着操作是异步的。 相应地，必须调用该轮询器的 `result` 方法，以等待操作的实际结果变为可用。

下面的代码摘自[示例：预配和部署 web 应用](azure-sdk-example-web-app.md)，展示了使用轮询器等待结果的示例：

```python
poller = app_service_client.web_apps.create_or_update(RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    {
        "location": LOCATION,
        "server_farm_id": plan_result.id,
        "site_config": {
            "linux_fx_version": "python|3.8"
        }
    }
)

web_app_result = poller.result()
```

在这种情况下，`create_or_update` 的返回值为类型 `AzureOperationPoller[Site]`，这意味着 `poller.result()` 的返回值是一个 [Site](/python/api/azure-mgmt-web/azure.mgmt.web.v2019_08_01.models.site?view=azure-python) 对象。

## <a name="exceptions"></a>例外

通常情况下，如果操作未能按预期执行（包括对 Azure REST API 的 HTTP 请求失败），Azure 库将引发异常。 对于应用代码，可以在库操作周围使用 `try...except` 块。

有关可能引发的异常类型的详细信息，请参阅相关操作的文档。

## <a name="logging"></a>日志记录

最新的 Azure 库使用 Python 标准 `logging` 库生成日志输出。 可以为单个库、库组或所有库设置日志记录级别。 注册日志记录流处理程序后，便可为特定客户端对象或特定操作启用日志记录。 有关详细信息，请参阅 [Azure 库中的日志记录](azure-sdk-logging.md)。

## <a name="proxy-configuration"></a>代理配置

若要指定代理，可以使用环境变量或可选参数。 有关详细信息，请参阅[如何配置代理](azure-sdk-configure-proxy.md)。

## <a name="optional-arguments-for-client-objects-and-methods"></a>用于客户端对象和方法的可选参数

在库参考文档中，经常会在客户端对象构造函数或特定操作方法的签名中看到 `**kwargs` 或 `**operation_config**` 参数。 这些占位符指示相关的对象或方法可能支持其他命名的自变量。 通常，参考文档中会指示可以使用的特定参数。 此外，通常还支持一些常规参数，后面部分中对此进行了介绍。

### <a name="arguments-for-libraries-based-on-azurecore"></a>基于 azure.core 的库的参数

这些参数适用于 [Python - 新库](https://azure.github.io/azure-sdk/releases/latest/#python)中列出的那些库。

| 名称                       | 类型 | 默认     | 说明 |
| ---                        | ---  | ---         | ---         |
| logging_enable             | bool | False       | 启用日志记录。 有关详细信息，请参阅 [Azure 库中的日志记录](azure-sdk-logging.md)。 |
| proxies                    | dict | {}          | 代理服务器 URL。 有关详细信息，请参阅[如何配置代理](azure-sdk-configure-proxy.md)。 |
| use_env_settings           | bool | True        | 如果为 True，表明允许为代理使用 `HTTP_PROXY` 和 `HTTPS_PROXY` 环境变量。 如果为 False，则忽略环境变量。 有关详细信息，请参阅[如何配置代理](azure-sdk-configure-proxy.md)。 |
| connection_timeout         | int  | 300         | 建立与 Azure REST API 终结点的连接时所产生的超时时间，以秒计算。 |
| read_timeout               | int  | 300         | 完成 Azure REST API 操作（即等待响应）时所产生的超时时间，以秒计算。 |
| retry_total                | int  | 10          | 允许的 REST API 调用重试次数。 使用 `retry_total=0` 禁用重试操作。 |
| retry_mode                 | 枚举 | 指数 | 以线性或指数方式应用重试计时。 如果设置为“单次”，则按固定间隔时间进行重试。 如果设置为“指数”，则每次重试距离上一次重试的时间是上一次间隔时间的两倍。 |

单个库不一定支持其中的参数，请始终查阅每个库的参考文档，了解具体信息。

### <a name="arguments-for-non-core-libraries"></a>非核心库的参数

| 名称               | 类型 | 默认 | 说明 |
| ---                | ---  | ---     | ---         |
| 验证             | bool | True    | 验证 SSL 证书。 |
| cert               | str  | 无    | 用于客户端验证的本地证书的路径。 |
| timeout            | int  | 30      | 用于建立服务器连接的超时值（以秒为单位）。 |
| allow_redirects    | bool | False   | 启用重定向。 |
| max_redirects      | int  | 30      | 允许的最大重定向次数。 |
| proxies            | dict | {}      | 代理服务器 URL。 有关详细信息，请参阅[如何配置代理](azure-sdk-configure-proxy.md)。 |
| use_env_proxies    | bool | False   | 启用从本地环境变量读取代理设置。 |
| retries            | int  | 10      | 允许的总重试次数。 |
| enable_http_logger | bool | False   | 以调试模式启用 HTTP 日志。 |

## <a name="inline-json-pattern-for-object-arguments"></a>对象参数的内联 JSON 模式

Azure 库中的许多操作可用于将对象参数表示为离散对象或内联 JSON。

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

使用 JSON 时，Azure 库会自动将内联 JSON 转换为相关参数的相应对象类型。

对象也可以具有嵌套对象参数，在这种情况下，也可以使用嵌套 JSON。

例如，假设你有一个 [`KeyVaultManagementClient`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.keyvaultmanagementclient?view=azure-python) 对象的实例，并调用其 [`create_or_update`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.operations.vaultsoperations?view=azure-python#create-or-update-resource-group-name--vault-name--parameters--custom-headers-none--raw-false--polling-true----operation-config-) 方法。 在这种情况下，第三个参数的类型为 [`VaultCreateOrUpdateParameters`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultcreateorupdateparameters?view=azure-python)，其本身包含 [`VaultProperties`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.vaultproperties?view=azure-python) 类型的参数。 而 `VaultProperties` 包含类型为 [`Sku`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.sku?view=azure-python) 和 [`list[AccessPolicyEntry`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.accesspolicyentry?view=azure-python) 的对象参数。 `Sku` 包含 [`SkuName`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.skuname?view=azure-python) 对象，且每个 `AccessPolicyEntry` 均包含 [`Permissions`](/python/api/azure-mgmt-keyvault/azure.mgmt.keyvault.v2019_09_01.models.permissions?view=azure-python) 对象。

若要使用嵌入对象调用 `create_or_update`，请使用如下所示的代码（假定已定义了 `tenant_id` 和 `object_id`）。 还可以在该函数调用之前创建必要的对象。

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

由于这两种形式是等效的，因此可以根据自己的喜好选择其中一种形式，甚至将它们混合使用。

如果 JSON 的格式不正确，通常会出现错误“DeserializationError：无法反序列化对象：类型，AttributeError：“str”对象没有属性“get”。 此错误的一个常见原因是，当库需要嵌套的 JSON 对象时，你为属性提供了单个字符串。 例如，在前面的示例中使用 `"sku": "standard"` 会生成此错误，因为 `sku` 参数是一个需要内联对象 JSON 的 `Sku` 对象，在本例中为 `{ "name": "standard"}`，它映射到所需的 `SkuName` 类型。

## <a name="next-steps"></a>后续步骤

了解了有关使用用于 Python 的 Azure 库的常见模式后，请参阅以下独立示例以探索特定的管理和客户端库方案：

- [示例：创建资源组](azure-sdk-example-resource-group.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)

可以按任意顺序尝试使用这些示例，因为它们既不是按顺序编写的，也不是相互依赖的。
