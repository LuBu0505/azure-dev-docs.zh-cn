---
title: 使用用于 Python 的 Azure 管理库对应用进行身份验证
description: 使用 Azure 管理 SDK 库通过 Azure 服务对 Python 应用进行身份验证
ms.date: 01/16/2020
ms.topic: conceptual
ms.custom: seo-python-october2019
ms.openlocfilehash: e972d0159f97feddf4dd773d69b422634c3966c1
ms.sourcegitcommit: 1bd9ec6a4115e9162e33b76a933869788e6ab702
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2020
ms.locfileid: "80441792"
---
# <a name="authenticate-by-using-the-azure-management-libraries-for-python"></a>使用用于 Python 的 Azure 管理库进行身份验证

本文介绍如何使用 Python SDK 管理库通过服务主体借助 Azure Active Directory (Azure AD) 对应用程序进行身份验证。 服务主体是在 Azure AD 中注册的应用程序的标识，它允许应用程序根据其权限访问或修改资源。

若要注册应用程序，必须首先使用适用于组织的租户创建 Active Directory。 可以按照[在 Azure Active Directory 中创建新租户](/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)中的说明来这样操作。 Active Directory 准备就绪后，请按[操作方法：使用门户创建可访问资源的 Azure AD 应用程序和服务主体](/azure/active-directory/develop/howto-create-service-principal-portal)一文的说明操作，以便注册应用程序、[检索服务主体的租户和应用程序（客户端）ID](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in)，以及设置[应用程序机密](/azure/active-directory/develop/howto-create-service-principal-portal#create-a-new-application-secret)，从而通过 Python 代码进行身份验证。

获得这些值后，可以使用这些凭据通过 Python SDK 库以多种方式进行身份验证。 每个方法的结果是从代码访问其他资源时使用的 SDK 客户端对象。

强烈建议在 [Azure KeyVault](/azure/key-vault/) 中存储租户 ID、客户端 ID 和机密，使这些值不会出现在系统或源代码管理中的任何位置。 只要需要，就可以轻松地检索这些值。

[!INCLUDE [chrome-note](includes/chrome-note.md)]

## <a name="authenticate-with-a-json-file"></a><a name="mgmt-auth-file"></a>使用 JSON 文件进行身份验证

在此方法中，我们创建一个 JSON 文件（其中包含服务主体所需的凭据），然后根据该文件中的信息创建 SDK 客户端对象。

1. 使用以下格式创建 JSON 文件（名称可随意，例如 *app_credentials.json*）。 将四个占位符替换为你的 Azure 订阅 ID、Azure AD 租户 ID、应用程序（客户端）ID 和机密：

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

    > [!TIP]
    > 可以先使用 [az login](/cli/azure/reference-index#az-login) 命令登录到 Azure，然后使用 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令，以便使用已准备到位的订阅 ID 检索凭据文件：
    >
    > ```azurecli
    > az login
    > az ad sp create-for-rbac --sdk-auth > credentials.json
    > ```
    >
    > 然后，可以替换特定应用程序的 `tenantId`、`clientId` 和 `clientSecret` 值，而不是使用常规用途值。

1. 将此文件保存在可供代码访问的安全位置。

1. 使用 [get_client_from_auth_file](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-auth-file-client-class--auth-path-none----kwargs-) 方法来创建客户端对象，并将 `<path_to_file>` 替换为 JSON 文件的路径：

    ```python
    from azure.common.client_factory import get_client_from_auth_file
    from azure.mgmt.compute import ComputeManagementClient

    client = get_client_from_auth_file(ComputeManagementClient, auth_path=<path_to_file>)
    ```

1. 也可在名为 `AZURE_AUTH_LOCATION` 的环境变量中存储文件的路径，省略 `auth_path` 参数。

## <a name="authenticate-with-a-json-dictionary"></a>使用 JSON 字典进行身份验证

如上一部分所述，可以在变量中生成必要的 JSON 并改为调用 [get_client_from_json_dict](/python/api/azure-common/azure.common.client_factory?view=azure-python#get-client-from-json-dict-client-class--config-dict----kwargs-)，而不是使用文件。 在这种情况下，应始终将租户 ID、客户端 ID 和机密存储在安全位置，如 [Azure KeyVault](/azure/key-vault/)。

```python
   from azure.common.client_factory import get_client_from_auth_file
   from azure.mgmt.compute import ComputeManagementClient

    # Retrieve tenant_id, client_id, and client_secret from Azure KeyVault

   config_dict = {
       "subscriptionId": "bfc42d3a-65ca-11e7-95cf-ecb1d756380e",
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
   client = get_client_from_json_dict(ComputeManagementClient, config_dict)
```

## <a name="authenticate-with-token-credentials"></a><a name="mgmt-auth-token"></a>使用令牌凭据进行身份验证

假设你从安全存储（例如 [Azure KeyVault](/azure/key-vault/)）检索凭据，请先创建一个 [ServicePrincipalCredentials] 对象，然后使用这些凭据和订阅 ID 创建客户端的实例：

```python
from azure.mgmt.compute import ComputeManagementClient
from azure.common.credentials import ServicePrincipalCredentials

# Retrieve credentials from secure storage. Never hard-code credentials into code.

credentials = ServicePrincipalCredentials(tenant = <tenant_id>,
    client_id = <client_id>, secret = <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

如果需要更多的控制，请使用[适用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 和 SDK ADAL 包装器：

```python
from azure.mgmt.compute import ComputeManagementClient
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD

# Retrieve credentials from secure storage. Never hard-code credentials into code.

LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)

credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    RESOURCE, <client_id>, <secret>)

client = ComputeManagementClient(credentials, <subscription_id>)
```

> [!NOTE]
> 如果使用 Azure 主权云，请在创建管理客户端时指定相应的基 URL（使用 `msrestazure.azure_cloud` 中的常量）：
>
> ```python
> client = ComputeManagementClient(credentials, subscription_id,
>     base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
> ```

### <a name="authenticate-with-token-credentials-deprecated"></a><a name="mgmt-auth-legacy"></a>使用令牌凭据进行身份验证（已弃用）

在[适用于 Python 的 Azure Active Directory 身份验证库 (ADAL)](https://github.com/AzureAD/azure-activedirectory-library-for-python) 发布之前，你使用 `UserPassCredentials` 类。 此类被视为已过时，不应继续使用，因为它不支持双重身份验证。

```python
from azure.common.credentials import UserPassCredentials

# DEPRECATED - legacy purposes only - use ADAL instead
credentials = UserPassCredentials(
    'user@domain.com',
    'my_smart_password'
)
```

## <a name="authenticate-with-azure-managed-identities"></a><a name="mgmt-auth-msi"></a>使用 Azure 托管标识进行身份验证

Azure 中的资源可以通过 Azure 托管标识这种简单的方式进行身份验证，无需使用特定凭据。

若要使用托管标识，必须从另一 Azure 资源（例如某个 Azure 函数或虚拟机）连接到 Azure。 若要了解如何为资源配置托管标识，请参阅[配置 Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)和[如何使用 Azure 资源的托管标识](/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)。

```python
from msrestazure.azure_active_directory import MSIAuthentication
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient

# Create MSI Authentication
credentials = MSIAuthentication()

# Create a Subscription Client
subscription_client = SubscriptionClient(credentials)
subscription = next(subscription_client.subscriptions.list())
subscription_id = subscription.subscription_id

# Create a Resource Management client
client = ResourceManagementClient(credentials, subscription_id)

# List resource groups as an example. The only limit is what role and policy are assigned to this MSI token.
for resource_group in client.resource_groups.list():
    print(resource_group.name)
```

## <a name="cli-based-authentication-development-purposes-only"></a><a name="mgmt-auth-cli"></a>基于 CLI 的身份验证（仅限开发目的）

在你运行 `az login` 后，SDK 即可使用 Azure CLI 的活动订阅创建客户端。 可以让 SDK 使用默认订阅 ID，也可以使用 [az account](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli) 来设置订阅

此选项只应该用于开发目的。

```python
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.compute import ComputeManagementClient

client = get_client_from_cli_profile(ComputeManagementClient)
```
