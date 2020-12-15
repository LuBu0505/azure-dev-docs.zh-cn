---
title: Azure 上 Node.js 应用的数据库
description: Azure 提供了各种不同的数据库，以用于 Web 应用和其他 Node.js 应用。
ms.topic: how-to
ms.date: 12/08/2020
ms.custom: devx-track-js
ms.openlocfilehash: 2aae93a85ca505967f0c999be4addc78ac31ad02
ms.sourcegitcommit: 1901759f41adfac3c3f2ff135bcf72206543b639
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/09/2020
ms.locfileid: "96933280"
---
# <a name="integrate-databases-in-nodejs-apps"></a>将数据库集成到 Node.js 应用中

Azure 数据库提供一个优秀的托管云数据解决方案，同时提供本机 API 或 Azure SDK 来连接到数据库。 

## <a name="database-and-data-storage-solutions-on-azure"></a>Azure 上的数据库和数据存储解决方案

下表链接到有关如何使用 Node.js 连接到 Azure 数据库以及如何将 Azure 数据库与 Node.js 结合使用的各种文章。 有关不同数据库选项的并排列表，请参阅[数据库 - 完全托管的智能数据库服务](https://azure.microsoft.com/product-categories/databases/)。

| 服务 | 快速入门 | npm 包 |
| --- | --- | --- |
| Cosmos DB 上的 SQL Server| [使用 SQL Server 创建 Node.js Azure Cosmos DB Web 应用](/azure/cosmos-db/create-sql-api-nodejs) | [@azure/cosmos](https://www.npmjs.com/package/@azure/cosmos) |
| Cosmos DB 上的 MongoDB| [创建 Node.js 和 MongoDB Web 应用](/azure/app-service-web/app-service-web-tutorial-nodejs-mongodb-app) | 任何 MongoDB 客户端 |
| Cosmos DB 上的 Cassandra|[使用 Node.js SDK 和 Azure Cosmos DB 构建 Cassandra 应用](/azure/cosmos-db/create-cassandra-nodejs)|[npm cassandra-driver](https://www.npmjs.com/package/cassandra-driver)|
| Cosmos DB 上的 Gremlin|[使用 Azure Cosmos DB Gremlin API 帐户生成 Node.js 应用程序](/azure/cosmos-db/create-graph-nodejs)|[npm gremlin](https://www.npmjs.com/package/gremlin)|
| Cosmos DB 上的 Redis Cache| [创建和使用 Redis 缓存](/azure/redis-cache/cache-nodejs-get-started) | [npm redis](https://www.npmjs.com/package/redis)|
| **Azure SQL 数据库** | [使用 Node.js 查询 Azure SQL 数据库](/azure/sql-database/sql-database-connect-query-nodejs) |[npm tedious](https://www.npmjs.com/package/tedious) |
| **MySQL** | [使用 Node.js 连接和查询数据](/azure/mysql/connect-nodejs) | [npm mysql](https://www.npmjs.com/package/mysql)|
| **PostgreSQL** | [使用 Node.js 连接和查询数据](/azure/postgresql/connect-nodejs) |[npm pg](https://www.npmjs.com/package/pg) |

## <a name="cosmos-db-connection-strings-with-azure-cli"></a>使用 Azure CLI 获取 Cosmos DB 连接字符串

使用以下命令 [az cosmosdb keys list](/cli/azure/cosmosdb?view=azure-cli-latest#az-cosmosdb-list-connection-strings)：

```azurecli-interactive
az cosmosdb keys list \
    -n $accountName \
    -g $resourceGroupName \
    --type connection-strings
```

## <a name="sql-connection-strings-with-azure-cli"></a>使用 Azure CLI 获取 SQL 连接字符串

使用以下命令 [az sql db show-connection-string](/cli/azure/sql/db?view=azure-cli-latest#az_sql_db_show_connection_string)：

```azurecli-interactive
az sql db show-connection-string \
    --client {ado.net, jdbc, odbc, php, php_pdo, sqlcmd} \
     [--auth-type {ADIntegrated, ADPassword, SqlPassword}] \
     [--ids] \
     [--name] \
     [--server] \
     [--subscription]
```

## <a name="mysql-username-and-password-with-azure-cli"></a>使用 Azure CLI 获取 MySQL 用户名和密码

这些在[创建资源时](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create)设置。 

## <a name="postgresql-username-and-password-with-azure-cli"></a>使用 Azure CLI 获取 PostgreSQL 用户名和密码

这些在[创建资源时](/cli/azure/postgres/server?view=azure-cli-latest#az_postgres_server_create)设置。 

## <a name="azure-storage-solutions-for-files-and-data"></a>用于文件和数据的 Azure 存储解决方案

还可以将 Azure 存储用于文件 (blob)、表以及队列（消息）存储：

| 服务 | 快速入门 |推荐的 SDK |
| --- | --- |--- |
| **Blob** | [使用 Azure Storage v10 SDK for JavaScript 上载、下载、列出和删除 blob](/azure/storage/blobs/storage-quickstart-blobs-nodejs-v10) |[@azure/storage-blob](https://www.npmjs.com/package/@azure/storage-blob)|
| **队列** | [如何通过 Node.js 使用队列存储](/azure/storage/queues/storage-nodejs-how-to-use-queues) |[npm @azure/storage-queue](https://www.npmjs.com/package/@azure/storage-queue)|
| **表** | [如何通过 Node.js 使用表存储](/azure/cosmos-db/table-storage-how-to-use-nodejs) |[npm azure-storage](https://www.npmjs.com/package/azure-storage)|

## <a name="next-steps"></a>后续步骤

* 使用 Azure Functions [编写无服务器代码](develop-serverless-apps.md)