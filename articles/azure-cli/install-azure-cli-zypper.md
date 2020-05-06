---
title: 使用 zypper 在 Linux 上安装 Azure CLI
description: 如何使用 zypper 安装 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: d07fe2e807bd6e1fac6d0e9f883bcc8092be46bb
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030999"
---
# <a name="install-azure-cli-with-zypper"></a>使用 zypper 安装 Azure CLI

对于附带 `zypper` 的 Linux 分发版（例如 openSUSE 或 SLES），可以安装适用于 Azure CLI 的包。 此包已在 openSUSE Leap 15.1 和 SLES 15 中测试。

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## <a name="install"></a>安装

1. 安装 `curl`：

   ```bash
   sudo zypper install -y curl
   ```

2. 导入 Microsoft 存储库密钥：

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

3. 创建本地 `azure-cli` 存储库信息：

   ```bash
   sudo zypper addrepo --name 'Azure CLI' --check https://packages.microsoft.com/yumrepos/azure-cli azure-cli
   ```

4. 更新 `zypper` 包索引并安装：

   ```bash
   sudo zypper install --from azure-cli azure-cli
   ```
   输入 2 即可忽略某些依赖项，继续安装。

然后即可使用 `az` 命令来运行 Azure CLI。 若要登录，请使用 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="troubleshooting"></a>故障排除

下面是使用 `zypper` 安装时出现的一些常见问题。 如果遇到的问题未在本文中列出，请[在 github 上提出问题](https://github.com/Azure/azure-cli/issues)。

### <a name="install-on-sles-12-or-other-systems-without-python-36"></a>安装在 SLES 12 或不带 Python 3.6 的其他系统上

在 SLES 12 上，默认的 python3 包为 3.4，不受 Azure CLI 的支持。 可以先从源构建较高版本的 python3， 然后下载 Azure CLI 包，并在没有依赖项的情况下安装它。
```bash
$ sudo zypper install -y gcc gcc-c++ make ncurses patch wget tar zlib-devel zlib openssl-devel
# Download Python source code
$ PYTHON_VERSION="3.6.9"
$ PYTHON_SRC_DIR=$(mktemp -d)
$ wget -qO- https://www.python.org/ftp/python/$PYTHON_VERSION/Python-$PYTHON_VERSION.tgz | tar -xz -C "$PYTHON_SRC_DIR"
# Build Python
$ $PYTHON_SRC_DIR/*/configure
$ make
$ sudo make install
#Download azure-cli package 
$ AZ_VERSION=$(zypper --no-refresh info azure-cli |grep Version | awk -F': ' '{print $2}' | awk '{$1=$1;print}')
$ wget https://packages.microsoft.com/yumrepos/azure-cli/azure-cli-$AZ_VERSION.x86_64.rpm
#Install without dependency
$ sudo rpm -ivh --nodeps azure-cli-$AZ_VERSION.x86_64.rpm
```

### <a name="proxy-blocks-connection"></a>代理阻止连接

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

你可能还想要显式配置 `zypper`（通过 `yast2`）以便在所有情况下都使用此代理。 为此，请以超级用户身份运行 `yast2 proxy` 命令，并填充窗体中显示的信息。 如果系统上有可用的窗口管理器，还可以使用 `Network Services > Proxy` 中的 `YaST Control Center` 窗格。

有关高级配置或详细信息，请参阅 [OpenSUSE 代理配置文档](https://www.suse.com/documentation/slms1/book_slms/data/sec_wy_config_updates_proxy.html)

为了获取 Microsoft 签名密钥并从我们的存储库中获取包，代理必须允许与以下地址之间的 HTTPS 连接：

* `https://packages.microsoft.com`
* `https://download.opensuse.org`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>更新

可以使用 `zypper update` 命令来更新包。

```bash
sudo zypper refresh
sudo zypper update azure-cli
```

## <a name="uninstall"></a>卸载

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. 从系统中删除包。

    ```bash
    sudo zypper remove -y azure-cli
    ```

2. 如果不打算重新安装 CLI，请删除存储库信息。

   ```bash
   sudo zypper removerepo azure-cli
   ```

3. 如果不使用其他 Microsoft 包，请删除 Microsoft 签名密钥。

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## <a name="next-steps"></a>后续步骤

现在你已经安装了 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
