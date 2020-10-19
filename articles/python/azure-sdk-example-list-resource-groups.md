---
title: 使用用于 Python 的 Azure 库列出资源组和资源
description: 使用 Azure SDK for Python 中的资源管理库列出资源组和组中的资源。
ms.date: 10/12/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 06395891ea1b8294f9eaafe7dad40bf7a335e965
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010663"
---
# <a name="example-use-the-azure-libraries-to-list-resource-groups-and-resources"></a>示例：使用 Azure 库列出资源组和资源

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库来执行下面两项任务：

- 列出 Azure 订阅中的所有资源组。
- 列出特定资源组中的资源。
 
除非另行说明，否则本文中的所有资源在 Linux/macOS bash 和 Windows 命令行界面上的工作方式相同。

本文稍后部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。

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

## <a name="3-write-code-to-work-with-resource-groups"></a>3:编写代码用于处理资源组

### <a name="3a-list-resource-groups-in-a-subscription"></a>3a. 列出订阅中的资源组

创建名为 list_groups.py 的 Python 文件，其中包含以下代码。 注释对详细信息进行了说明：

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Retrieve the list of resource groups
group_list = resource_client.resource_groups.list()

# Show the groups in formatted output
column_width = 40

print("Resource Group".ljust(column_width) + "Location")
print("-" * (column_width * 2))

for group in list(group_list):
    print(f"{group.name:<{column_width}}{group.location}")
```

### <a name="3b-list-resources-within-a-specific-resource-group"></a>3b. 列出特定资源组中的资源

创建名为 list_resources.py 的 Python 文件，其中包含以下代码。 注释对详细信息进行了说明：

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
import os

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# Obtain the management object for resources.
resource_client = ResourceManagementClient(credential, subscription_id)

# Retrieve the list of resources in "myResourceGroup" (change to any name desired).
# The expand argument includes additional properties in the output.
resource_list = resource_client.resources.list_by_resource_group(
    "myResourceGroup",
    expand = "createdTime,changedTime"
)

# Show the resources in formatted output
column_width = 36

print("Resource".ljust(column_width) + "Type".ljust(column_width)
    + "Create date".ljust(column_width) + "Change date".ljust(column_width))
print("-" * (column_width * 4))

for resource in list(resource_list):
    print(f"{resource.name:<{column_width}}{resource.type:<{column_width}}"
       f"{str(resource.created_time):<{column_width}}{str(resource.changed_time):<{column_width}}")
```

### <a name="authentication-in-the-code"></a>代码中的身份验证

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)

## <a name="4-run-the-scripts"></a>4:运行脚本

列出订阅中的所有资源组：

```cmd
python list_groups.py
```

列出资源组中的所有资源：

```cmd
python list_resources.py
```

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令使用 JSON 输出来列出订阅中的资源组：

```azurecli
az group list
```

以下命令列出 centralus 区域中“myResourceGroup”内的资源（必须使用位置参数来标识特定数据中心）：

```azurecli
az resource list --resource group myResourceGroup --location centralus
```

## <a name="see-also"></a>另请参阅

- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
