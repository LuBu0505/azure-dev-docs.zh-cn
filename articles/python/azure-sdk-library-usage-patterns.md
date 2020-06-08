---
title: 用于 Python 的 Azure 库的使用模式
description: 用于 Python 的 Azure SDK 库的常见使用模式概述
ms.date: 05/26/2020
ms.topic: conceptual
ms.openlocfilehash: f712dc41233b8301e370c9eb63786d8e2d7f8c70
ms.sourcegitcommit: efab6be74671ea4300162e0b30aa8ac134d3b0a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/01/2020
ms.locfileid: "84256272"
---
# <a name="azure-libraries-for-python-usage-patterns"></a>用于 Python 的 Azure 库的使用模式

用于 Python 的 Azure SDK 完全由[用于 Python 的 Azure SDK 索引页](https://azure.github.io/azure-sdk/releases/latest/all/python.html)上列出的多个独立库组成。

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

## <a name="inline-json-pattern-for-object-arguments"></a>用于对象参数的内联 JSON 模式

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
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)

可以按任意顺序尝试使用这些示例，因为它们既不是按顺序编写的，也不是相互依赖的。
