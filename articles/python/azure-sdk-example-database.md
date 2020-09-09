---
title: 使用 Azure SDK 库预配 Azure MySQL 数据库
description: 使用用于 Python 的 Azure SDK 库中的管理库来预配 Azure MySQL、PostgresSQL 或 MariaDB 数据库。
ms.date: 06/02/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e9a08761fb9af300b5d3f2c4a9704bc7f10e1158
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275120"
---
# <a name="example-use-the-azure-libraries-to-provision-a-database"></a>示例：使用 Azure 库预配数据库

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库来预配 Azure MySQL 数据库。 它还提供了一个简单的脚本，用于使用 mysql-connector 库（不是 Azure SDK 的一部分）查询数据库。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。 如果你想要使用 Azure 门户，请参阅[创建 PostgreSQL 服务器](/azure/postgresql/quickstart-create-server-database-portal)或[创建 MariaDB 服务器](/azure/mariadb/quickstart-create-mariadb-server-database-using-azure-portal)。）

可以使用类似的代码来预配 PostgreSQL 或 MariaDB 数据库。

除非注明，否则本文中的所有命令在 Linux/Mac OS bash 和 Windows 命令 shell 中的工作方式相同。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-needed-azure-library-packages"></a>2:安装所需的 Azure 库包

创建一个具有以下内容的名为 *requirements.txt* 的文件：

```text
azure-mgmt-resource
azure-mgmt-rdbms
azure-cli-core
mysql
mysql-connector
```

在激活了虚拟环境的终端或命令提示符下，安装下列要求：

```cmd
pip install -r requirements.txt
```

> [!NOTE]
> 在 Windows 上，尝试将 mysql 库安装到一个 32 位的 Python 库会产生有关 mysql.h 文件的错误。 在这种情况下，请安装 64 位版本的 Python，然后重试。

## <a name="3-write-code-to-provision-the-database"></a>3：编写代码来预配数据库

创建名为“provision_db.py”的 Python 文件，其中包含以下代码。 注释提供了详细信息。

```python
import random, os
from azure.common.client_factory import get_client_from_cli_profile
from azure.mgmt.resource import ResourceManagementClient

# For PostgreSQL, use the PostgreSQLManagement class.
# For MariaDB, use the MariaDBManagement class.
from azure.mgmt.rdbms.mysql import MySQLManagementClient

from azure.mgmt.rdbms.mysql.models import ServerForCreate, ServerPropertiesForDefaultCreate, ServerVersion

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = 'PythonAzureExample-DB-rg'
LOCATION = "westus"

# Step 1: Provision the resource group.
resource_client = get_client_from_cli_profile(ResourceManagementClient)

rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    { "location": LOCATION })

print(f"Provisioned resource group {rg_result.name}")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: Provision the database server

# We use a random number to create a reasonably unique database server name.
# If you've already provisioned a database and need to re-run the script, set
# the DB_SERVER_NAME environment variable to that name instead.
#
# Also set DB_USER_NAME and DB_USER_PASSWORD variables to avoid using the defaults.

db_server_name = os.environ.get("DB_SERVER_NAME", f"PythonAzureExample-MySQL-{random.randint(1,100000):05}")
db_admin_name = os.environ.get("DB_ADMIN_NAME", "azureuser")
db_admin_password = os.environ.get("DB_ADMIN_PASSWORD", "ChangePa$$w0rd24")

# Obtain the management client object
mysql_client = get_client_from_cli_profile(MySQLManagementClient)

# Provision the server and wait for the result
poller = mysql_client.servers.create(RESOURCE_GROUP_NAME,
    db_server_name,
    ServerForCreate(
        location=LOCATION,
        properties=ServerPropertiesForDefaultCreate(
            administrator_login=db_admin_name,
            administrator_login_password=db_admin_password,
            version=ServerVersion.five_full_stop_seven
        )
    )
)

server = poller.result()

print(f"Provisioned MySQL server {server.name}")

# Step 3: Provision a firewall rule to allow the local workstation to connect

RULE_NAME = "allow_ip"
ip_address = os.environ["PUBLIC_IP_ADDRESS"]

# For the above code, create an environment variable named PUBLIC_IP_ADDRESS that
# contains your workstation's public IP address as reported by a site like
# https://whatismyipaddress.com/.

# Provision the rule and wait for completion
poller = mysql_client.firewall_rules.create_or_update(RESOURCE_GROUP_NAME,
    db_server_name, RULE_NAME,
    ip_address,  # Start ip range
    ip_address   # End ip range
)

firewall_rule = poller.result()

print(f"Provisioned firewall rule {firewall_rule.name}")

# Step 4: Provision a database on the server

DB_NAME = "example-db1"

poller = mysql_client.databases.create_or_update(RESOURCE_GROUP_NAME,
    db_server_name, DB_NAME)

db_result = poller.result()

print(f"Provisioned MySQL database {db_result.name} with ID {db_result.id}")
```

要使此示例可以运行，必须创建一个名为 `PUBLIC_IP_ADDRESS` 且包含你的工作站 IP 地址的环境变量。

此代码使用基于 CLI 的身份验证方法 (`get_client_from_cli_profile`)，因为它演示了你可能会使用 Azure CLI 直接执行的操作。 在这两种情况下，使用相同的身份验证标识。

若要在生产脚本中使用此类代码，应改为使用 `DefaultAzureCredential`（推荐）或基于服务主体的方法，如[如何使用 Azure 服务对 Python 应用进行身份验证](azure-sdk-authenticate.md)中所述。

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient?view=azure-python)
- [MySQLManagementClient (azure.mgmt.rdbms.mysql)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.mysqlmanagementclient?view=azure-python)
- [ServerForCreate (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverforcreate?view=azure-python)
- [ServerPropertiesForDefaultCreate (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverpropertiesfordefaultcreate?view=azure-python)
- [ServerVersion (azure.mgmt.rdbms.mysql.models)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mysql.models.serverversion?view=azure-python)

另请参阅：
    - [PostgreSQLManagementClient (azure.mgmt.rdbms.postgresql)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.postgresql.postgresqlmanagementclient?view=azure-python)
    - [MariaDBManagementClient (azure.mgmt.rdbms.mariadb)](/python/api/azure-mgmt-rdbms/azure.mgmt.rdbms.mariadb.mariadbmanagementclient?view=azure-python)

## <a name="4-run-the-script"></a>4：运行脚本

```cmd
python provision_db.py
```

## <a name="5-insert-a-record-and-query-the-database"></a>5：插入记录并查询数据库

使用以下代码创建一个名为“use_db.py”的文件。 请注意 `DB_SERVER_NAME`、`DB_ADMIN_NAME` 和 `DB_ADMIN_PASSWORD` 环境变量的依赖项，这些环境变量应使用来自预配代码的值进行填充。

此代码仅适用于 MySQL；对于 PostgreSQL 和 MariaDB，需要使用不同的库。

```python
import os
import mysql.connector

db_server_name = os.environ["DB_SERVER_NAME"]
db_admin_name = os.environ.get("DB_ADMIN_NAME", "azureuser")
db_admin_password = os.environ.get("DB_ADMIN_PASSWORD", "ChangePa$$w0rd24")

DB_NAME = "example-db"

connection = mysql.connector.connect(user=f"{db_admin_name}@{db_server_name}",
    password=db_admin_password, host=f"{db_server_name}.mysql.database.azure.com", port=3306,
    database=DB_NAME)

cursor = connection.cursor()

table_name = "ExampleTable1"

sql_create = f"CREATE TABLE {table_name} (name varchar(255), code int)"

cursor.execute(sql_create)
print(f"Successfully created table {table_name}")

sql_insert = f"INSERT INTO {table_name} (name, code) VALUES ('Azure', 1)"
insert_data = "('Azure', 1)"

cursor.execute(sql_insert)
print("Successfully inserted data into table")

sql_select_values= f"SELECT * FROM {table_name}"

cursor.execute(sql_select_values)
row = cursor.fetchone()

while row:
    print(str(row[0]) + " " + str(row[1]))
    row = cursor.fetchone()

connection.commit()
```

所有这些代码均使用 mysql.connector API。 唯一的特定于 Azure 的部分是 MySQL 服务器的完整主机域 (mysql.database.azure.com)。

然后运行代码：

```cmd
python use_db.py
```

## <a name="6-clean-up-resources"></a>6：清理资源

```azurecli
az group delete -n PythonAzureExample-DB-rg
```

如果不需要保留预配在此示例中的资源，并想要避免订阅中的持续费用，则运行此命令。

你还可以使用 [`ResourceManagementClient.resource_groups.delete`](/python/api/azure-mgmt-resource/azure.mgmt.resource.resources.v2019_10_01.operations.resourcegroupsoperations?view=azure-python#delete-resource-group-name--custom-headers-none--raw-false--polling-true----operation-config-) 方法从代码中删除资源组。

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

以下 Azure CLI 命令完成了与 Python 脚本相同的预配步骤。 对于 PostgreSQL 数据库，请使用 [`az postgres`](/cli/azure/postgres?view=azure-cli-latest) 命令；对于 MariaDB，请使用 [`az mariadb`](/cli/azure/mariadb?view=azure-cli-latest) 命令。

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
az group create -l centralus -n PythonAzureExample-DB-rg

az mysql server create -l westus -g PythonAzureExample-DB-rg -n PythonAzureExample-MySQL-12345 ^
    -u azureuser -p ChangePa$$w0rd24 --sku-name B_Gen5_1

# Change the IP address to the public IP address of your workstation, that is, the address shown
# by a site like https://whatismyipaddress.com/. 

az mysql server firewall-rule create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 ^
    -n allow_ip --start-ip-address 10.11.12.13 --end-ip-address 10.11.12.13

az mysql db create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 -n example-db
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
az group create -l centralus -n PythonAzureExample-DB-rg

az mysql server create -l westus -g PythonAzureExample-DB-rg -n PythonAzureExample-MySQL-12345 \
    -u azureuser -p ChangePa$$w0rd24 --sku-name B_Gen5_1

# Change the IP address to the public IP address of your workstation, that is, the address shown
# by a site like https://whatismyipaddress.com/. 

az mysql server firewall-rule create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 \
    -n allow_ip --start-ip-address 10.11.12.13 --end-ip-address 10.11.12.13

az mysql db create -g PythonAzureExample-DB-rg --server PythonAzureExample-MySQL-12345 -n example-db
```

---

## <a name="see-also"></a>另请参阅

- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配虚拟机](azure-sdk-example-virtual-machines.md)
- [示例：预配和部署 Web 应用](azure-sdk-example-web-app.md)
