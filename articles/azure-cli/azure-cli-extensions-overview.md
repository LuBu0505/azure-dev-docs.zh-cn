---
title: Azure CLI 扩展
description: 使用 Azure CLI 的扩展
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 73488fc464ed052f22f071f6373fb6b8f682b778
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030979"
---
# <a name="use-extensions-with-azure-cli"></a>使用 Azure CLI 的扩展 

Azure CLI 提供用于加载扩展的功能。 扩展属于 Python wheel，它们未随附在 CLI 中，而是作为 CLI 命令运行。
使用扩展可以访问试验性命令和预发行的命令，以及编写自己的 CLI 接口。 本文介绍如何管理扩展，并解答有关其用法的常见问题。

## <a name="find-extensions"></a>查找扩展

若要查看 Microsoft 提供和维护的扩展，请使用 [az extension list-available](/cli/azure/extension#az-extension-list-available) 命令。

```azurecli-interactive
az extension list-available --output table
```

我们还在文档站点上提供了[扩展列表](azure-cli-extensions-list.md)。

## <a name="install-extensions"></a>安装扩展

找到要安装的扩展后，请使用 [az extension add](https://docs.microsoft.com/cli/azure/extension#az-extension-add) 获取它。 如果该扩展在 `az extension list-available` 中列出，可以按名称安装该扩展。

```azurecli-interactive
az extension add --name <extension-name>
```

如果此扩展来自外部资源，或者你有指向它的直接链接，则可以提供源 URL 或本地路径。 该扩展必须是已编译的 Python wheel 文件。 

```azurecli-interactive
az extension add --source <URL-or-path>
```

安装扩展后，可在 `$AZURE_EXTENSION_DIR` shell 变量的值下找到它。 如果此变量未设置，默认情况下在 Linux 和 macOS 上该值为 `$HOME/.azure/cliextensions`，在 Windows 上为 `%USERPROFILE%\.azure\cliextensions`。

## <a name="update-extensions"></a>更新扩展

如果已按名称安装了扩展，可以使用 [az extension update](https://docs.microsoft.com/cli/azure/extension#az-extension-update) 更新该扩展。

```azurecli-interactive
az extension update --name <extension-name>
```

否则，可以按照[安装扩展](#install-extensions)说明，从源更新扩展。

如果扩展名称无法由 CLI 解析，请卸载该扩展，然后尝试重新安装。 该扩展也可能属于基本 CLI。
请按照[安装 Azure CLI](install-azure-cli.md) 中的说明尝试更新 CLI，并查看扩展的命令是否已添加。

## <a name="uninstall-extensions"></a>卸载扩展

如果不再需要某个扩展，请使用 [az extension remove](https://docs.microsoft.com/cli/azure/extension#az-extension-remove) 将其卸载。

```azurecli-interactive
az extension remove --name <extension-name>
```

还可以通过从安装扩展的位置删除它来进行手动删除。 `$AZURE_EXTENSION_DIR` shell 变量定义模块的安装位置。
如果此变量未设置，默认情况下在 Linux 和 macOS 上该值为 `$HOME/.azure/cliextensions`，在 Windows 上为 `%USERPROFILE%\.azure\cliextensions`。

```bash
rm -rf $AZURE_EXTENSION_DIR/<extension-name>
```

## <a name="faq"></a>常见问题解答

以下是一些有关 CLI 扩展的其他常见问题的答案。

### <a name="what-file-formats-are-allowed-for-installation"></a>安装允许哪些文件格式？

当前，只有编译的 Python wheel 才能作为扩展进行安装。

### <a name="can-extensions-replace-existing-commands"></a>扩展是否可以替换现有命令？

是的。 扩展可以替换现有命令，但在运行已替换的命令之前，CLI 会发出一个警告。

### <a name="how-can-i-tell-if-an-extension-is-in-pre-release"></a>如何判断扩展是否处于预发布阶段？

扩展的文档和版本将显示该扩展是否是预发行版。 Microsoft 通常以 CLI 扩展的形式发布预览版命令，以后会提供相应的选项用于将这些命令移到主要 CLI 产品中。 将命令转出扩展后，应卸载旧扩展。 

### <a name="can-extensions-depend-upon-each-other"></a>扩展是否可以彼此依赖？

不是。 由于 CLI 不能保证加载顺序，因此可能无法满足依赖关系方面的要求。 删除一个扩展不会影响其他任何扩展。

### <a name="are-extensions-updated-along-with-the-cli"></a>扩展是否随 CLI 一起更新？

不是。 扩展必须单独更新，如[更新扩展](#update-extensions)所述。

### <a name="how-to-develop-our-own-extension"></a>如何开发我们自己的扩展？
有关更多帮助，请参阅官方存储库。 [Azure/azure-cli-extensions](https://github.com/Azure/azure-cli/tree/master/doc/extensions)
