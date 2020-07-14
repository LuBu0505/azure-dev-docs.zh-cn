---
author: judubois
ms.date: 05/18/2020
ms.author: judubois
ms.openlocfilehash: b1dd65024158f622d6b202dc2ad83a31f8d98296
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507687"
---
## <a name="prepare-the-working-environment"></a>准备工作环境

首先，使用以下命令设置一些环境变量：

```bash
AZ_RESOURCE_GROUP=database-workshop
AZ_DATABASE_NAME=<YOUR_DATABASE_NAME>
AZ_LOCATION=<YOUR_AZURE_REGION>
AZ_SQL_SERVER_USERNAME=spring
AZ_SQL_SERVER_PASSWORD=<YOUR_AZURE_SQL_PASSWORD>
AZ_LOCAL_IP_ADDRESS=<YOUR_LOCAL_IP_ADDRESS>
```

使用以下值替换占位符，在本文中将使用这些值：

- `<YOUR_DATABASE_NAME>`：Azure SQL 数据库服务器的名称。 它在 Azure 中应是唯一的。
- `<YOUR_AZURE_REGION>`：将使用的 Azure 区域。 默认情况下可以使用 `eastus`，但我们建议你配置一个离居住位置更近的区域。 可输入 `az account list-locations`，获取可用区域的完整列表。
- `<AZ_SQL_SERVER_PASSWORD>`：Azure SQL 数据库服务器的密码。 该密码应该至少有八个字符。 这些字符应该属于以下类别中的三个类别：英文大写字母、英文小写字母、数字 (0-9)和非字母数字字符（!, $, #, % 等）。
- `<YOUR_LOCAL_IP_ADDRESS>`：你将在其中运行 Spring Boot 应用程序的本地计算机的 IP 地址。 若要找到该地址，一种简便方法是使浏览器指向 [whatismyip.akamai.com](http://whatismyip.akamai.com/)。

接下来，使用以下命令创建资源组：

```azurecli
az group create \
    --name $AZ_RESOURCE_GROUP \
    --location $AZ_LOCATION \
    | jq
```

> [!NOTE]
> 我们使用 `jq` 实用工具来显示 JSON 数据，使其更易于阅读。 默认情况下，此实用工具安装在 [Azure Cloud Shell](https://shell.azure.com/) 上。 如果你不喜欢该实用工具，可安全地删除要使用的所有命令的 `| jq` 部分。

## <a name="create-an-azure-sql-database-instance"></a>创建 Azure SQL 数据库实例

首先，我们将创建一个托管的 Azure SQL 数据库服务器。

> [!NOTE]
> 可以在[快速入门：创建 Azure SQL 数据库单一数据库](/azure/sql-database/sql-database-single-database-get-started)中阅读有关创建 Azure SQL 数据库服务器的更多详细信息。

在 [Azure Cloud Shell](https://shell.azure.com/) 中运行以下命令：

```azurecli
az sql server create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME \
    --location $AZ_LOCATION \
    --admin-user $AZ_SQL_SERVER_USERNAME \
    --admin-password $AZ_SQL_SERVER_PASSWORD \
    | jq
```

此命令创建 Azure SQL 数据库服务器。

### <a name="configure-a-firewall-rule-for-your-azure-sql-database-server"></a>为 Azure SQL 数据库服务器配置防火墙规则

Azure SQL 数据库实例在默认情况下受保护。 它们有不允许任何传入连接的防火墙。 为了能够使用数据库，需要添加一项防火墙规则，允许本地 IP 地址访问数据库服务器。

由于我们已在本文开头配置了本地 IP 地址，因此你可通过运行以下命令来打开服务器的防火墙：

```azurecli
az sql server firewall-rule create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name $AZ_DATABASE_NAME-database-allow-local-ip \
    --server $AZ_DATABASE_NAME \
    --start-ip-address $AZ_LOCAL_IP_ADDRESS \
    --end-ip-address $AZ_LOCAL_IP_ADDRESS \
    | jq
```

### <a name="configure-a-azure-sql-database"></a>配置 Azure SQL 数据库

你在前面创建的 Azure SQL 数据库服务器为空。 它没有任何可以与 Spring Boot 应用程序配合使用的数据库。 通过运行以下命令创建名为 `demo` 的新数据库：

```azurecli
az sql db create \
    --resource-group $AZ_RESOURCE_GROUP \
    --name demo \
    --server $AZ_DATABASE_NAME \
    | jq
```