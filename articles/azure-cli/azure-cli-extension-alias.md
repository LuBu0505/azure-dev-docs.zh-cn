---
title: Azure CLI 别名扩展
description: 如何使用 Azure CLI 别名扩展
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 4d324235360e017970a215226572e3effeb9b917
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030849"
---
# <a name="the-azure-cli-alias-extension"></a>Azure CLI 别名扩展

使用别名扩展，用户可以通过使用现有命令定义自定义 Azure CLI 命令。 别名允许快捷方式，因此可以简化工作流。 由于别名由 Jinja2 模板引擎提供支持，它们甚至能够提供高级参数处理。

> [!NOTE]
> 别名扩展处于公共预览阶段。 功能和配置文件格式可能发生变化。

## <a name="install-the-alias-extension"></a>安装别名扩展

为使用别名扩展而需安装的最低 Azure CLI 版本为 **2.0.28**。 若要检查 CLI 版本，请运行 `az --version`。 如果需要更新安装，请按照[安装 Azure CLI](./install-azure-cli.md) 中的说明执行操作。

使用 [az extension add](/cli/azure/extension#az-extension-add) 命令安装该扩展。

```azurecli-interactive
az extension add --name alias
```

使用 [az extension list](/cli/azure/extension#az-extension-list) 验证扩展安装。 如果该别名扩展已正确安装，它会列入命令输出。

```azurecli-interactive
az extension list --output table --query '[].{Name:name}'
```

```output
Name
------
alias
```

## <a name="keep-the-extension-up-to-date"></a>使扩展保持最新

该别名扩展处于活跃开发阶段，新版本将定期发布。 更新 CLI 时不会安装新版本。 请使用 [az extension update](/cli/azure/extension#az-extension-update) 安装该扩展的更新。

```azurecli-interactive
az extension update --name alias
```

## <a name="manage-aliases-for-the-azure-cli"></a>管理 Azure CLI 的别名

使用别名扩展可以创建和管理其他 CLI 命令的别名。 若要查看所有可用命令和参数详细信息，请结合 `--help` 运行别名命令。

```azurecli-interactive
az alias --help
```

## <a name="create-simple-alias-commands"></a>创建简单的别名命令

别名的一个用途是缩短现有命令组或命令名称。 例如，可以将 `group` 命令组缩短为 `rg`，将 `list` 命令缩短为 `ls`。

```azurecli-interactive
az alias create --name rg --command group
az alias create --name ls --command list
```

这些新定义的别名现在可在其定义所在的任何位置使用。

```azurecli-interactive
az rg list
az rg ls
az vm ls
```

请不要将 `az` 包含为命令的一部分。

别名也可以是完整命令的快捷方式。 下一个示例在表输出中列出了可用资源组及其位置：

```azurecli-interactive
az alias create --name ls-groups --command "group list --query '[].{Name:name, Location:location}' --output table"
```

现在 `ls-groups` 可以像任何其他 CLI 命令一样运行。

```azurecli-interactive
az ls-groups
```

## <a name="create-an-alias-command-with-arguments"></a>使用参数创建别名命令

还可以将位置参数包含为别名中的 `{{ arg_name }}`，来将其添加到别名命令中。 括号内的空格是必需的。

```azurecli-interactive
az alias create --name "alias_name {{ arg1 }} {{ arg2 }} ..." --command "invoke_including_args"
```

下一个示例别名演示如何使用位置参数获取 VM 的公共 IP 地址。

```azurecli-interactive
az alias create \
    --name "get-vm-ip {{ resourceGroup }} {{ vmName }}" \
    --command "vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }}
        --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress"
```

当运行此命令时，需要为位置参数提供值。

```azurecli-interactive
az get-vm-ip MyResourceGroup MyVM
```

还可以在别名命令中使用环境变量，在运行时会评估这些环境变量。 下一个示例将添加 `create-rg` 别名，该别名在 `eastus` 中创建资源组，并且添加 `owner` 标记。 此标记被分配了本地环境变量 `USER` 的值。

```azurecli-interactive
az alias create \
    --name "create-rg {{ groupName }}" \
    --command "group create --name {{ groupName }} --location eastus --tags owner=\$USER"
```

若要在别名命令中注册环境变量，必须转义美元符号 `$`。

## <a name="process-arguments-using-jinja2-templates"></a>使用 Jinja2 模板处理参数

别名扩展中的参数替换由 [Jinja2](http://jinja.pocoo.org/docs/2.10/) 执行。 使用 Jinja2 模板可以操控参数。

使用 Jinja2 模板可以编写接受与基础命令不同类型的参数的别名。 例如，可以编写接受存储 URL 的别名。 然后，此 URL 将受到分析，以将帐户和容器名称传递给存储命令。

```azurecli-interactive
az alias create \
    --name 'storage-ls {{ url }}' \
    --command "storage blob list
        --account-name {{ url.replace('https://', '').split('.')[0] }}
        --container-name {{ url.replace('https://', '').split('/')[1] }}"
```

若要了解 Jinja2 模板引擎，请参阅 [Jinja2 文档](http://jinja.pocoo.org/docs/2.10/templates/)。

## <a name="alias-configuration-file"></a>别名配置文件

创建和修改别名的另一种方法是更改别名配置文件。 别名命令定义会写入位于 `$AZURE_USER_CONFIG/alias` 的配置文件中。 `AZURE_USER_CONFIG` 的默认值为 `$HOME/.azure`（在 macOS 与 Linux 上）和 `%USERPROFILE%\.azure`（在 Windows 上）。 别名配置文件以 INI 配置文件格式编写。 别名命令的格式为：

```ini
[alias_name]
command = invoked_commands
```

对于包含位置参数的别名，别名命令的格式为：

```ini
[alias_name {{ arg1 }} {{ arg2 }} ...]
command = invoked_commands_including_args
```

## <a name="create-an-alias-command-with-arguments-via-the-alias-configuration-file"></a>通过别名配置文件创建带有参数的别名命令

以下示例演示了包含参数的命令的别名。 此命令获取 VM 的公共 IP 地址。 别名命令必须在一行中输入，并在别名中使用所有参数。

```ini
[get-vm-ip {{ resourceGroup }} {{ vmName }}]
command = vm list-ip-addresses --resource-group {{ resourceGroup }} --name {{ vmName }} --query [0].virtualMachine.network.publicIpAddresses[0].ipAddress
```

## <a name="uninstall-the-alias-extension"></a>卸载别名扩展

若要卸载扩展，请使用 [az extension remove](/cli/azure/extension#az-extension-remove)命令。

```azurecli-interactive
az extension remove --name alias
```

如果由于出现 bug 或其他扩展问题而卸载了该扩展，请[提出 GitHub 问题](https://github.com/Azure/azure-cli-extensions/issues)，以便我们可以提供修复。
