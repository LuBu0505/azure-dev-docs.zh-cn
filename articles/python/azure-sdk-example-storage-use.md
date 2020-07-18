---
title: 通过用于 Python 的 Azure SDK 使用 Azure 存储
description: 使用用于 Python 的 Azure SDK 库访问 Azure 存储帐户中预配的 Blob 容器，然后将文件上传到该容器。
ms.date: 06/15/2020
ms.topic: conceptual
ms.openlocfilehash: 41c2c586678084e30f9f5b2bff3c773b46f5463d
ms.sourcegitcommit: c6642cae6fdb5e3025ed66fcd4ef89792c3b436a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/15/2020
ms.locfileid: "86405728"
---
# <a name="example-access-azure-storage-using-the-azure-libraries-for-pyhon"></a>示例：使用用于 Python 的 Azure 库访问 Azure 存储

此示例演示了如何在 Python 应用程序代码中使用 Azure 客户端库，以便将文件上传到 Blob 存储容器。 该示例假设你已预配了[示例：预配 Azure 存储](azure-sdk-example-storage.md)中所示的资源。

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-library-packages"></a>2:安装库包

1. 在 requirements.txt 文件中，为所需的客户端库包添加行，然后保存该文件：

    ```text
    azure-storage-blob
    azure-identity
    ```

1. 在终端或命令提示符下，重新安装下列要求：

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-create-a-file-to-upload"></a>3：创建要上传的文件

创建一个名为“sample-source.txt”的源文件（如代码所示），其内容类似于以下内容：

```text
Hello there, Azure Storage. I'm a friendly file ready to be stored in a blob.
```

## <a name="4-use-blob-storage-from-app-code"></a>4：通过应用代码使用 Blob 存储

以下部分（编号为 4a 和 4b）演示了用于访问 Blob 容器的两种方法，该容器已进行预配，请参见[示例：预配 Azure 存储](azure-sdk-example-storage.md)中所示的资源。

[第一种方法（下面的 4a 部分）](#4a-use-blob-storage-with-authentication)使用 `DefaultAzureCredential` 对应用进行身份验证，如[如何对 Python 应用进行身份验证](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)中所述。 使用此方法时，必须先将相应的权限分配给应用标识，这是建议的做法。

[第二种方法（下面的 4b 部分）](#4b-use-blob-storage-with-a-connection-string)使用连接字符串直接访问存储帐户。 尽管此方法看起来更简单，但它有两个重大缺点：

- 连接字符串在本质上使用存储帐户（而不是该帐户中的单个资源）来验证连接代理。 因此，连接字符串提供的授权范围超出了所需的权限。

- 连接字符串包含以纯文本形式提供的访问密钥，因此，如果未正确构造或未提供适当保护，则会出现潜在的漏洞。 如果公开了此类连接字符串，则可将其用于访问存储帐户中的各种资源。

出于这些原因，建议在生产代码中使用该身份验证方法。

### <a name="4a-use-blob-storage-with-authentication"></a>4a：将 Blob 存储与身份验证配合使用

1. 创建名为 `STORAGE_BLOB_URL` 的环境变量：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    STORAGE_BLOB_URL=https://pythonazurestorage12345.blob.core.windows.net
    ```

    ---

    将“pythonazurestorage12345”替换为特定存储帐户的名称。

    此环境变量仅由该示例使用，不会由 Azure 库使用。

1. 使用以下代码创建一个名为“use_blob_auth py”的文件。 注释对步骤进行了说明。

    ```python
    import os
    from azure.identity import DefaultAzureCredential

    # Import the client object from the Azure library
    from azure.storage.blob import BlobClient

    credential = DefaultAzureCredential()

    # Retrieve the storage blob service URL, which is of the form
    # https://pythonazurestorage12345.blob.core.windows.net/
    storage_url = os.environ["STORAGE_BLOB_URL"]

    # Create the client object using the storage URL and the credential
    blob_client = BlobClient(storage_url, container_name="blob-container-01", blob_name="sample-blob.txt", credential=credential)

    # Open a local file and upload its contents to Blob Storage
    with open("./sample-source.txt", "rb") as data:
        blob_client.upload_blob(data)
    ```

    参考链接：
      - [DefaultAzureCredential (azure.identity)](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python)
      - [BlobClient (azure.storage.blob)](/python/api/azure-storage-blob/azure.storage.blob.blobclient?view=azure-python)

1. 尝试运行代码（有意失败）：

    ```cmd
    python use_blob_auth.py
    ```

    由于你使用的本地服务主体不具有访问 blob 容器的权限，你会看到以下错误：“此请求无权使用此权限执行此操作”。

1. 使用 Azure CLI 命令 [az role assignment create](/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)（此命令较长！）将容器权限授予服务主体：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```azurecli
    az role assignment create --assignee %AZURE_CLIENT_ID% ^
        --role "Storage Blob Data Contributor" ^
        --scope "/subscriptions/%AZURE_SUBSCRIPTION_ID%/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```azurecli
    az role assignment create --assignee $AZURE_CLIENT_ID \
        --role "Storage Blob Data Contributor" \
        --scope "/subscriptions/$AZURE_SUBSCRIPTION_ID/resourceGroups/PythonAzureExample-Storage-rg/providers/Microsoft.Storage/storageAccounts/pythonazurestorage12345/blobServices/default/containers/blob-container-01"
    ```

    ---

    `--scope` 参数标识此角色分配适用的位置。 在此示例中，将“存储 Blob 数据参与者”角色授予名为“blob-container-01”的特定容器。

    将 `pythonazurestorage12345` 替换为存储帐户的确切名称。 如果需要，还可以调整资源组和 blob 容器的名称。 如果使用错误名称，会发现错误“无法对嵌套资源执行请求的操作。 找不到父资源‘pythonazurestorage12345’”。

    此命令中的 `--scope` 参数还使用 AZURE_CLIENT_ID 和 AZURE_SUBSCRIPTION_ID 环境变量，你应该通过按照[配置适用于 Azure 的本地 Python 开发环境](configure-local-development-environment.md)在本地环境中为服务主体设置这些变量。

1. 权限传播需要花费时间，因此请等待一两分钟，然后再次运行该代码，验证它现在是否正常工作。 如果再次看到权限错误，请等待更长的时间，然后重试代码。

有关作用域和角色分配的更多信息，请参阅[如何分配角色权限](how-to-assign-role-permissions.md)。

### <a name="4b-use-blob-storage-with-a-connection-string"></a>4b：将 blob 存储与连接字符串配合使用

1. 创建一个名为 `AZURE_STORAGE_CONNECTION_STRING` 的环境变量，其值是存储帐户的完整连接字符串。 （此环境变量还由各种 Azure CLI 注释使用。）

1. 创建包含以下代码的名为“use_blob_conn_string.py”的 Python 文件。 注释对步骤进行了说明。

    ```python
    import os

    # Import the client object from the Azure library
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

## <a name="5-verify-blob-creation"></a>5.验证 blob 创建

运行任一方法的代码后，请转到 [Azure 门户](https://portal.azure.com)，导航到 blob 容器，以验证是否存在名为“sample-blob.txt”的新 blob，并使用与 sample-source.txt 文件相同的内容：

![blob 容器的 Azure 门户页面，显示已上传的文件](media/azure-sdk-example-storage/portal-blob-container-file.png)

## <a name="6-clean-up-resources"></a>6：清理资源

```azurecli
az group delete -n PythonAzureExample-Storage-rg
```

如果不需要保留预配在此示例中的资源，并想要避免订阅中的持续费用，则运行此命令。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

## <a name="see-also"></a>另请参阅

- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
