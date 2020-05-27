---
title: 通过 Azure SDK for Python 预配和使用 Azure 存储
description: 使用 Azure SDK for Python 库预配 Azure 存储帐户中的 blob 容器，然后将文件上传到该容器。
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 9b26dd4c5708231c807ea57979bed6bdcd9de25f
ms.sourcegitcommit: 2cdf597e5368a870b0c51b598add91c129f4e0e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2020
ms.locfileid: "83405100"
---
# <a name="example-use-the-azure-sdk-with-azure-storage"></a>示例：将 Azure SDK 与 Azure 存储配合使用

本文介绍如何在 Python 脚本中使用 Azure SDK 管理库来预配资源组，该组包含 Azure 存储帐户和 Blob 存储容器。 然后，你将了解如何在 Python 应用程序代码中使用 Azure SDK 客户端库，将文件上传到 Blob 存储容器。

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

请确保创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-needed-management-libraries"></a>2:安装所需的管理库

1. 创建一个 requirements.txt 文件，其中列出了此示例中使用的管理库：

    ```txt
    azure-mgmt-resource
    azure-mgmt-storage
    azure-cli-core
    ```

1. 在激活了虚拟环境的终端下，安装下列要求：

    ```bash
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-storage-resources"></a>3：写入代码以预配存储资源

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
RESOURCE_GROUP_NAME = "PythonSDKExample-Storage-rg"
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

STORAGE_ACCOUNT_NAME = f"pythonsdkstorage{random.randint(1,100000):05}"

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

## <a name="4-run-the-script"></a>4.运行脚本：

```bash
python provision_blob.py
```

此脚本需要一到两分钟的时间才能完成。

## <a name="5-verify-the-resources"></a>5：验证资源

1. 打开 [Azure 门户](https://portal.azure.com)，验证是否按预期预配了资源组和存储帐户。 可能需要在资源组中选择“显示隐藏的类型”，才能查看从 Python 脚本预配的存储帐户：

    ![新资源组的 Azure 门户页面，显示存储帐户](media/azure-sdk-example-storage/portal-show-hidden-types.png)

1. 选择存储帐户，然后在左侧菜单中选择“Blob 服务” > “容器”，以验证是否显示“bloc-container-01”：

    ![存储帐户的 Azure 门户页面，显示 blob 容器](media/azure-sdk-example-storage/portal-show-blob-containers.png)

有关使用 Azure 存储管理库的其他示例，请参阅[管理 Python 存储示例](https://docs.microsoft.com/samples/azure-samples/storage-python-manage/storage-python-manage/)。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤：

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus \
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg \
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
# Provision the resource group

az group create -n PythonSDKExample-Storage-rg -l centralus

# Provision the storage account

az storage account create -g PythonSDKExample-Storage-rg -l centralus ^
    -n pythonsdkstorage12345 --kind StorageV2 --sku Standard_LRS

# Retrieve the connection string

az storage account show-connection-string -g PythonSDKExample-Storage-rg ^
    -n pythonsdkstorage12345

# Provision the blob container; NOTE: this command assumes you have an environment variable
# named AZURE_STORAGE_CONNECTION_STRING with the connection string for the storage account.

set AZURE_STORAGE_CONNECTION_STRING=<connection_string>
az storage container create --account-name pythonsdkstorage12345 -n blob-container-01
```

---

## <a name="6-use-resources-through-the-sdk-client-libraries"></a>6：通过 SDK 客户端库使用资源

以下部分介绍了访问上一节中预配的 blob 容器的两种方法。 这些示例专门使用适当的 SDK 客户端库将文件 blob 上传到该容器。

按照[常见步骤](#common-steps-for-both-methods)亲自尝试代码。

第一种方法用 `DefaultAzureCredential` 对应用进行身份验证，如[如何对 Python 应用进行身份验证](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)中所述。 使用此方法时，必须先将相应的权限分配给应用标识，这是建议的做法。

第二种方法使用连接字符串直接访问存储帐户。 尽管此方法看起来更简单，但它有两个重大缺点：

- 连接字符串在本质上使用存储帐户（而不是该帐户中的单个资源）来验证连接代理。 因此，连接字符串提供的授权范围超出了所需的权限。

- 连接字符串包含以纯文本形式提供的访问密钥，因此，如果未正确构造或未提供适当保护，则会出现潜在的漏洞。 如果公开了此类连接字符串，则可将其用于访问存储帐户中的各种资源。

出于上述原因，生产代码应使用身份验证方法。 但对于试验，最好使用连接字符串。

### <a name="common-steps-for-both-methods"></a>两种方法的常见步骤

1. 在 requirements.txt 文件中，为所需的客户端库添加行并保存文件：

    ```text
    azure-storage-blob
    azure-identity
    ```

1. 在终端或命令提示符下，重新安装下列要求：

    ```bash
    pip install -r requirements.txt
    ```

1. 创建一个名为“sample-source.txt”的源文件（如代码所示），其内容类似于以下内容：

    ```text
    Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
    ```

### <a name="use-blob-storage-with-authentication"></a>将 Blob 存储与身份验证配合使用

1. 创建名为 `AZURE_STORAGE_BLOB_URL` 的环境变量：

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_STORAGE_BLOB_URL=https://pythonsdkstorage12345.blob.core.windows.net
    ```

    ---

    将“pythonsdkstorage12345”替换为特定存储帐户的名称。

1. 使用以下代码创建一个名为“use_blob_auth py”的文件。 注释对步骤进行了说明。

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the SDK library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonsdkstorage12345.blob.core.windows.net/
    storage_url = os.environ["AZURE_STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. 尝试运行以下代码：

    ```bash
    python use_blob_auth.py
    ```

    由于你使用的本地服务主体不具有访问 blob 容器的权限，你会看到以下错误：“此请求无权使用此权限执行此操作”。

1. 若要将容器的权限授予服务主体，请使用 Azure CLI 命令 [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)（此命令较长！）：

    # <a name="bash"></a>[bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonSDKExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonsdkstorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    `--scope` 参数标识此角色分配适用的位置。 在此示例中，将“存储 Blob 数据参与者”角色授予名为“blob-container-01”的特定容器。

    将 `pythonsdkstorage12345` 替换为存储帐户的确切名称。 如果需要，还可以调整资源组和 blob 容器的名称。 如果使用错误名称，会发现错误“无法对嵌套资源执行请求的操作。 找不到父资源“pythonsdkstorage12345”。

    此命令中的 `--scope` 参数还使用 AZURE_CLIENT_ID 和 AZURE_SUBSCRIPTION_ID 环境变量，你应该通过按照[配置适用于 Azure 的本地 Python 开发环境](configure-local-development-environment.md)在本地环境中为服务主体设置这些变量。

1. 再次运行代码，以验证代码现在是否正常工作。 如果再次看到权限错误，请等待一分钟以便传播权限，然后重试该代码。

有关作用域和角色分配的更多信息，请参阅[如何分配角色权限](how-to-assign-role-permissions.md)。

### <a name="use-blob-storage-with-a-connection-string"></a>将 blob 存储与连接字符串配合使用

1. 创建包含以下代码的名为“use_blob_conn_string.py”的 Python 文件。 注释对步骤进行了说明。

    ```python
    import os

    # Import the client object from the SDK library
    from azure.storage.blob import BlobClient

    # Retrieve the connection string from an environment variable.
    conn_string = os.environ["AZURE_STORAGE_CONNECTION_STRING"]

    # Create the client object for the resource identified by the connection string,
    # indicating also the blob container and the name of the specific blob we want.
    blob_client = BlobClient.from_connection_string(conn_string, container_name="blob-container-01", blob_name="sample-blob.txt")

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

1. 运行代码：

    ```bash
    python use_blob_conn_string.py
    ```

同样，虽然此方法很简单，但连接字符串会授权存储帐户中的所有操作。 对于生产代码，最好使用上一节中所述的特定权限。

### <a name="verify-blob-creation"></a>验证 blob 创建

运行任一方法的代码后，请转到 [Azure 门户](https://portal.azure.com)，导航到 blob 容器，以验证是否存在名为“sample-blob.txt”的新 blob，并使用与 sample-source.txt 文件相同的内容：

![blob 容器的 Azure 门户页面，显示已上传的文件](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="7-clean-up-resources"></a>7:清理资源

```azurecli
az group delete -n PythonSDKExample-Storage-rg
```

如果不需要保留预配在此示例中的资源，并想要避免订阅中的持续费用，则运行此命令。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [示例：预配 Web 应用并部署代码 >>>](azure-sdk-example-web-app.md)
