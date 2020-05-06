---
title: 安装适用于 macOS 的 Azure CLI
description: 如何在 macOS 上安装 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/05/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 862ebf9144d7e81be6dda550eba108198f38d397
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031059"
---
# <a name="install-azure-cli-on-macos"></a>在 macOS 上安装 Azure CLI

对于 macOS 平台，可以通过 [homebrew 包管理器](https://brew.sh)安装 Azure CLI。 使用 Homebrew 可以轻松保持 CLI 的最新安装状态。 该 CLI 包已在 macOS 10.9 和更高版本中测试。

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-with-homebrew"></a>使用 Homebrew 安装

Homebrew 是管理 CLI 安装的最容易的方法。 它可以方便地进行安装、更新和卸载。
如果系统中没有可用的 Homebrew，请先[安装 Homebrew](https://docs.brew.sh/Installation.html)，然后继续。

安装 CLI 时，可以先更新 brew 存储库信息，然后运行 `install` 命令：

```bash
brew update && brew install azure-cli
```

> [!IMPORTANT]
>
> Azure CLI 依赖于 Homebrew `python3` 包，并将安装它。
> Azure CLI 保证可与 Homebrew 上发布的最新版本的 `python3` 兼容。

然后即可使用 `az` 命令来运行 Azure CLI。 若要登录，请使用 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="troubleshooting"></a>故障排除

如果在通过 Homebrew 安装 CLI 时遇到问题，会显示以下常见错误。 如果遇到的问题未在本文中列出，请[在 github 上提出问题](https://github.com/Azure/azure-cli/issues)。

### <a name="completion-is-not-working"></a>无法完成

Azure CLI 的 Homebrew 公式将在 Homebrew 托管的完成目录中安装名为 `az` 的完成文件（默认位置为 `/usr/local/etc/bash_completion.d/`）。 若要启用完成功能，请按[此处](https://docs.brew.sh/Shell-Completion)的 Homebrew 说明操作。

### <a name="unable-to-find-python-or-installed-packages"></a>找不到 Python 或安装的包

在执行 Homebrew 安装期间，可能会出现次要版本不匹配或其他问题。 CLI 不会使用 Python 虚拟环境，因此，它只能查找已安装的 Python 版本。 可行的解决方法之一是从 Homebrew 安装并重新链接 `python3` 依赖项。

```bash
brew update && brew install python3 && brew upgrade python3
brew link --overwrite python3
```

### <a name="cli-version-1x-is-installed"></a>已安装 CLI 安装 1.x

如果安装了过时的版本，则陈旧的 Homebrew 缓存可能导致此问题。 请遵照[更新](#update)说明操作。

### <a name="proxy-blocks-connection"></a>代理阻止连接

你可能无法从 Homebrew 获取资源，除非已将其正确配置为使用你的代理。 请遵循 [Homebrew 代理配置说明](https://docs.brew.sh/Manpage#using-homebrew-behind-a-proxy)。

> [!IMPORTANT]
> 如果你位于代理后面，则必须设置 `HTTP_PROXY` 和 `HTTPS_PROXY` 以通过 CLI 连接到 Azure 服务。
> 如果不使用基本身份验证，建议将这些变量导出到 `.bashrc` 文件中。
> 请始终遵循企业的安全策略和系统管理员的要求。

为了从 Homebrew 获取 Bottle 资源，代理必须允许与以下地址之间的 HTTPS 连接：

* `https://formulae.brew.sh`
* `https://homebrew.bintray.com`

## <a name="update"></a>更新

CLI 定期使用 Bug 修复、改进、新功能和预览版功能进行更新。 新版本大约两周发布一次。 更新本地存储库信息，然后升级 `azure-cli` 包。

```bash
brew update && brew upgrade azure-cli
```

## <a name="uninstall"></a>卸载

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

使用 Homebrew 卸载 `azure-cli` 包。

```bash
brew uninstall azure-cli
```

## <a name="other-installation-methods"></a>其他安装方法

如果不能使用 homebrew 在你的环境中安装 Azure CLI，可以使用适用于 Linux 的手册说明。 请注意，此过程未正式保持与 macOS 兼容。 始终建议使用诸如 Homebrew 之类的包管理器。 仅当没有其他选项可用时才使用手动安装方法。

有关手动安装说明，请参阅[在 Linux 上手动安装 Azure CLI](install-azure-cli-linux.md)。

## <a name="next-steps"></a>后续步骤

现在你已经安装了 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
