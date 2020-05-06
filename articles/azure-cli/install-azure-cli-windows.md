---
title: 安装适用于 Windows 的 Azure CLI
description: 如何在 Windows 上安装 Azure CLI
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 05/01/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: c5e9118a04b0dc608309093866307fdc7083f591
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82030749"
---
# <a name="install-azure-cli-on-windows"></a>在 Windows 上安装 Azure CLI

在 Windows 上，Azure CLI 是通过 MSI 安装的，因此，可以通过 Windows 命令提示符 (CMD) 或 PowerShell 访问 CLI。
为适用于 Linux 的 Windows 子系统 (WSL) 安装时，可以安装适用于 Linux 分发版的包。 请参阅[安装主页](install-azure-cli.md)，获取受支持包管理器的列表，或者了解如何在 WSL 下手动进行安装。

[!INCLUDE [current-version](includes/current-version.md)]

## <a name="install-or-update"></a>安装或更新

MSI 分发版用于在 Windows 上安装或更新 Azure CLI。 在使用 MSI 安装程序之前，不需要卸载任何当前版本。

> [!div class="nextstepaction"]
> [下载 MSI 安装程序](https://aka.ms/installazurecliwindows)

当安装程序询问是否可以对计算机进行更改时，请单击“是”框。

也可使用 PowerShell 安装 Azure CLI。 以管理员身份启动 PowerShell 并运行以下命令：

   ```PowerShell
   Invoke-WebRequest -Uri https://aka.ms/installazurecliwindows -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
   ```
这将下载并安装适用于 Windows 的 Azure CLI 最新版本。 如果已安装一个版本，这将更新现有版本。 安装完成后，需要重新打开 PowerShell 以使用 Azure CLI。

现在可以通过 Windows 命令提示符或 PowerShell 使用 `az` 命令运行 Azure CLI 了。 PowerShell 提供了 Windows 命令提示符所不能提供的一些 Tab 键补全功能。 若要登录，请运行 [az login](/cli/azure/reference-index#az-login) 命令。

[!INCLUDE [interactive-login](includes/interactive-login.md)]

若要详细了解不同的身份验证方法，请参阅[使用 Azure CLI 登录](authenticate-azure-cli.md)。

## <a name="troubleshooting"></a>故障排除

以下是在 Windows 上安装时出现的一些常见问题。 如果遇到的问题未在本文中列出，请[在 GitHub 上提出问题](https://github.com/Azure/azure-cli/issues)。

### <a name="proxy-blocks-connection"></a>代理阻止连接

如果由于代理阻止连接而不能下载 MSI 安装程序，请确保已正确配置代理。 对于 Windows 10，这些设置是在 `Settings > Network & Internet > Proxy` 窗格中管理的。 如果要了解所需的设置，或者在计算机可能是配置管理型计算机或需要高级设置的情况下，请与系统管理员联系。

> [!IMPORTANT]
> 这些设置也需要能够通过 CLI（从 PowerShell 或命令提示符）访问 Azure 服务。 在 PowerShell 中，请使用以下命令执行此操作：
>
> ```powershell
> (New-Object System.Net.WebClient).Proxy.Credentials = `
>   [System.Net.CredentialCache]::DefaultNetworkCredentials
> ```

为了获取 MSI，代理必须允许与以下地址之间的 HTTPS 连接：

* `https://aka.ms/`
* `https://azurecliprod.blob.core.windows.net/`

## <a name="uninstall"></a>卸载

[!INCLUDE [uninstall-boilerplate.md](includes/uninstall-boilerplate.md)]

通过 Windows 中的“应用和功能”列表卸载 Azure CLI。 若要卸载：

| 平台 | 说明 |
|---|---|
| Windows 10 | “开始”>“设置”>“应用” |
| Windows 8<br/>Windows 7 | “开始”>“控制面板”>“程序”>“卸载程序” |

进入此屏幕后，请在程序搜索栏中键入 __Azure CLI__。 要卸载的程序列为“Microsoft CLI 2.0 for Azure”。  选择此应用程序，然后单击 `Uninstall` 按钮。

## <a name="next-steps"></a>后续步骤

现在你已经安装了 Azure CLI，下面简要介绍其功能和常用命令。

> [!div class="nextstepaction"]
> [Azure CLI 入门](get-started-with-azure-cli.md)
