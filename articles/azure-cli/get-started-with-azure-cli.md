---
title: Azure CLI 入门
description: 学习命令基础知识，开始使用 Azure CLI。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 01/30/2020
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: bc9b86db6fb9c5b3731550df9dda96debcbfba9f
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030719"
---
# <a name="get-started-with-azure-cli"></a>Azure CLI 入门

欢迎使用 Azure CLI！  本文介绍 CLI 并帮助你完成常见任务。

> [!NOTE]
>
> 在脚本和 Microsoft 文档网站中，已针对 `bash` shell 编写了 Azure CLI 示例。 一行的示例将在任何平台上运行。 包括续行符 (`\`) 或变量赋值的更长示例需要修改才能在其他 shell（包括 PowerShell）上工作。

## <a name="install-or-run-in-azure-cloud-shell"></a>在 Azure Cloud Shell 中安装或运行

开始使用 Azure CLI 的最简单方法是通过浏览器在 Azure Cloud Shell 环境中运行它。 若要了解 Cloud Shell，请参阅 [bash in Azure Cloud Shell 快速入门](/azure/cloud-shell/quickstart)。

准备好安装 CLI 时，请参阅[安装说明](install-azure-cli.md)。

在第一次安装 CLI 后，请通过运行 `az --version` 检查它是否已安装，以及所安装的版本是否正确。

> [!NOTE]
> 如果使用 Azure 经典部署模型，请[安装 Azure 经典 CLI](install-classic-cli.md)。

## <a name="sign-in"></a>登录

对本地安装使用任何 CLI 命令之前，需要使用 [az login](/cli/azure/reference-index#az-login) 登录。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

登录后，你将看到与你的 Azure 帐户关联的订阅列表。 在使用 `isDefault: true` 的情况下显示的订阅信息是登录后当前已激活的订阅。 若要选择另一个订阅，请将 [az account set](/cli/azure/account#az-account-set) 命令与要切换到的订阅 ID 配合使用。 有关订阅选择的详细信息，请参阅[使用多个 Azure 订阅](manage-azure-subscriptions-azure-cli.md)。

有多种方法可用来以非交互方式登录，[使用 Azure CLI 登录](authenticate-azure-cli.md)中详细介绍了这些方法。

## <a name="common-commands"></a>常用命令

此表列出了 CLI 中的一些常用命令及其参考文档的链接。

| 资源类型 | Azure CLI 命令组 |
|---------------|-------------------------|
| [资源组](/azure/azure-resource-manager/resource-group-overview) | [az group](/cli/azure/group) |
| [虚拟机](/azure/virtual-machines) | [az vm](/cli/azure/vm) |
| [存储帐户](/azure/storage/common/storage-introduction) | [az storage account](/cli/azure/storage/account) |
| [Key Vault](/azure/key-vault/key-vault-whatis) | [az keyvault](/cli/azure/keyvault) |
| [Web 应用程序](/azure/app-service) | [az webapp](/cli/azure/webapp) |
| [SQL 数据库](/azure/sql-database) | [az sql server](/cli/azure/sql/server) |
| [CosmosDB](/azure/cosmos-db) | [az cosmosdb](/cli/azure/cosmosdb) |

## <a name="finding-commands"></a>查找命令

CLI 中的命令以组命令的形式进行组织。   每个组表示一个 Azure 服务，命令针对该服务运行。

若要搜索命令，请使用 [az find](/cli/azure/reference-index#az-find)。 例如，若要搜索包含 `secret` 的命令名称，请使用以下命令：

```azurecli-interactive
az find secret
```

使用 `--help` 参数获取组的命令和子组的完整列表。 例如，若要查找用于处理网络安全组 (NSG) 的 CLI 命令，请运行：

```azurecli-interactive
az network nsg --help
```

CLI 为 bash shell 下的命令提供完整 tab 键补全。

## <a name="globally-available-arguments"></a>全局可用参数

有一些参数可用于每条命令。

* `--help` 会输出有关命令及其参数的 CLI 参考信息并列出可用的子组和命令。
* `--output` 可更改输出格式。 可用的输出格式包括 `json`、`jsonc`（彩色 JSON）、`tsv`（制表符分隔值）、`table`（用户可读 ASCII 表）以及 `yaml`。 默认情况下，CLI 输出 `json`。 若要详细了解可用输出格式，请参阅 [Azure CLI 的输出格式](format-output-azure-cli.md)。
* `--query` 使用 [JMESPath 查询语言](http://jmespath.org/)筛选从 Azure 服务返回的输出。 若要详细了解查询，请参阅[使用 Azure CLI 查询命令结果](query-azure-cli.md)和 [JMESPath 教程](http://jmespath.org/tutorial.html)。
* `--verbose` 输出有关操作期间在 Azure 中创建的资源的信息和其他有用信息。
* `--debug` 输出有关 CLI 操作的更详细信息，用于调试目的。 如果发现了 bug，在提交 bug 报告时，请提供启用 `--debug` 标志生成的输出。

## <a name="interactive-mode"></a>交互模式

CLI 提供一种交互模式，可自动显示帮助信息，并可更轻松地选择子命令。 使用 [az interactive](/cli/azure/reference-index#az-interactive) 命令即可进入交互模式。

```azurecli-interactive
az interactive
```

有关交互模式的详细信息，请参阅 [Azure CLI 交互模式](interactive-azure-cli.md)。

此外，还有提供交互体验的 [Visual Studio Code 插件](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli)，包括自动完成和鼠标悬停显示的文档。

## <a name="learn-cli-basics-with-quickstarts-and-tutorials"></a>使用快速入门和教程了解 CLI 基础知识

若要开始使用 Azure CLI，请试用深入教程以设置虚拟机并利用 CLI 的功能查询 Azure 资源。

> [!div class="nextstepaction"]
> [使用 Azure CLI 教程创建虚拟机](azure-cli-vm-tutorial.yml)

其他热门服务也有快速入门教程。

* [使用 Azure CLI 创建存储帐户](/azure/storage/common/storage-quickstart-create-storage-account-cli)
* [使用 CLI 向/从 Azure Blob 存储转移对象](/azure/storage/blobs/storage-quickstart-blobs-cli)
* [使用 Azure CLI 创建单一 Azure SQL 数据库](/azure/sql-database/sql-database-get-started-cli)
* [使用 Azure CLI 创建 Azure Database for MySQL 服务器](/azure/mysql/quickstart-create-mysql-server-database-using-azure-cli)
* [使用 Azure CLI 创建用于 PostgreSQL 的 Azure 数据库](/azure/postgresql/quickstart-create-server-database-azure-cli)
* [在 Azure 中创建 Python Web 应用](/azure/app-service/app-service-web-get-started-python)
* [在用于容器的 Azure Web 应用中运行自定义 Docker 中心映像](/azure/app-service/containers/quickstart-custom-docker-image)

## <a name="give-feedback"></a>提供反馈

我们欢迎你提供有关 CLI 的反馈以帮助我们改进和解决 bug。 可以[在 Github 上提出问题](https://github.com/azure/azure-cli/issues)，或利用 CLI 的内置功能来通过 [az feedback](/cli/azure/reference-index#az-feedback) 命令留下常规反馈。

```azurecli-interactive
az feedback
```

## <a name="see-also"></a>另请参阅

* [Azure CLI 可以管理的服务](azure-services-the-azure-cli-can-manage.md)
* [Azure CLI 的完整命令参考列表](/cli/azure/reference-index)
* [有关如何使用 Azure CLI 的热门文章](popular-articles-using-the-azure-cli.md)
