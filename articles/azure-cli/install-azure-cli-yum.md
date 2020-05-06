---
title: 使用 yum 在 Linux 上安装 Azure CLI
description: 如何使用 yum 安装 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 11/26/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: a98a51e4dc3ac85d27e27ef9b9164a7f98431d31
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82031109"
---
# <a name="install-azure-cli-with-yum"></a>使用 yum 安装 Azure CLI

对于附带 `yum` 的 Linux 分发版（例如 RHEL、Fedora 或 CentOS），可以安装适用于 Azure CLI 的包。 此包已在 RHEL 7.7、RHEL 8、Fedora 24 和更高版本、CentOS 7 和 CentOS 8 中测试。

[!INCLUDE [current-version](includes/current-version.md)]

[!INCLUDE [rpm-warning](includes/rpm-warning.md)]

## <a name="install"></a>安装

1. 导入 Microsoft 存储库密钥。

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```

2. 创建本地 `azure-cli` 存储库信息。

   ```bash
   sudo sh -c 'echo -e "[azure-cli]
   name=Azure CLI
   baseurl=https://packages.microsoft.com/yumrepos/azure-cli
   enabled=1
   gpgcheck=1
   gpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/azure-cli.repo'
   ```

3. 使用 `yum install` 命令安装。

   ```bash
   sudo yum install azure-cli
   ```

使用 `az` 命令运行 Azure CLI。 若要登录，请使用 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="troubleshooting"></a>故障排除

下面是使用 `yum` 安装时出现的一些常见问题。 如果遇到的问题未在本文中列出，请[在 github 上提出问题](https://github.com/Azure/azure-cli/issues)。

### <a name="install-on-rhel-76-or-other-systems-without-python-3"></a>安装在 RHEL 7.6 或不带 Python 3 的其他系统上

如果可以，请将系统升级到带有对 `python3` 包的官方支持的版本。 否则，需要首先安装 `python3` 包（[从源代码生成](https://github.com/linux-on-ibm-z/docs/wiki/Building-Python-3.6.x)，或者通过某个[其他存储库](https://developers.redhat.com/blog/2018/08/13/install-python3-rhel/)进行安装）。 然后即可下载包，并在没有依赖项的情况下安装它。
```bash
$ sudo yum install yum-utils
$ sudo yumdownloader azure-cli
$ sudo rpm -ivh --nodeps azure-cli-*.rpm
```

如果已安装 python3，但在尝试运行 cli 时仍收到错误 `python3: command not found`，则需要将其添加到你的路径。
```bash
$ scl enable rh-python36 bash
```

### <a name="proxy-blocks-connection"></a>代理阻止连接

[!INCLUDE[configure-proxy](includes/configure-proxy.md)]

你可能还想要显式配置 `yum` 以便在所有情况下都使用此代理。 请确保以下行显示在 `[main]` 的 `/etc/yum.conf` 部分下：

```yum.conf
[main]
# ...
proxy=http://[proxy]:[port] # If your proxy requires https, change http->https
proxy_username=[username] # Only required for basic auth
proxy_password=[password] # Only required for basic auth
```

为了获取 Microsoft 签名密钥并从我们的存储库中获取包，代理必须允许与以下地址之间的 HTTPS 连接：

* `https://packages.microsoft.com`

[!INCLUDE[troubleshoot-wsl.md](includes/troubleshoot-wsl.md)]

## <a name="update"></a>更新

使用 `yum update` 命令更新 Azure CLI。

```bash
sudo yum update azure-cli
```

## <a name="uninstall"></a>卸载

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

1. 从系统中删除包。

   ```bash
   sudo yum remove azure-cli
   ```

2. 如果不打算重新安装 CLI，请删除存储库信息。

   ```bash
   sudo rm /etc/yum.repos.d/azure-cli.repo
   ```

3. 如果不使用任何其他 Microsoft 包，请删除签名密钥。

   ```bash
   MSFT_KEY=`rpm -qa gpg-pubkey /* --qf "%{version}-%{release} %{summary}\n" | grep Microsoft | awk '{print $1}'`
   sudo rpm -e --allmatches gpg-pubkey-$MSFT_KEY
   ```

## <a name="next-steps"></a>后续步骤

现在你已经安装了 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
