---
title: 演练，第 6 部分：通过 Azure 服务对 Python 应用进行身份验证
description: 检查主应用的启动代码，该代码可设置 API 终结点所需的 DefaultAzureCredential 对象和客户端对象。
ms.date: 08/24/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 17d5e007c65572ef301a4aa682260af6f492a626
ms.sourcegitcommit: 5ab6e90e20a87f9a8baea652befc74158a9b6613
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89614283"
---
# <a name="part-6-main-app-startup-code"></a>第 6 部分：主应用启动代码

[上一部分：依赖项、导入语句和环境变量](walkthrough-tutorial-authentication-05.md)

应用的启动代码（跟在 `import` 语句之后）可初始化处理 HTTP 请求的函数中使用的不同变量。

首先，我们创建 Flask 应用对象，并从环境变量中检索第三方 API 终结点 URL：

```python
app = Flask(__name__)

number_url = os.environ["THIRD_PARTY_API_ENDPOINT"]
```

接下来，我们获取 [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python
) 对象，这是在通过 Azure 服务进行身份验证时建议使用的凭据。 请参阅[如何对 Python 应用进行身份验证](azure-sdk-authenticate.md#authenticate-with-defaultazurecredential)。

```python
credential = DefaultAzureCredential()
```

当 `DefaultAzureCredential` 在本地运行时，它会查找包含本地服务主体信息的 `AZURE_TENANT_ID`、`AZURE_CLIENT_ID` 和 `AZURE_CLIENT_SECRET` 环境变量。 当 `DefaultAzureCredential` 在云中运行时，它会自动使用为应用注册的服务主体，该主体通常包含在托管标识中。

接下来，代码从 Azure Key Vault 中检索第三方 API 的访问密钥。 在预配脚本中，Key Vault 是使用 [`az keyvault create`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) 创建的，并且机密通过 [`az keyvault secret set`](/cli/azure/keyvault/secret?view=azure-cli-latest#az-keyvault-secret-set) 存储。

Key Vault 资源本身通过从 `KEY_VAULT_URL` 环境变量加载的 URL 访问。

```python
key_vault_url = os.environ["KEY_VAULT_URL"]
```

要连接到密钥保管库，必须创建合适的客户端对象。 由于我们想要检索机密，因此使用 [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python)，这需要密钥保管库 URL 和凭据对象（表示应用在运行时使用的标识）。

```python
keyvault_client = SecretClient(vault_url=key_vault_url, credential=credential)
```

创建 `SecretClient` 对象不会以任何方式对凭据进行身份验证。 `SecretClient` 只是一种在内部管理资源 URL 和凭据的客户端构造。 仅当通过客户端调用操作（例如 [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-)，这会生成对 Azure 资源的 REST API 调用）时，才会发生身份验证和授权。

```python
api_secret_name = os.environ["THIRD_PARTY_API_SECRET_NAME"]
vault_secret = keyvault_client.get_secret(api_secret_name)

# The "secret" from Key Vault is an object with multiple properties. The key we
# want for the third-party API is in the value property. 
access_key = vault_secret.value
```

即使应用标识有权访问密钥保管库，它仍必须获得专门的授权才能访问机密。  否则，`get_secret` 调用将失败。 出于此原因，预配脚本使用 Azure CLI 命令 [`az keyvault set-policy`](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-set-policy) 为应用设置了“获取机密”访问策略。 有关详细信息，请参阅 [Key Vault 身份验证](/azure/key-vault/general/authentication)和[授予应用对 Key Vault 的访问权限](/azure/key-vault/general/managed-identity#grant-your-app-access-to-key-vault)。 后一篇文章介绍了如何使用 Azure 门户设置访问策略。 （还针对托管标识编写了文章，但该文章同样适用于开发中使用的本地服务原则。）

最后，应用代码设置了客户端对象，通过该对象，我们可以将消息写入 Azure 存储队列。 队列的 URL 位于环境变量 `STORAGE_QUEUE_URL` 中。

```python
queue_url = os.environ["STORAGE_QUEUE_URL"]
queue_client = QueueClient.from_queue_url(queue_url=queue_url, credential=credential)
```

与 Key Vault 一样，我们使用 Azure 库中的特定客户端对象 [`QueueClient`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python) 及其 [`from_queue_url`](/python/api/azure-storage-queue/azure.storage.queue.queueclient?view=azure-python#from-queue-url-queue-url--credential-none----kwargs-) 方法连接到位于相关 URL 的资源。 同样，尝试创建此客户端对象将验证凭据表示的应用标识是否获得了访问队列的授权。 如前所述，此授权通过将“存储队列数据参与者”角色分配给主应用来授予。

假设所有这些启动代码都成功，应用将具有其所有内部变量，以支持其 /api/v1/getcode API 终结点。

> [!div class="nextstepaction"]
> [第 7 部分 - 主应用 API 终结点 >>>](walkthrough-tutorial-authentication-07.md)
