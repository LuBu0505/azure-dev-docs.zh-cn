---
title: 如何使 Python 应用程序向 Azure 服务进行身份验证
description: 如何使用 Azure 库获取必要的凭据对象，以使 Python 应用向 Azure 服务进行身份验证
ms.date: 05/12/2020
ms.topic: conceptual
ms.openlocfilehash: 5a882a6cc18ef20a8a26650bacaa7bfe94e90771
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329425"
---
# <a name="how-to-authenticate-python-apps-with-azure-services"></a>如何使 Python 应用向 Azure 服务进行身份验证

使用用于 Python 的 Azure 库编写应用代码时，请使用以下模式访问 Azure 资源：

1. 获取凭据（通常是一次性操作）。
1. 使用该凭据为资源获取相应的客户端对象。
1. 尝试通过客户端对象访问或修改资源，该对象将生成对资源的 REST API 的 HTTP 请求。

对 REST API 的请求是 Azure 对凭据对象所描述的应用标识进行身份验证的点。 然后，Azure 会检查该标识是否有权执行请求的操作。 如果该标识没有授权，则操作失败。 （授予权限取决于资源的类型，如 Azure Key Vault、Azure 存储等。有关详细信息，请参阅该资源类型的文档。）

这些过程中涉及的标识（即，凭据对象所描述的标识）通常由表示用户、组、服务或应用的安全主体定义。 本文中介绍的一些身份验证方法使用显式主体，这通常称为“服务主体”。

但对于大多数云应用程序，我们建议使用第一部分中所述的 `DefaultAzureCredential` 对象，因为它已完全无需处理应用程序的服务主体。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="authenticate-with-defaultazurecredential"></a>使用 DefaultAzureCredential 进行身份验证

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Obtain the credential object. When run locally, DefaultAzureCredential relies
# on environment variables named AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, and AZURE_TENANT_ID.
credential = DefaultAzureCredential()

# Create the client object using the credential
#
# **NOTE**: SecretClient here is only an example; the same process
# applies to all other Azure client libraries.

vault_url = os.environ["KEY_VAULT_URL"]
secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to retrieve a secret value. The operation fails if the principal
# cannot be authenticated or is not authorized for the operation in question.
retrieved_secret = client.get_secret("secret-name-01")
```

[`azure-identity`](/python/api/azure-identity/azure.identity?view=azure-python) 库中的 [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential?view=azure-python) 类提供最简单且建议使用的身份验证方法。

前面的代码在访问 Azure Key Vault 时使用 `DefaultAzureCredential`，其中 Key Vault 的 URL 在名为 `KEY_VAULT_URL` 的环境变量中可用。 此代码明确实现本文开头所述的模式：获取凭据对象，创建 SDK 客户端对象，然后尝试使用该客户端对象执行操作。

同样，在最后一步之前不会进行身份验证和授权。 创建 SDK [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python) 对象不涉及与相关资源的通信；`SecretClient` 对象只是围绕基础 Azure REST API 的包装器，仅存在于应用的运行时内存中。 仅当调用 [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient?view=azure-python#get-secret-name--version-none----kwargs-) 方法时，客户端对象才会生成对 Azure 的相应 REST API 调用。 然后，`get_secret` 的 Azure 终结点会对调用方的标识进行身份验证并检查授权。

代码部署到 Azure 并在 Azure 上运行时，`DefaultAzureCredential` 会自动使用系统分配（“托管”）的标识，你可以在托管该标识的任何服务中为应用启用该标识。 例如，对于部署到 Azure 应用服务的 Web 应用，可通过 Azure 门户中的“标识” > “系统分配”选项或使用 Azure CLI 中的 `az webapp identity assign` 命令启用其托管标识。 还会使用 Azure 门户或 Azure CLI 将特定资源（例如，Azure 存储或 Azure Key Vault）的权限分配给该标识。 在这些情况下，此 Azure 托管的标识将最大限度地提高安全性，因为你从不在代码中处理显式服务主体。

在本地运行代码时，`DefaultAzureCredential` 会自动使用名为 `AZURE_TENANT_ID`、`AZURE_CLIENT_ID` 和 `AZURE_CLIENT_SECRET` 的环境变量所描述的服务主体。 然后，在调用 API 终结点时，SDK 客户端对象会在 HTTP 请求标头中（以安全的方式）包含这些值。 不需要更改代码。 有关创建服务主体和设置环境变量的详细信息，请参阅[为 Azure 配置本地 Python 开发环境 - 配置身份验证](configure-local-development-environment.md#configure-authentication)。

在这两种情况下，必须向所涉及的标识分配适当资源的权限，各个服务的文档中对此进行了介绍。 有关前面代码所需的 Key Vault 权限的详细信息，请参阅[使用访问控制策略提供 Key Vault 身份验证](/azure/key-vault/general/group-permissions-for-apps)。

<a name="cli-auth-note"></a>
> [!IMPORTANT]
> 在将来，如果服务主体环境变量不可用，`DefaultAzureCredential` 将使用通过 `az login` 登录到 Azure CLI 的标识。 如果你是订阅的所有者或管理员，则此功能的实际作用是，无需分配任何特定权限，你的代码就可以访问该订阅中的大多数资源。 此行为对于试验非常方便。 但是，我们强烈建议你在开始编写生产代码时使用特定服务主体并分配特定权限，因为你将了解如何将确切的权限分配给不同的标识，以及在部署到生产环境之前可以在测试环境中准确验证这些权限。

### <a name="using-defaultazurecredential-with-sdk-management-libraries"></a>将 DefaultAzureCredential 与 SDK 管理库配合使用

```python
# WARNING: this code presently fails!

from azure.identity import DefaultAzureCredential

# azure.mgmt.resource is an Azure SDK management library
from azure.mgmt.resource import SubscriptionClient

# Attempt to retrieve the subscription ID
credential = DefaultAzureCredential()
subscription_client = SubscriptionClient(credential)

# The following line produces a "no attribute 'signed_session'" error:
subscription = next(subscription_client.subscriptions.list())

print(subscription.subscription_id)
```

目前，`DefaultAzureCredential` 仅适用于 Azure SDK 客户端（“数据平面”）库，不适用于名称以 `azure-mgmt` 开头的 Azure SDK 管理库，如此代码示例中所示。 调用 `subscription_client.subscriptions.list()` 失败，并出现相当模糊的错误，“DefaultAzureCredential”对象没有属性“signed_session”。 出现此错误的原因是，当前 SDK 管理库假定凭据对象包含 `DefaultAzureCredential` 缺少的 `signed_session` 属性。

直到稍后在 2020 年更新这些库，才可以通过以下两种方法解决该错误：

1. 使用本文后续部分中所述的其他身份验证方法之一，这些方法非常适用于仅使用 SDK 管理库且不会部署到云的代码，在这种情况下，你只能依赖于本地服务主体。

1. 使用 Azure SDK 工程团队成员提供的 [CredentialWrapper 类 (cred_wrapper.py)](https://gist.github.com/lmazuel/cc683d82ea1d7b40208de7c9fc8de59d)，而不是 `DefaultAzureCredential`。 Microsoft 发布更新的管理库后，就可以直接切换回 `DefaultAzureCredential`。 此方法的优点是，可以将同一凭据同时用于 SDK 客户端和管理库，并且它在本地和云中都有效。

    假定已将 cred_wrapper.py 的副本下载到项目文件夹中，则前面的代码将如下所示：

    ```python
    from cred_wrapper import CredentialWrapper
    from azure.mgmt.resource import SubscriptionClient

    credential = CredentialWrapper()
    subscription_client = SubscriptionClient(credential)
    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

    更新管理库后，就可以直接使用 `DefaultAzureCredential`。

## <a name="other-authentication-methods"></a>其他身份验证方法

尽管 `DefaultAzureCredential` 是大多数情况下建议使用的身份验证方法，但其他方法也可用，有以下注意事项：

- 大多数方法适用于显式服务主体，并且不会利用部署到云的代码的托管标识。 与生产代码一起使用时，必须为云应用程序管理和维护不同的服务主体。

- 某些方法（例如基于 CLI 的身份验证）仅适用于本地脚本，不能与生产代码一起使用。

部署到云的应用程序的服务主体在 Active Directory 订阅中进行管理。 有关详细信息，请参阅[如何管理服务主体](how-to-manage-service-principals.md)。

### <a name="authenticate-with-a-json-file"></a>使用 JSON 文件进行身份验证

在此方法中，创建一个 JSON 文件（其中包含服务主体所需的凭据）。 然后使用该文件创建 SDK 客户端对象。 此方法既可在本地使用，也可在云中使用。 

1. 使用以下格式创建 JSON 文件：

    ```json
    {
        "subscriptionId": "<azure_aubscription_id>",
        "tenantId": "<tenant_id>",
        "clientId": "<application_id>",
        "clientSecret": "<application_secret>",
        "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
        "resourceManagerEndpointUrl": "https://management.azure.com/",
        "activeDirectoryGraphResourceId": "https://graph.windows.net/",
        "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
        "galleryEndpointUrl": "https://gallery.azure.com/",
        "managementEndpointUrl": "https://management.core.windows.net/"
    }
    ```

    将四个占位符替换为你的 Azure 订阅 ID、租户 ID、客户端 ID 和客户端密码。

    > [!TIP]
    > 如[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述，可以将 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令与 `--sdk-auth` 参数一起使用，以直接生成此 JSON 格式。

1. 使用名称（如 credentials.json）将此文件保存在可供代码访问的安全位置。 若要保护凭据，请确保从源代码管理中省略此文件，并且不要将其与其他开发人员共享。 也就是说，服务主体的租户 ID、客户端 ID 和客户端密码应始终在开发工作站上保持隔离。

1. 创建一个名为 `AZURE_AUTH_LOCATION` 且值为 JSON 文件的路径的环境变量：

    # <a name="cmd"></a>[cmd](#tab/cmd)

    ```cmd
    set AZURE_AUTH_LOCATION=../credentials.json
    ```

    # <a name="bash"></a>[bash](#tab/bash)

    ```bash
    AZURE_AUTH_LOCATION="../credentials.json"
    ```

    ---

    这些示例假定 JSON 文件名为 credentials.json，并且位于项目的父文件夹中。


1. 使用 [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) 方法来创建客户端对象：

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.resource import SubscriptionClient

    # This form of get_client_from_auth_file relies on the AZURE_AUTH_LOCATION
    # environment variable.
    subscription_client = get_client_from_auth_file(SubscriptionClient)

    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

也可以使用 `auth_path` 参数在代码中直接指定路径，在这种情况下，不需要环境变量：

```python
subscription_client = get_client_from_auth_file(SubscriptionClient, auth_path="../credentials.json")
```

### <a name="authenticate-with-a-json-dictionary"></a>使用 JSON 字典进行身份验证

```python
import os
from azure.common.client_factory import get_client_from_json_dict
from azure.mgmt.resource import SubscriptionClient

# Retrieve the IDs and secret to use in the JSON dictionary
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

config_dict = {
   "subscriptionId": subscription_id,
    "tenantId": tenant_id,
   "clientId": client_id,
   "clientSecret": client_secret,
   "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
   "resourceManagerEndpointUrl": "https://management.azure.com/",
   "activeDirectoryGraphResourceId": "https://graph.windows.net/",
   "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
   "galleryEndpointUrl": "https://gallery.azure.com/",
   "managementEndpointUrl": "https://management.core.windows.net/"
}

subscription_client = get_client_from_json_dict(SubscriptionClient, config_dict)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

如上一部分所述，可以通过变量生成必要的 JSON 数据并调用 [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-)，而不是使用文件。 此代码假定已创建[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述的环境变量。 对于部署到云的代码，可以在服务器 VM 上创建这些环境变量，也可以在使用平台服务（如 Azure 应用服务和 Azure Functions）时将这些环境变量用作应用程序设置。

还可以将值存储在 Azure Key Vault 中，并在运行时（而不是使用环境变量）检索这些值。

### <a name="authenticate-with-token-credentials"></a>使用令牌凭据进行身份验证

```python
import os
from azure.mgmt.resource import SubscriptionClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve the IDs and secret to use with ServicePrincipalCredentials
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

credential = ServicePrincipalCredentials(tenant=tenant_id, client_id=client_id, secret=client_secret)

subscription_client = SubscriptionClient(credential)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

在此方法中，将使用从安全存储（例如，Azure Key Vault 或环境变量）中获取的凭据创建 [`ServicePrincipalCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.serviceprincipalcredentials?view=azure-python) 对象。 前面的代码假定已创建[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述的环境变量。

使用此方法，可以通过为客户端对象指定 `base_url` 参数来使用 [Azure 主权或国家云](/azure/active-directory/develop/authentication-national-cloud)，而不是 Azure 公有云：

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

#...

subscription_client = SubscriptionClient(credentials, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```

主权云常量可在 [msrestazure.azure_cloud 库](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py)中找到。

### <a name="authenticate-with-token-credentials-and-an-adal-context"></a>使用令牌凭据和 ADAL 上下文进行身份验证

如果在使用令牌凭据时需要更多的控制，请使用[适用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包装器：

```python
import os, adal
from azure.mgmt.resource import SubscriptionClient
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve the IDs and secret to use with ServicePrincipalCredentials
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]
tenant_id = os.environ["AZURE_TENANT_ID"]
client_id = os.environ["AZURE_CLIENT_ID"]
client_secret = os.environ["AZURE_CLIENT_SECRET"]

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + tenant_id)

credential = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, client_id, client_secret)

subscription_client = SubscriptionClient(credential)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

如果需要 adal 库，请运行 `pip install adal`。

使用此方法，可以使用 [Azure 主权或国家云](/azure/active-directory/develop/authentication-national-cloud)，而不是 Azure 公有云。

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

# ...

LOGIN_ENDPOINT = AZURE_CHINA_CLOUD.endpoints.active_directory
RESOURCE = AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id
```

只需将 `AZURE_PUBLIC_CLOUD` 替换为 [msrestazure.azure_cloud 库](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py)中的相应主权云常量。

### <a name="cli-based-authentication-development-purposes-only"></a>基于 CLI 的身份验证（仅限开发目的）

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import SubscriptionClient

subscription_client = get_client_from_cli_profile(SubscriptionClient)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

在此方法中，将使用通过 Azure CLI 命令 `az login` 登录的用户的凭据创建客户端对象。

可以让 SDK 使用默认订阅 ID，也可以使用 [`az account`](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) 来设置订阅

此方法应仅用于早期试验和开发目的，因为已登录的用户通常拥有所有者或管理员权限，并且无需任何其他权限即可访问大多数资源。 有关详细信息，请参阅有关[将 CLI 凭据与 `DefaultAzureCredential` 配合使用](#cli-auth-note)的上一条注释。

### <a name="deprecated-authenticate-with-userpasscredentials"></a>不推荐使用：使用 UserPassCredentials 进行身份验证

在[适用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 发布之前，必须使用现已弃用的 [`UserPassCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.userpasscredentials?view=azure-python) 类。 此类不支持双因素身份验证，不应再使用。

## <a name="see-also"></a>另请参阅

- [为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)
- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配和使用 Azure 存储](azure-sdk-example-storage.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和使用 MySQL 数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
