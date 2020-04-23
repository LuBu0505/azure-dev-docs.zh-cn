---
title: 使用 Azure CLI 登录
description: 使用 Azure CLI 以交互方式登录或使用本地凭据登录
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 02/22/2019
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: 26112c9a7f338a7f178dc627d89f6e1f33d97714
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030859"
---
# <a name="sign-in-with-azure-cli"></a>使用 Azure CLI 登录 

Azure CLI 有多种身份验证类型。 最简单的入门方法是使用 [Azure Cloud Shell](/azure/cloud-shell/overview)，这样可以自动登录。
在本地，可以通过浏览器使用 [az login](/cli/azure/reference-index#az-login) 命令以交互方式登录。 编写脚本时，建议的方法是使用服务主体。 通过授予服务主体所需的最低适当权限，可以确保自动化的安全性。

CLI 不会存储任何登录信息。 [身份验证刷新令牌](https://docs.microsoft.com/azure/active-directory/develop/v1-id-and-access-tokens#refresh-tokens)由 Azure 生成并存储。 自 2018 年 8 月起，此令牌在不活动 90 天后将被撤销，但此值可由 Microsoft 或租户管理员更改。 令牌被撤销后，你会收到来自 CLI 的消息，指示你需要重新登录。

登录后，将针对默认订阅运行 CLI 命令。 如果你有多个订阅，可以[更改默认订阅](manage-azure-subscriptions-azure-cli.md)。

## <a name="sign-in-interactively"></a>以交互方式登录

Azure CLI 的默认身份验证方法是使用 Web 浏览器和访问令牌进行登录。

[!INCLUDE [interactive_login](includes/interactive-login.md)]

## <a name="sign-in-with-credentials-on-the-command-line"></a>在命令行中使用凭据登录。

在命令行中提供 Azure 用户凭据。

> [!Note]
> 此方法不适用于 Microsoft 帐户或已启用双重身份验证的帐户。

```azurecli-interactive
az login -u <username> -p <password>
```

> [!IMPORTANT]
> 如果想要避免在控制台中显示自己的密码并以交互方式使用 `az login`，请在 `bash` 下面使用 `read -s` 命令。
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login -u <username> -p $AZ_PASS
> ```
>
> 在 PowerShell 下，请使用 `Get-Credential` cmdlet。
>
> ```powershell
> $AzCred = Get-Credential -UserName <username>
> az login -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password
> ```

## <a name="sign-in-with-a-service-principal"></a>使用服务主体进行登录

服务主体是未绑定到任何特定用户的帐户，这些帐户具有通过预定义角色分配的权限。 使用服务主体进行身份验证是编写安全脚本或程序的最佳方法，因为这样可以同时应用权限限制和本地存储的静态凭据信息。 若要了解有关服务主体的详细信息，请参阅[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal)。

若要使用服务主体登录，需要：

* 与该服务主体关联的 URL 或名称
* 该服务主体的密码，或用于创建该服务主体的 X509 证书（PEM 格式）
* 与该服务主体关联的租户（`.onmicrosoft.com` 域或 Azure 对象 ID）

> [!NOTE]
> 必须将证书  追加到 PEM 文件中的私钥  之后。  有关 PEM 文件格式的示例，请参阅[使用 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli#sign-in-using-a-service-principal)。 
>

> [!IMPORTANT]
>
> 如果服务主体使用 Key Vault 中存储的证书，则该证书的私钥必须在未登录到 Azure 的情况下可用。 若要检索私钥以供脱机使用，请使用 [az keyvault secret show](/cli/azure/keyvault/secret)。

```azurecli-interactive
az login --service-principal -u <app-url> -p <password-or-cert> --tenant <tenant>
```

> [!IMPORTANT]
> 如果想要避免在控制台中显示自己的密码并以交互方式使用 `az login`，请在 `bash` 下面使用 `read -s` 命令。
>
> ```bash
> read -sp "Azure password: " AZ_PASS && echo && az login --service-principal -u <app-url> -p $AZ_PASS --tenant <tenant>
> ```
>
> 在 PowerShell 下，请使用 `Get-Credential` cmdlet。
>
> ```powershell
> $AzCred = Get-Credential -UserName <app-url>
> az login --service-principal -u $AzCred.UserName -p $AzCred.GetNetworkCredential().Password --tenant <tenant>
> ```

## <a name="sign-in-with-a-different-tenant"></a>使用其他租户身份登录

可以使用 `--tenant` 参数选择用于登录的租户。 此参数的值可以是 `.onmicrosoft.com` 域或租户的 Azure 对象 ID。 交互式登录方法和命令行登录方法都可以配合 `--tenant` 来使用。

```azurecli-interactive
az login --tenant <tenant>
```

## <a name="sign-in-with-a-managed-identity"></a>使用托管标识登录

在针对 Azure 资源托管标识配置的资源上，可以使用托管标识登录。 使用资源的标识登录时，登录操作通过 `--identity` 标记来完成。

```azurecli-interactive
az login --identity
```

若要详细了解 Azure 资源的托管标识，请参阅[配置 Azure 资源的托管标识](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm)和[使用 Azure 资源的托管标识进行登录](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-sign-in)。
