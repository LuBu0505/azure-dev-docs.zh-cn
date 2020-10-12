---
title: 如何通过 Azure 服务对 Python 应用程序进行身份验证
description: 如何使用 Azure 库获取必要的凭据对象，以使 Python 应用向 Azure 服务进行身份验证
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: 1fe206394d05e07b19254520131447770cbbd5b0
ms.sourcegitcommit: 29b161c450479e5d264473482d31e8d3bf29c7c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91764673"
---
# <a name="how-to-authenticate-and-authorize-python-apps-on-azure"></a>如何在 Azure 上对 Python 应用进行身份验证和授权

大多数部署到 Azure 的云应用程序都需要访问其他 Azure 资源，如存储、数据库、存储的机密等等。 若要访问这些资源，必须对应用程序进行身份验证和授权：

- 身份验证通过 Azure Active Directory 验证应用的标识。

- 授权确定经过身份验证的应用可以对任何给定资源执行哪些操作。 授权操作由分配给该资源的应用标识的角色定义。 在少数情况下（如 Azure Key Vault），授权还由分配给应用标识的其他访问策略确定。

本文介绍了身份验证和授权的详细信息：

- 如何分配应用标识
- 如何向标识授予权限
- 身份验证和授权的发生方式和时间
- 应用使用 Azure 库对 Azure 进行身份验证的不同方法。 建议使用 `DefaultAzureCredential`，但这不是必需的。

## <a name="how-to-assign-an-app-identity"></a>如何分配应用标识

在 Azure 上，应用标识由服务主体定义。 （服务主体是特定类型的“安全主体”，用于标识应用或服务，即一段代码，而不是一个用户或一组用户。）

涉及的服务主体取决于应用的运行位置，如以下各节中所述。

### <a name="identity-when-running-the-app-on-azure"></a>在 Azure 上运行应用时的标识

在云中（例如在生产环境中）运行时，应用最常使用系统分配的托管标识。 使用[托管标识](/azure/active-directory/managed-identities-azure-resources/overview)，可以在为资源分配角色和权限时使用应用的名称。 Azure 自动管理基础服务主体，并使用其他 Azure 资源自动对应用进行身份验证。 因此，你无需直接处理服务主体。 此外，应用代码永远无需处理 Azure 资源的访问令牌、机密或连接字符串，从而降低了此类信息可能被泄漏或以其他方式遭到入侵的风险。

托管标识的配置取决于用于托管应用的服务。 请参阅[支持托管标识的服务](/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)一文，以获取指向每个服务的说明的链接。 例如，对于部署到 Azure 应用服务的 Web 应用，可通过 Azure 门户中的“标识” > “系统分配”选项或使用 Azure CLI 中的 `az webapp identity assign` 命令启用托管标识 。

如果无法使用托管标识，请改为手动将应用程序注册到 Azure Active Directory。 注册操作可为应用分配服务主体，供你在分配角色和权限时使用。 有关详细信息，请参阅[注册应用程序](/azure/active-directory/develop/quickstart-register-app)。

### <a name="identity-when-running-the-app-locally"></a>在本地运行应用时的标识

在开发过程中，你通常希望在开发人员工作站上运行和调试应用代码，同时让此代码仍可访问云中的 Azure 资源。 在这种情况下，可以通过 Azure Active Directory 创建一个单独的服务主体，专门用于本地开发。 再次将角色和权限分配给该服务主体，以获得相关资源。 通常，你授权此开发标识仅访问非生产资源。

要详细了解如何创建本地服务主体并使其可供 Azure 库使用，请参阅[配置本地开发环境](configure-local-development-environment.md)。 完成这个一次性配置后，你可在本地和云中运行相同的应用代码，而无需进行任何特定于环境的修改。

每个开发人员都应具有自己的服务主体，这些服务主体可以安全地存储在他们的工作站上的用户帐户中，且永远不要将其存储在源代码管理存储库中。 如有任一个服务主体被窃取或泄露，你可轻松删除它以撤销其所有权限，然后为该开发人员重新创建服务主体。 有关详细信息，请参阅[如何管理服务主体](how-to-manage-service-principals.md)。

> [!NOTE]
> 尽管可以使用自己的 Azure 用户凭据运行应用，但这样做并不能帮助你创建将应用部署到云时所需的特定资源权限。 最好是设置用于开发的服务主体，并为其分配所需的角色和权限，然后你可以使用已部署应用的托管标识或服务主体复制这些角色和权限。

## <a name="assign-roles-and-permissions-to-an-identity"></a>为标识分配角色和权限

了解应用在 Azure 上和在本地运行时的标识后，就可以使用基于角色的访问控制 (RBAC) 通过 Azure 门户或 Azure CLI 授予权限。 如需了解完整的详细信息，请参阅[如何向应用标识或服务主体分配角色权限](/azure/role-based-access-control/role-assignments-steps)。

## <a name="when-does-authentication-and-authorization-occur"></a>身份验证和授权何时发生？

使用用于 Python 的 Azure 库 (SDK) 编写应用代码时，请使用以下模式访问 Azure 资源：

1. 使用本文后面介绍的方法之一来获取用于描述应用标识的凭据。

1. 使用该凭据获取目标资源的客户端对象。 （每种类型的资源在 Azure 库中都有其自己的客户端对象，你可以向其提供资源的 URL。）

1. 尝试通过客户端对象访问或修改资源，该对象将生成对资源的 REST API 的 HTTP 请求。 API 调用是 Azure 随后对应用标识进行身份验证和检查授权的点。

以下代码通过尝试访问 Azure Key Vault 来描述和演示这些步骤。

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Acquire the resource URL. In this code we assume the resource URL is in an
# environment variable, KEY_VAULT_URL in this case.

vault_url = os.environ["KEY_VAULT_URL"]


# Acquire a credential object for the app identity. When running in the cloud,
# DefaultAzureCredential uses the app's managed identity or user-assigned service principal.
# When run locally, DefaultAzureCredential relies on environment variables named
# AZURE_CLIENT_ID, AZURE_CLIENT_SECRET, and AZURE_TENANT_ID.

credential = DefaultAzureCredential()


# Acquire an appropriate client object for the resource identified by the URL. The
# client object only stores the given credential at this point but does not attempt
# to authenticate it.
#
# **NOTE**: SecretClient here is only an example; the same process applies to all
# other Azure client libraries.

secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to perform an operation on the resource using the client object (in
# this case, retrieve a secret from Key Vault). The operation fails for any of
# the following reasons:
#
# 1. The information in the credential object is invalid (for example, the AZURE_CLIENT_ID
#    environment variable cannot be found).
# 2. The app identity cannot be authenticated using the information in the credential object.
# 3. The app identity is not authorized to perform the requested operation on the
#    resource (identified in this case by the vault_url.

retrieved_secret = secret_client.get_secret("secret-name-01")
```

同样，在代码通过客户端对象向 Azure REST API 发出特定请求之前，不会进行身份验证或授权。 用于创建 `DefaultAzureCredential` 的语句（请参阅下一节）仅在内存中创建一个客户端对象，但不执行其他检查。

创建 SDK [`SecretClient`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient) 对象也不涉及与相关资源的通信。 `SecretClient` 对象只是基础 Azure REST API 的包装器，仅存在于应用的运行时内存中。 

仅当代码调用 [`get_secret`](/python/api/azure-keyvault-secrets/azure.keyvault.secrets.secretclient#get-secret-name--version-none----kwargs-) 方法时，客户端对象才会生成对 Azure 的相应 REST API 调用。 然后，`get_secret` 的 Azure 终结点会对调用方的标识进行身份验证并检查授权。

## <a name="authenticate-with-defaultazurecredential"></a>使用 DefaultAzureCredential 进行身份验证

对于大多数应用程序，[`azure-identity`](/python/api/azure-identity/azure.identity) 库中的 [`DefaultAzureCredential`](/python/api/azure-identity/azure.identity.defaultazurecredential) 类都会提供建议使用的最简单的身份验证方法。 `DefaultAzureCredential` 在云中自动使用应用的托管标识，并在本地运行时自动从环境变量加载本地服务主体。

```python
import os
from azure.identity import DefaultAzureCredential
from azure.keyvault.secrets import SecretClient

# Acquire the resource URL
vault_url = os.environ["KEY_VAULT_URL"]

# Acquire a credential object
credential = DefaultAzureCredential()

# Acquire a client object
secret_client = SecretClient(vault_url=vault_url, credential=credential)

# Attempt to perform an operation
retrieved_secret = secret_client.get_secret("secret-name-01")
```

前面的代码在访问 Azure Key Vault 时使用 `DefaultAzureCredential` 对象，其中 Key Vault 的 URL 可在名为 `KEY_VAULT_URL` 的环境变量中获得。 此代码明确实现典型的库使用模式：获取凭据对象，为 Azure 资源创建适当的客户端对象，然后尝试使用该客户端对象对该资源执行操作。 同样，在最后一步之前不会进行身份验证和授权。

代码部署到 Azure 并在 Azure 上运行时，`DefaultAzureCredential` 会自动使用系统分配的托管标识，你可以在托管该标识的任何服务中为应用启用该标识。 使用 Azure 门户或 Azure CLI 将特定资源（例如，Azure 存储或 Azure Key Vault）的权限分配给该标识。 在这些情况下，此 Azure 托管的标识将最大限度地提高安全性，因为你从不在代码中处理显式服务主体。

在本地运行代码时，`DefaultAzureCredential` 会自动使用名为 `AZURE_TENANT_ID`、`AZURE_CLIENT_ID` 和 `AZURE_CLIENT_SECRET` 的环境变量所描述的服务主体。 然后，在调用 API 终结点时，此客户端对象会在 HTTP 请求标头中（以安全的方式）包含这些值。 在本地或云中运行时，无需更改任何代码。 有关创建服务主体和设置环境变量的详细信息，请参阅[为 Azure 配置本地 Python 开发环境 - 配置身份验证](configure-local-development-environment.md#configure-authentication)。

在这两种情况下，必须向所涉及的标识分配适当资源的权限。 [如何分配角色权限](/azure/role-based-access-control/role-assignments-steps)中介绍了常规流程；你可在各项服务的文档中找到具体信息。 要详细了解前面代码所需的 Key Vault 权限，请参阅[使用访问控制策略提供 Key Vault 身份验证](/azure/key-vault/general/group-permissions-for-apps)。

### <a name="using-defaultazurecredential-with-sdk-management-libraries"></a>将 DefaultAzureCredential 与 SDK 管理库配合使用

`DefaultAzureCredential` 适用于[使用 azure.core 的库](azure-sdk-library-package-index.md#libraries-using-azurecore)列表上显示的 Azure SDK 管理库的版本（即名称中带有“mgmt”的版本）。 （此外，已更新的库的 pypi 页面还包含“凭据系统进行了彻底更新和优化”这一行内容，以指示所作更改。）

例如，可将 `DefaultAzureCredential` 与 azure-mgmt-resource 的版本 15.0.0 或更高版本结合使用：

```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.resource import SubscriptionClient

credential = DefaultAzureCredential()
subscription_client = SubscriptionClient(credential)

sub_list = subscription_client.subscriptions.list()
print(list(sub_list))
```

### <a name="defaultazurecredential-object-has-no-attribute-signed-session"></a>“DefaultAzureCredential 对象没有 signed-session 特性”

如果尝试将 `DefaultAzureCredential` 用于尚未更新为使用 azure.core 的库，那么通过客户端对象的调用会失败，出现信息相对模糊的错误：“DefaultAzureCredential 对象没有 signed-session 特性”。 例如，如果将上一部分中的代码与低于版本 15 的 azure-mgmt-resource 库结合使用，就会遇到此类错误。

出现此错误的原因是，SDK 管理库的非 azure.core 版本假定凭据对象包含 `DefaultAzureCredential` 缺少的 `signed_session` 属性。

如果要使用的管理库尚未更新，可以使用以下替代方法：

1. 使用本文后续部分中所述的其他身份验证方法之一，这些方法非常适用于仅使用 SDK 管理库且不会部署到云的代码，在这种情况下，你只能依赖于本地服务主体。

1. 使用 Azure SDK 工程团队成员提供的 [CredentialWrapper 类 (cred_wrapper.py)](https://gist.github.com/lmazuel/cc683d82ea1d7b40208de7c9fc8de59d)，而不是 `DefaultAzureCredential`。 当所需的管理库可用后，请切换回 `DefaultAzureCredential`。 此方法的优点是，可以将同一凭据同时用于 SDK 客户端和管理库，并且它在本地和云中都有效。

    假定已将 cred_wrapper.py 的副本下载到项目文件夹中，则前面的代码将如下所示：

    ```python
    from cred_wrapper import CredentialWrapper
    from azure.mgmt.resource import SubscriptionClient

    credential = CredentialWrapper()
    subscription_client = SubscriptionClient(credential)
    subscription = next(subscription_client.subscriptions.list())
    print(subscription.subscription_id)
    ```

    同样，当更新的管理库可用后，可直接使用 `DefaultAzureCredential`，如原始代码示例中所示。

## <a name="other-authentication-methods"></a>其他身份验证方法

尽管 `DefaultAzureCredential` 是大多数情况下建议使用的身份验证方法，但其他方法也可用，有以下注意事项：

- 大多数方法适用于显式服务主体，并且不会利用部署到云的代码的托管标识。 与生产代码一起使用时，必须为云应用程序管理和维护不同的服务主体。

- 某些方法（例如基于 CLI 的身份验证）仅适用于本地脚本，不能与生产代码一起使用。 对于开发工作来说，基于 CLI 的身份验证非常便捷，因为它使用 Azure 登录的权限，不需要显式角色分配。

部署到云的应用程序的服务主体在 Active Directory 订阅中进行管理。 有关详细信息，请参阅[如何管理服务主体](how-to-manage-service-principals.md)。

在所有情况下，相应的服务主体或用户必须对相关资源和操作具有适当的权限。

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
    > 如[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述，可以将 [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac) 命令与 `--sdk-auth` 参数一起使用，以直接生成此 JSON 格式。

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

1. 使用 [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory#get-client-from-auth-file-client-class--auth-path-none----kwargs-) 方法来创建客户端对象：

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

如上一部分所述，可以通过变量生成必要的 JSON 数据并调用 [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory#get-client-from-json-dict-client-class--config-dict----kwargs-)，而不是使用文件。 此代码假定已创建[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述的环境变量。 对于部署到云的代码，可以在服务器 VM 上创建这些环境变量，也可以在使用平台服务（如 Azure 应用服务和 Azure Functions）时将这些环境变量用作应用程序设置。

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

在此方法中，将使用从安全存储（例如，Azure Key Vault 或环境变量）中获取的凭据创建 [`ServicePrincipalCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.serviceprincipalcredentials) 对象。 前面的代码假定已创建[配置本地开发环境](configure-local-development-environment.md#create-a-service-principal-and-environment-variables-for-development)中所述的环境变量。

使用此方法，可以通过为客户端对象指定 `base_url` 参数来使用 [Azure 主权或国家云](/azure/active-directory/develop/authentication-national-cloud)，而不是 Azure 公有云：

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

#...

subscription_client = SubscriptionClient(credentials, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```

你可在 [msrestazure.azure_cloud 库](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py)中找到主权云常量。

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

如果需要 ADAL 库，请运行 `pip install adal`。

使用此方法，可以使用 [Azure 主权或国家云](/azure/active-directory/develop/authentication-national-cloud)，而不是 Azure 公有云。

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD

# ...

LOGIN_ENDPOINT = AZURE_CHINA_CLOUD.endpoints.active_directory
RESOURCE = AZURE_CHINA_CLOUD.endpoints.active_directory_resource_id
```

只需将 `AZURE_PUBLIC_CLOUD` 替换为 [msrestazure.azure_cloud 库](https://github.com/Azure/msrestazure-for-python/blob/master/msrestazure/azure_cloud.py)中的相应主权云常量。

### <a name="cli-based-authentication-development-purposes-only"></a>基于 CLI 的身份验证（仅限开发目的）

在此方法中，将使用通过 Azure CLI 命令 `az login` 登录的用户的凭据创建客户端对象。 基于 CLI 的身份验证仅适用于开发目的，因为无法在生产环境中使用它。

Azure 库默认的订阅 ID，你也可使用 [`az account`](/cli/azure/manage-azure-subscriptions-azure-cli) 在运行代码之前设置订阅。

使用基于 CLI 的身份验证时，会针对 CLI 登录凭据允许的所有操作向应用程序授权。 结果是，如果你是订阅的所有者或管理员，那么无需分配任何特定权限，你的代码就可访问该订阅中的大多数资源。 此行为对于试验非常方便。 但是，强烈建议你在开始编写生产代码时使用特定服务主体并分配特定权限，因为你会了解如何将确切的权限分配给不同的标识，而且在部署到生产环境之前，可在测试环境中准确验证这些权限。

#### <a name="cli-based-authentication-with-azurecore-libraries"></a>使用 azure.core 库的基于 CLI 的身份验证

使用[已为 azure.core 更新的 Azure 库](/azure/developer/python/azure-sdk-library-package-index#libraries-using-azurecore)时，请使用 azure-identity 库（版本 1.4.0+）中的 [`AzureCliCredential`](/python/api/azure-identity/azure.identity.azureclicredential) 对象。 例如，可将以下代码用于 azure-mgmt-resource 版本 15.0.0+：

```python
from azure.identity import AzureCliCredential
from azure.mgmt.resource import SubscriptionClient

credential = AzureCliCredential()
subscription_client = SubscriptionClient(credential)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

#### <a name="cli-based-authentication-with-older-non-azurecore-libraries"></a>使用更旧的（非 azure.core）库的基于 CLI 的身份验证

使用未针对 azure.core 更新的更旧的 Azure 库时，可使用 azure-cli-core 库中的 [`get_client_from_cli_profile`](/python/api/azure-common/azure.common.client_factory#get-client-from-cli-profile-client-class----kwargs-) 方法。 例如，可将以下代码用于版本低于 15.0.0 的 azure-mgmt-resource：

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import SubscriptionClient

subscription_client = get_client_from_cli_profile(SubscriptionClient)

subscription = next(subscription_client.subscriptions.list())
print(subscription.subscription_id)
```

如需引用同一脚本中的不同订阅，请使用本文前面介绍的 ['get_client_from_auth_file'](#authenticate-with-a-json-file) 或 [`get_client_from_json_dict`](#authenticate-with-a-json-dictionary) 方法。

### <a name="deprecated-authenticate-with-userpasscredentials"></a>不推荐使用：使用 UserPassCredentials 进行身份验证

在[适用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 发布之前，必须使用现已弃用的 [`UserPassCredentials`](/python/api/msrestazure/msrestazure.azure_active_directory.userpasscredentials) 类。 此类不支持双因素身份验证，不应再使用。

## <a name="see-also"></a>另请参阅

- [为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)
- [如何分配角色权限](/azure/role-based-access-control/role-assignments-steps)
- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配和使用 Azure 存储](azure-sdk-example-storage.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和使用 MySQL 数据库](azure-sdk-example-database.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
