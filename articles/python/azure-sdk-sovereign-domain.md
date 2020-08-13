---
title: 使用用于 Python 的 Azure 库连接到所有区域 - 多云
description: 如何使用 msrestazure 的 azure_cloud 模块连接到不同主权区域中的 Azure
ms.date: 07/13/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: caf3f90fb9d21535dd6bf8974a6bdc719f3758ab
ms.sourcegitcommit: 980efe813d1f86e7e00929a0a3e1de83514ad7eb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87983173"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-libraries-for-python"></a>多云：使用用于 Python 的 Azure 库连接到所有区域

可使用用于 Python 的 Azure 库连接到所有[提供](https://azure.microsoft.com/regions/services) Azure 的区域。

默认情况下，这些 Azure 库配置为连接到全局 Azure。

## <a name="using-pre-defined-sovereign-cloud-constants"></a>使用预定义的主权云常量

预定义的主权云常量由 `msrestazure` (0.4.11+) 的 `azure_cloud` 模块提供：

- `AZURE_PUBLIC_CLOUD`
- `AZURE_CHINA_CLOUD`
- `AZURE_US_GOV_CLOUD`
- `AZURE_GERMAN_CLOUD`

若要在你的所有代码中应用某个常量，请使用上一列表中的一个值来定义名为 `AZURE_CLOUD` 的环境变量。 （`AZURE_PUBLIC_CLOUD` 是默认值。）

若要在特定操作内应用常量，请从 `msrest.azure_cloud` 导入所需的常量，并在创建凭据和客户端对象时使用它：

```python
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

credentials = UserPassCredentials(login, password,
    cloud_environment=AZURE_CHINA_CLOUD)

client = ResourceManagementClient(credentials,
    subscription_id, base_url=AZURE_CHINA_CLOUD.endpoints.resource_manager)
```
  
## <a name="using-your-own-cloud-definition"></a>使用你自己的云定义

以下代码将 `get_cloud_from_metadata_endpoint` 与私有云的 Azure 资源管理器终结点（如基于 Azure Stack 构建的终结点）配合使用：

```python
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from msrestazure.azure_active_directory import UserPassCredentials
from azure.mgmt.resource import ResourceManagementClient

stack_cloud = get_cloud_from_metadata_endpoint("https://contoso-azurestack-arm-endpoint.com")
credentials = UserPassCredentials(login, password,
    cloud_environment=stack_cloud)

client = ResourceManagementClient(credentials, subscription_id,
    base_url=stack_cloud.endpoints.resource_manager)
```

## <a name="using-adal"></a>使用 ADAL

连接到另一个区域时，请考虑以下问题：

- 用于请求令牌（身份验证）的终结点是什么？
- 我要在其中使用此令牌（使用情况）的终结点是什么？

下面是一个一般性示例：

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Public Azure - default values
authentication_endpoint = 'https://login.microsoftonline.com/'
azure_endpoint = 'https://management.azure.com/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-government"></a>Azure Government

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Government
authentication_endpoint = 'https://login.microsoftonline.us/'
azure_endpoint = 'https://management.usgovcloudapi.net/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-germany"></a>Azure 德国

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure Germany
authentication_endpoint = 'https://login.microsoftonline.de/'
azure_endpoint = 'https://management.microsoftazure.de/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```

### <a name="azure-china-21vianet"></a>Azure 中国世纪互联

```python
import adal
from msrestazure.azure_active_directory import AdalAuthentication
from azure.mgmt.resource import ResourceManagementClient

# Service Principal
tenant = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
client_id = 'ABCDEFGH-1234-1234-1234-ABCDEFGHIJKL'
password = 'password'

# Azure China
authentication_endpoint = 'https://login.chinacloudapi.cn/'
azure_endpoint = 'https://management.chinacloudapi.cn/'

context = adal.AuthenticationContext(authentication_endpoint+tenant)
credentials = AdalAuthentication(context.acquire_token_with_client_credentials,
    azure_endpoint, client_id, password)

subscription_id = '33333333-3333-3333-3333-333333333333'

resource_client = ResourceManagementClient(credentials,
    subscription_id, base_url=azure_endpoint)
```
