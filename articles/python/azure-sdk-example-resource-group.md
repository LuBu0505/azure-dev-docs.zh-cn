---
title: 使用用于 Python 的 Azure 库预配资源组
description: 使用 Azure SDK for Python 中的资源管理库通过 Python 代码创建资源组。
ms.date: 11/12/2020
ms.topic: conceptual
ms.custom: devx-track-python, devx-track-azurecli
ms.openlocfilehash: 65c78e480336f689096ccbd9f75420febf732f20
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94872838"
---
# <a name="example-use-the-azure-libraries-to-provision-a-resource-group"></a>示例：使用 Azure 库预配资源组

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库来预配资源组。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。 如果你想要使用 Azure 门户，请参阅[创建资源组](/azure/azure-resource-manager/management/manage-resource-groups-portal)。）

除非另行说明，否则本文中的所有资源在 Linux/macOS bash 和 Windows 命令行界面上的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

请务必为该项目创建一个虚拟环境并将其激活。

## <a name="2-install-the-azure-library-packages"></a>2:安装 Azure 库包

创建一个具有以下内容的名为 *requirements.txt* 的文件：

```text
azure-mgmt-resource
azure-identity
```

在激活了虚拟环境的终端或命令提示符下，安装下列要求：

```cmd
pip install -r requirements.txt
```

## <a name="3-write-code-to-provision-a-resource-group"></a>3：写入代码以预配资源组

创建包含以下代码的名为“provision_rg.py” 的 Python 文件。 注释对详细信息进行了说明：

```python
# Import the needed credential and management objects from the libraries.
from azure.mgmt.resource import ResourceManagementClient
from azure.identity import AzureCliCredential
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-rg",
    {
        "location": "centralus"
    }
)

# Within the ResourceManagementClient is an object named resource_groups,
# which is of class ResourceGroupsOperations, which contains methods like
# create_or_update.
#
# The second parameter to create_or_update here is technically a ResourceGroup
# object. You can create the object directly using ResourceGroup(location=LOCATION)
# or you can express the object as inline JSON as shown here. For details,
# see Inline JSON pattern for object arguments at
# https://docs.microsoft.com/azure/developer/python/azure-sdk-overview#inline-json-pattern-for-object-arguments.

print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# The return value is another ResourceGroup object with all the details of the
# new group. In this case the call is synchronous: the resource group has been
# provisioned by the time the call returns.

# Update the resource group with tags
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-rg",
    {
        "location": "centralus",
        "tags": { "environment":"test", "department":"tech" }
    }
)

print(f"Updated resource group {rg_result.name} with tags")

# Optional lines to delete the resource group. begin_delete is asynchronous.
# poller = resource_client.resource_groups.begin_delete(rg_result.name)
# result = poller.result()
```

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)

## <a name="4-run-the-script"></a>4：运行脚本

```cmd
python provision_rg.py
```

## <a name="5-verify-the-resource-group"></a>5：验证资源组

可以通过 Azure 门户或 Azure CLI 来验证该组是否存在。

- Azure 门户：打开 [Azure 门户](https://portal.azure.com)，选择“资源组”，并查看是否列出了该组。 如果已打开门户，请使用 Refresh 命令更新该列表。

- Azure CLI：运行以下命令：

    ```azurecli
    az group show -n PythonAzureExample-rg
    ```

## <a name="6-clean-up-resources"></a>6：清理资源

```azurecli
az group delete -n PythonAzureExample-rg  --no-wait
```

如果不需要保留在此示例中预配的资源组，请运行此命令。 资源组不会在你的订阅中产生任何持续的费用，但最好清除你不会主动使用的任何组。 `--no-wait` 参数允许命令立即返回，而不是等到操作完成再返回。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤：

```azurecli
az group create -n PythonAzureExample-rg -l centralus
```

## <a name="see-also"></a>另请参阅

- [示例：列出订阅中的资源组](azure-sdk-example-list-resource-groups.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
