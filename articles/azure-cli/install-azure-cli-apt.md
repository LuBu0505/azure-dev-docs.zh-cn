---
title: 使用 apt 在 Linux 上安装 Azure CLI
description: 如何使用 apt 包管理器安装 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 10/14/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 302b98717ee422de9bd60a57b18d900bcf5fcaf9
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030969"
---
# <a name="install-azure-cli-with-apt"></a>使用 apt 安装 Azure CLI

如果运行附带 `apt` 的发行版（例如 Ubuntu 或 Debian），则可以安装适用于 Azure CLI 的 x86_64 包。 此包已经过测试，支持：

* Ubuntu trusty、xenial、artful、bionic 和 disco
* Debian wheezy、jessie、stretch 和 buster

[!INCLUDE [current-version](includes/current-version.md)]

> [!NOTE]
>
> 用于 Azure CLI 的包会安装其自己的 Python 解释器，而不使用系统 Python。

## <a name="install"></a>安装

我们提供了两种方法来为支持 `apt` 的分发版安装 Azure CLI：可以自动运行 install 命令的一体式脚本；可由用户作为分步式过程运行的指令。

### <a name="install-with-one-command"></a>使用一条命令安装

我们将提供并维护可以通过一个步骤运行所有安装命令的脚本。 可以使用 `curl` 运行该脚本并通过管道将其直接传递给 `bash`，或者将该脚本下载到某个文件，并在检查后再运行它。

> [!IMPORTANT]
> 此脚本只在 Ubuntu 16.04+ 和 Debian 8+ 中经过验证。 它不一定可在其他分发版上运行。
> 如果使用衍生的分发版（例如 Linux Mint），请遵照手动安装说明，并执行任何必要的故障排除。

```bash
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### <a name="manual-install-instructions"></a>手动安装说明

如果不想以超级用户的身份运行脚本，或者一体化脚本失败，请遵循以下步骤安装 Azure CLI。

1. 获取安装过程所需的包：

    ```bash
    sudo apt-get update
    sudo apt-get install ca-certificates curl apt-transport-https lsb-release gnupg
    ```

2. 下载并安装 Microsoft 签名密钥：

    ```bash
    curl -sL https://packages.microsoft.com/keys/microsoft.asc |
        gpg --dearmor |
        sudo tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
    ```

3. <div id="set-release"/>添加 Azure CLI 软件存储库：

    ```bash
    AZ_REPO=$(lsb_release -cs)
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $AZ_REPO main" |
        sudo tee /etc/apt/sources.list.d/azure-cli.list
    ```

4. 更新存储库信息并安装 `azure-cli` 包：

    ```bash
    sudo apt-get update
    sudo apt-get install azure-cli
    ```

使用 `az` 命令运行 Azure CLI。 若要登录，请使用 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="troubleshooting"></a>疑难解答

下面是使用 `apt` 安装时出现的一些常见问题。 如果遇到的问题未在本文中列出，请[在 github 上提出问题](https://github.com/Azure/azure-cli/issues)。

### <a name="lsb_release-does-not-return-the-correct-base-distribution-version"></a>lsb_release 没有返回正确的基础发行版本

某些 Ubuntu 或 Debian 派生的版本（例如 Linux Mint）不会通过 `lsb_release` 返回正确的版本名称。 此值在安装过程中用于确定要安装的包。 如果知道自己的发行版派生自的 Ubuntu 或 Debian 版本的代码名称，则可在[添加存储库](#set-release)时手动设置 `AZ_REPO` 值。 否则，请查看发行版的信息，了解如何确定基础发行版代码名称，然后将 `AZ_REPO` 设置为正确值。

### <a name="no-package-for-your-distribution"></a>没有适用于你的发行版的程序包

有时候，在某个发行版发布后，可能要过一段时间才会有 Azure CLI 包可用于它。 对于依赖项的将来版本，Azure CLI 设计为弹性的并且尽可能少地依赖这些依赖项。 如果没有适用于你的基础发行版的程序包，请尝试使用较早发行版的程序包。

为此，请在[添加存储库](#set-release)时手动设置 `AZ_REPO` 的值。 对于 Ubuntu 发行版，请使用 `bionic` 存储库，对于 Debian 发行版，请使用 `stretch`。 在 Ubuntu Trusty 和 Debian Wheezy 之前发布的发行版不受支持。

### <a name="elementary-os-eos-fails-to-install-the-azure-cli"></a>基本 OS (EOS) 无法安装 Azure CLI

EOS 无法安装 Azure cli，因为 `lsb_release` 返回 `HERA`，这是 EOS 版本名称。  解决方案是修复文件 `/etc/apt/sources.list.d/azure-cli.list` 并将 `hera main` 更改为 `bionic main`。

原始文件内容：

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ hera main
```

修改的文件内容

```
deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ bionic main
```

### <a name="proxy-blocks-connection"></a>代理阻止连接

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

你可能还想要显式配置 `apt` 以便在所有情况下都使用此代理。 请确保以下行显示在 `/etc/apt/apt.conf.d/` 下的 `apt` 配置文件中。 我们建议使用现有的全局配置文件（现有的代理配置文件 `40proxies`）或 `99local`，但要遵守系统管理要求。

```apt.conf
Acquire {
    http::proxy "http://[username]:[password]@[proxy]:[port]";
    https::proxy "https://[username]:[password]@[proxy]:[port]";
}
```

如果代理不使用基本身份验证，请__删除__代理 URI 的 `[username]:[password]@` 部分。 如果需要代理配置的详细信息，请参阅官方的 Ubuntu 文档：

* [apt.conf manpage](http://manpages.ubuntu.com/manpages/bionic/en/man5/apt.conf.5.html)
* [Ubuntu wiki - apt-get 操作方法](https://help.ubuntu.com/community/AptGet/Howto#Setting_up_apt-get_to_use_a_http-proxy)

为了获取 Microsoft 签名密钥并从我们的存储库中获取包，代理必须允许与以下地址之间的 HTTPS 连接：

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>更新

使用 `apt-get upgrade` 更新 CLI 包。

   ```bash
   sudo apt-get update && sudo apt-get upgrade
   ```

> [!NOTE]
> 此命令将会升级系统上所有未发生依赖关系更改的已安装包。
> 若只要升级 CLI，请使用 `apt-get install`。
>
> ```bash
> sudo apt-get update && sudo apt-get install --only-upgrade -y azure-cli
> ```

## <a name="uninstall"></a>卸载

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. 使用 `apt-get remove` 进行卸载：

    ```bash
    sudo apt-get remove -y azure-cli
    ```

2. 如果不打算重新安装 CLI，请删除 Azure CLI 存储库信息：

   ```bash
   sudo rm /etc/apt/sources.list.d/azure-cli.list
   ```

3. 如果不使用 Microsoft 的其他包，请删除签名密钥：

    ```bash
    sudo rm /etc/apt/trusted.gpg.d/microsoft.asc.gpg
    ```

4. 删除任何不需要的包：

   ```bash
   sudo apt autoremove
   ```

## <a name="next-steps"></a>后续步骤

现在你已经安装了 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
