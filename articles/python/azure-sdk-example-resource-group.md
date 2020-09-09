---
title: 使用用于 Python 的 Azure 库预配资源组
description: 使用 Azure SDK for Python 中的资源管理库通过 Python 代码创建资源组。
ms.date: 05/29/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 459311e76f70e10a5da62c4205a9843427f76cec
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275111"
---
# <a name="example-use-the-azure-libraries-to-provision-a-resource-group"></a>示例：使用 Azure 库预配资源组

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库来预配资源组。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。 如果你想要使用 Azure 门户，请参阅[创建资源组](/azure/azure-resource-manager/management/manage-resource-groups-portal)。）

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-azure-library-packages"></a>2:安装 Azure 库包

创建一个具有以下内容的名为 *requirements.txt* 的文件：

```text
azure-mgmt-resource
azure-cli-core
```

在激活了虚拟环境的终端或命令提示符下，安装下列要求：

```cmd
pip install -r requirements.txt
```

## <a name="3-write-code-to-provision-a-resource-group"></a>3：写入代码以预配资源组

创建包含以下代码的名为“provision_rg.py” 的 Python 文件。 注释对详细信息进行了说明：

```python
# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(
    "PythonAzureExample-ResourceGroup-rg",
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

# Optional line to delete the resource group
#resource_client.resource_groups.delete("PythonAzureExample-ResourceGroup-rg")
```

此代码使用基于 CLI 的身份验证方法 (`get_client_from_cli_profile`)，因为它演示了你可能会使用 Azure CLI 直接执行的操作。 在这两种情况下，使用相同的身份验证标识。

若要在生产脚本中使用此类代码，应改为使用 `DefaultAzureCredential`（推荐）或基于服务主体的方法，如[如何使用 Azure 服务对 Python 应用进行身份验证](azure-sdk-authenticate.md)中所述。

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)

## <a name="4-run-the-script"></a>4：运行脚本

```cmd
python provision_rg.py
```

## <a name="5-verify-the-resource-group"></a>5：验证资源组

可以通过 Azure 门户或 Azure CLI 来验证该组是否存在。

- Azure 门户：打开 [Azure 门户](https://portal.azure.com)，选择“资源组”，并查看是否列出了该组。 如果已打开门户，请使用 Refresh 命令更新该列表。

- Azure CLI：运行以下命令：

    ```azurecli
    az group show -n PythonAzureExample-ResourceGroup-rg
    ```

## <a name="6-clean-up-resources"></a>6：清理资源

```azurecli
az group delete -n PythonAzureExample-ResourceGroup-rg
```

如果不需要保留在此示例中预配的资源组，请运行此命令。 资源组不会在你的订阅中产生任何持续的费用，但最好清除你不会主动使用的任何组。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤：

```azurecli
az group create -n PythonAzureExample-ResourceGroup-rg -l centralus
```

## <a name="see-also"></a>另请参阅

- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
