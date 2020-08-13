---
title: 使用用于 Python 的 Azure 库预配 Azure 存储
description: 使用 Azure SDK for Python 库预配 Azure 存储帐户中的 blob 容器，然后将文件上传到该容器。
ms.date: 05/29/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 6a956cf0f4f4689e653307f95d5f0900c8d01589
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983429"
---
# <a name="example-provision-azure-storage-using-the-azure-libraries-for-python"></a>示例：使用用于 Python 的 Azure 库预配 Azure 存储

本文介绍如何在 Python 脚本中使用 Azure 管理库来预配一个资源组，该组包含 Azure 存储帐户和 Blob 存储容器。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。）

预配资源后，请参阅[示例：使用 Azure 存储](azure-sdk-example-storage-use.md)，以在 Python 应用程序代码中使用 Azure 客户端库将文件上传到 Blob 存储容器。

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-needed-azure-library-packages"></a>2:安装所需的 Azure 库包

1. 创建一个 requirements.txt 文件，其中列出了此示例中使用的管理库：

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-cli-core
    ```

1. 在激活了虚拟环境的终端下，安装下列要求：

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3：写入代码以预配存储资源

本部分介绍如何通过 Python 代码预配存储资源。 如果你愿意，还可以通过 Azure 门户或[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)来预配资源。

创建包含以下代码的名为“provision_blob.py” 的 Python 文件。 注释对详细信息进行了说明：

```python
import os, random

# Import the needed management objects from the libraries. The azure.common library
# is installed automatically with the other libraries.
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.storage import StorageManagementClient

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-Storage-rg"
LOCATION = "centralus"

# Step 1: Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group

# Step 2: Provision the storage account, starting with a management object.

storage_client = get_client_from_cli_profile(StorageManagementClient)

# This example uses the CLI profile credentials because we assume the script
# is being used to provision the resource in the same way the Azure CLI would be used.

STORAGE_ACCOUNT_NAME = f"pythonazurestorage{random.randint(1,100000):05}"

# You can replace the storage account here with any unique name. A random number is used
# by default, but note that the name changes every time you run this script.
# The name must be 3-24 lower case letters and numbers only.


# Check if the account name is available. Storage account names must be unique across
# Azure because they're used in URLs.
availability_result = storage_client.storage_accounts.check_name_availability(STORAGE_ACCOUNT_NAME)

if not availability_result.name_available:
    print(f"Storage name {STORAGE_ACCOUNT_NAME} is already in use. Try another name.")
    exit()

# The name is available, so provision the account
poller = storage_client.storage_accounts.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME,
    {
        "location" : LOCATION,
        "kind": "StorageV2",
        "sku": {"name": "Standard_LRS"}
    }
)

# Long-running operations return a poller object; calling poller.result()
# waits for completion.
account_result = poller.result()
print(f"Provisioned storage account {account_result.name}")

# Step 3: Retrieve the account's primary access key and generate a connection string.
keys = storage_client.storage_accounts.list_keys(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME)

print(f"Primary key for storage account: {keys.keys[0].value}")

conn_string = f"DefaultEndpointsProtocol=https;EndpointSuffix=core.windows.net;AccountName={STORAGE_ACCOUNT_NAME};AccountKey={keys.keys[0].value}"

print(f"Connection string: {conn_string}")

# Step 4: Provision the blob container in the account (this call is synchronous)
CONTAINER_NAME = "blob-container-01"
container = storage_client.blob_containers.create(RESOURCE_GROUP_NAME, STORAGE_ACCOUNT_NAME, CONTAINER_NAME, {})

# The fourth argument is a required BlobContainer object, but because we don't need any
# special values there, so we just pass empty JSON.

print(f"Provisioned blob container {container.name}")
```

此代码使用基于 CLI 的身份验证方法 (`get_client_from_cli_profile`)，因为它演示了你可能会使用 Azure CLI 直接执行的操作。 在这两种情况下，使用相同的身份验证标识。

若要在生产脚本中使用此类代码，应改为使用 `DefaultAzureCredential`（推荐）或基于服务主体的方法，如[如何使用 Azure 服务对 Python 应用进行身份验证](azure-sdk-authenticate.md)中所述。

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)
- [StorageManagementClient (azure.mgmt.storage)](/python/api/azure-mgmt-storage/azure.mgmt.storage.storagemanagementclient?view=azure-python)

## <a name="4-run-the-script"></a>4.运行脚本

```cmd
python provision_blob.py
```

此脚本需要一到两分钟的时间才能完成。

## <a name="5-verify-the-resources"></a>5：验证资源

1. 打开 [Azure 门户](https://portal.azure.com)，验证是否按预期预配了资源组和存储帐户。 可能需要在资源组中选择“显示隐藏的类型”，才能查看从 Python 脚本预配的存储帐户：

    ![新资源组的 Azure 门户页面，显示存储帐户](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. 选择存储帐户，然后在左侧菜单中选择“Blob 服务” > “容器”，以验证是否显示“bloc-container-01”：

    ![存储帐户的 Azure 门户页面，显示 blob 容器](media/azure-sdk-example-storage/portal-show-blob-containers.png)

1. 如果要尝试通过应用程序代码使用这些预配的资源，请继续阅读[示例：使用 Azure 存储](azure-sdk-example-storage-use.md)。

有关使用 Azure 存储管理库的其他示例，请参阅[管理 Python 存储示例](https://docs.microsoft.com/samples/azure-samples/storage-python-manage/storage-python-manage/)。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤：

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

rem Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus ^
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

rem Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg ^
    -n pythonazurestorage12345

rem Provision the blob container; NOTE: this command assumes you have an environment variable
rem named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonAzureExample-Storage-rg -l centralus \
    -n pythonazurestorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonAzureExample-Storage-rg \
    -n pythonazurestorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonazurestorage12345 -n blob-container-01
```

---

## <a name="6-clean-up-resources"></a>6：清理资源

如果要按照[示例：使用 Azure 存储](azure-sdk-example-storage-use.md)一文中所述的步骤在应用代码中使用这些资源，请将这些资源留在原处。

否则，请运行以下命令，以避免你的订阅中继续产生费用。

```azurecli
az group delete -n PythonAzureExample-Storage-rg
```

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

## <a name="see-also"></a>另请参阅

- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
