---
title: Azure CLI 配置选项
description: 如何配置 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 06/11/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: fd37b633100d92a4126910a3fb9e8ad25b11423c
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82209780"
---
# <a name="azure-cli-configuration"></a>Azure CLI 配置

Azure CLI 允许用户配置日志记录、数据收集和默认参数值等设置。
CLI 提供便捷命令 `az configure` 用于管理某些默认设置。 可在配置文件中或使用环境变量设置其他值。

CLI 使用的配置值按以下优先顺序计算，列表中位于较高位置的项优先。

1. 命令行参数
2. 环境变量
3. 配置文件中的值，或使用 `az configure` 进行设置

## <a name="cli-configuration-with-az-configure"></a>使用 az configure 进行 CLI 配置

使用 [az configure](/cli/azure/reference-index#az-configure) 命令设置 CLI 的默认值。
此命令采用一个参数 `--defaults`，即 `key=value` 对的空格分隔列表。 CLI 使用提供的值来取代所需的参数。

下表列出了可用的配置键。

| 名称 | 说明 |
|------|-------------|
| group | 所有命令使用的默认资源组。 |
| location | 所有命令使用的默认位置。 |
| Web | `az webapp` 命令使用的默认应用名称。 |
| vm | `az vm` 命令使用的默认 VM 名称。 |
| vmss | 用于 `az vmss` 命令的默认虚拟机规模集 (VMSS) 名称。 |
| acr | `az acr` 命令使用的默认容器注册表名称。 |

例如，可按如下所示设置所有命令的默认资源组和位置。

```azurecli-interactive
az configure --defaults location=westus2 group=MyResourceGroup
```

## <a name="cli-configuration-file"></a>CLI 配置文件

CLI 配置文件包含用于管理 CLI 行为的其他设置。 配置文件本身位于 `$AZURE_CONFIG_DIR/config`。 `AZURE_CONFIG_DIR` 的默认值为 `$HOME/.azure`（在 Linux 与 macOS 上）和 `%USERPROFILE%\.azure`（在 Windows 上）。

配置文件是以 INI 文件格式编写的。 此文件格式定义为节标头后接键-值项的列表。

* 节标头以 `[section-name]` 的形式写入。 节名称区分大小写。
* 项以 `key=value` 的形式写入。 键名称不区分大小写。
* 注释是以 `#` 或 `;` 开头的任何行。 不允许内联注释。

布尔值不区分大小写，由以下值表示。

* __True__：1、yes、true、on
* __False__：0、no、false、off

以下示例 CLI 配置文件禁用任何确认提示，并设置在 `/var/log/azure` 目录中进行日志记录。

```ini
[core]
disable_confirm_prompt=Yes

[logging]
enable_log_file=yes
log_dir=/var/log/azure
```

有关所有可用配置值及其含义的详细信息，请参阅下一部分。 有关 INI 文件格式的完整详细信息，请参阅 [INI 上的 Python 文档](https://docs.python.org/3/library/configparser.html#supported-ini-file-structure)。

## <a name="cli-configuration-values-and-environment-variables"></a>CLI 配置值和环境变量

下表包含可在配置文件中放置的所有节和选项名称。 将其相应环境变量设置为 `AZURE_{section}_{name}`（全大写）。 例如，在 `storage_account` 变量中设置 `batchai` 的 `AZURE_BATCHAI_STORAGE_ACCOUNT` 默认值。

如果提供了默认值，则任何命令都不再需要该参数， 而是改用默认值。

| 部分 | 名称      | 类型 | 说明|
|---------|-----------|------|------------|
| __core__ | output | 字符串 | 默认输出格式。 可以是 `json`、`jsonc`、`tsv` 或 `table`。 |
| | disable\_confirm\_prompt | boolean | 启用/禁用确认提示。 |
| | collect\_telemetry | boolean | 允许 Microsoft 收集有关 CLI 使用情况的匿名数据。 有关隐私信息，请参阅 [Azure CLI 使用条款](https://github.com/Azure/azure-cli/blob/dev/LICENSE)。 |
| __logging__ | enable\_log\_file | boolean | 启用/关闭日志记录。 |
| | log\_dir | 字符串 | 要将日志写入到的目录。 此值默认为 `${AZURE_CONFIG_DIR}/logs`。 |
| __storage__ | connection\_string | 字符串 | `az storage` 命令使用的默认连接字符串。 |
| | account | 字符串 | `az storage` 命令使用的默认帐户名。 |
| | key | 字符串 | `az storage` 命令使用的默认帐户密钥。 |
| | sas\_token | 字符串 | `az storage` 命令使用的默认 SAS 令牌。 |
| __batchai__ | storage\_account | 字符串 | `az batchai` 命令使用的默认存储帐户。 |
| | storage\_key | 字符串 | `az batchai` 命令使用的默认存储密钥。 |
| __batch__ | account | 字符串 | `az batch` 命令使用的默认 Azure Batch 帐户名。 |
| | access\_key | 字符串 | `az batch` 命令使用的默认访问密钥。 只能与 `aad` 授权配合使用。 |
| | endpoint | 字符串 | `az batch` 命令要连接到的默认终结点。 |
| | auth\_mode | 字符串 | `az batch` 命令使用的授权模式。 可以是 `shared_key` 或 `aad`。 |
| __cloud__ | name | 字符串 | 所有 `az` 命令的默认云。  可能的值为 `AzureCloud`（默认）、`AzureChinaCloud`、`AzureUSGovernment`、`AzureGermanCloud`。 若要更改云，可以使用 `az cloud set –name` 命令。  有关示例，请参阅[通过 Azure CLI 管理云](manage-clouds-azure-cli.md)。 |

> [!NOTE]
> 配置文件中可能包含其他值，但这些值是直接通过 CLI 命令（包括 `az configure`）管理的。 你只能自行更改上表中列出的值。
