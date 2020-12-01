---
title: 使用用于 Python 的 Azure 库连接到所有区域 - 多云
description: 如何使用 msrestazure 的 azure_cloud 模块连接到不同主权区域中的 Azure
ms.date: 11/18/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: ba751604351bd7038a7696b6c8e93d663aa9263f
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485666"
---
# <a name="multi-cloud-connect-to-all-regions-with-the-azure-libraries-for-python"></a>多云：使用用于 Python 的 Azure 库连接到所有区域

可使用用于 Python 的 Azure 库连接到所有[提供](https://azure.microsoft.com/regions/services) Azure 的区域。

默认情况下，这些 Azure 库配置为连接到全局 Azure 云。

## <a name="using-pre-defined-sovereign-cloud-constants"></a>使用预定义的主权云常量

预定义的主权云常量由 `msrestazure` 库 (0.4.11+) 的 `azure_cloud` 模块提供：

- `AZURE_PUBLIC_CLOUD`
- `AZURE_CHINA_CLOUD`
- `AZURE_US_GOV_CLOUD`
- `AZURE_GERMAN_CLOUD`

若要使用某定义，请从 `msrestazure.azure_cloud` 导入相应常量，然后在创建客户端对象时应用该常量。 

使用 `DefaultAzureCredential` 时，如以下示例中所示，还需要使用 `azure.identity.AzureAuthorityHosts` 中相应的值。

```python
import os
from msrestazure.azure_cloud import AZURE_CHINA_CLOUD as cloud
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential

# Assumes the subscription ID to use is in the AZURE_SUBSCRIPTION_ID environment variable
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

# When using sovereign domains (that is, any cloud other than AZURE_PUBLIC_CLOUD),
# you must use an authority with DefaultAzureCredential.
credential = DefaultAzureCredential(authority=cloud.endpoints.active_directory)

resource_client = ResourceManagementClient(credential,
    subscription_id, base_url=cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])

subscription_client = SubscriptionClient(credential,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])
```
  
## <a name="using-your-own-cloud-definition"></a>使用你自己的云定义

以下代码将 `get_cloud_from_metadata_endpoint` 与私有云的 Azure 资源管理器终结点（如基于 Azure Stack 构建的终结点）配合使用：

```python
import os
from msrestazure.azure_cloud import get_cloud_from_metadata_endpoint
from azure.mgmt.resource import ResourceManagementClient, SubscriptionClient
from azure.identity import DefaultAzureCredential

# Assumes the subscription ID to use is in the AZURE_SUBSCRIPTION_ID environment variable
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]

stack_cloud = get_cloud_from_metadata_endpoint("https://contoso-azurestack-arm-endpoint.com")

# When using a private, you must use an authority with DefaultAzureCredential.
# The active_directory endpoint should be a URL like https://login.microsoftonline.com.
# https:// is optional in the URL but required on the endpoint.
credential = DefaultAzureCredential(authority=stack_cloud.endpoints.active_directory)

resource_client = ResourceManagementClient(credential, subscription_id,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[cloud.endpoints.resource_manager + ".default'"])

subscription_client = SubscriptionClient(credential,
    base_url=stack_cloud.endpoints.resource_manager,
    credential_scopes=[stack_cloud.endpoints.resource_manager + ".default'"])
```
