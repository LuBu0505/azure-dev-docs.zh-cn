---
title: 快速入门 - 使用 Azure PowerShell 配置 Terraform
description: 本快速入门介绍如何使用 Azure PowerShell 安装和配置 Terraform。
keywords: azure devops terraform 安装 配置 windows init 计划 应用 执行 登录 rbac 服务主体 自动化脚本 powershell
ms.topic: quickstart
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 8f95d0bb09d7e9e7ea789b90a27178cdf5426d74
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401531"
---
# <a name="quickstart-configure-terraform-using-azure-powershell"></a>快速入门：使用 Azure PowerShell 配置 Terraform
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍如何在 PowerShell 中开始使用 [Azure 上的 Terraform](https://www.terraform.io/docs/providers/azurerm/index.html)。

在本文中，学习如何：
> [!div class="checklist"]
> * 安装最新版本的 PowerShell
> * 安装新的 PowerShell Az 模块
> * 安装 Azure CLI
> * 安装 Terraform
> * 创建 Azure 服务主体来进行身份验证
> * 使用服务主体登录到 Azure 
> * 设置环境变量，以便 Terraform 正确地对 Azure 订阅进行身份验证
> * 创建基本 Terraform 配置文件
> * 创建并应用 Terraform 执行计划
> * 撤消执行计划

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>配置环境

1. 允许与 Azure 资源交互的最新 PowerShell 模块称为 [Azure PowerShell Az 模块](/powershell/azure/new-azureps-module-az)。 在你使用 Azure PowerShell Az 模块时，我们建议你在所有平台上使用 PowerShell 7（或更高版本）。 如果安装了 PowerShell，可以通过在 PowerShell 提示符处输入以下命令来验证版本。

    ```powershell
    $PSVersionTable.PSVersion
    ```

1. [安装 PowerShell](/powershell/scripting/install/installing-powershell-core-on-windows)。 此演示已在 Windows 10 上使用 PowerShell 7.0.2 进行了测试。

1. 若要[使 Terraform 向 Azure 进行身份验证](https://www.terraform.io/docs/providers/azurerm/guides/azure_cli.html)，则需要[安装 Azure CLI](/cli/azure/install-azure-cli-windows)。 此演示使用 Azure CLI 2.9.1 版本进行了测试。

1. [下载 Terraform](https://www.terraform.io/downloads.html)。

1. 从下载中，将可执行文件提取到所选的目录中。

1. [将系统的全局路径更新到可执行文件](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)。

1. 使用 `terraform` 命令验证全局路径配置。

    ```powershell
    terraform
    ```

    **注释**：
    - 如果找到 Terraform 可执行文件，它将列出语法和可用命令。

## <a name="authenticate-to-azure"></a>向 Azure 进行身份验证

使用 PowerShell 和 Terraform 时，必须使用服务主体登录。 接下来的两个部分将说明以下任务：

- [创建 Azure 服务主体](#create-an-azure-service-principal)
- [使用服务主体登录到 Azure](#log-in-to-azure-using-a-service-principal)


### <a name="span-idcreate-an-azure-service-principalcreate-an-azure-service-principal"></a><span id="create-an-azure-service-principal"/>创建 Azure 服务主体

若要使用服务主体登录到 Azure 订阅，首先需要访问服务主体。 如果已有一个服务主体，则可以跳过此节。

[使用 PowerShell 创建服务主体](/powershell/azure/create-azure-service-principal-azureps)时，有很多选项。 对于本文，我们将创建一个具有“参与者”角色的服务主体。 此“参与者”角色（默认角色）具有读取和写入到 Azure 帐户的完全权限。 有关基于角色的访问控制 (RBAC) 和角色的详细信息，请参阅 [RBAC：内置角色](/azure/active-directory/role-based-access-built-in-roles)。

调用 [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) 可为指定的订阅创建服务主体。 成功完成后，将显示服务主体的信息，如服务主体名称和显示名称。 如果在不指定任何身份验证凭据的情况下调用 `New-AzADServicePrincipal`，则会自动生成密码。 但是，此密码不显示，因为它以 `SecureString` 类型返回。 因此，你需要调用 `New-AzADServicePrincipal`，并将结果发送到一个变量。 然后，可以将变量转换为纯文本以显示它。

1. 获取要使用的 Azure 订阅的订阅 ID。 如果不知道订阅 ID，可以从 [Azure 门户](https://portal.azure.com/)获取该值。

    1. 登录到 [Azure 门户](https://portal.azure.com/)。
    1. 在“Azure 服务”下，选择“订阅” 。
    1. 订阅表列表包含一个具有每个订阅的 ID 的列。

1. 启动 PowerShell。

1. 使用 [New-AzADServicePrincipal](/powershell/module/az.resources/new-azadserviceprincipal) 创建新的服务主体。 将 `<azure_subscription_id>` 替换为要使用的 Azure 订阅的 ID。

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<azure_subscription_id>
    ```

1. 显示服务主体的名称。

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. 将自动生成的密码显示为文本 [ConvertFrom-SecureString](/powershell/module/microsoft.powershell.security/convertfrom-securestring)。

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

**注释**：

- 使用服务主体登录订阅需要服务主体名称和密码值。
- 如果丢失了密码，则无法对其进行检索。 因此，应将密码存储在安全的位置。 如果忘记了密码，则需要[重置服务主体凭据](/powershell/azure/create-azure-service-principal-azureps#reset-credentials)。

### <a name="span-idlog-in-to-azure-using-a-service-principallog-in-to-azure-using-a-service-principal"></a><span id="log-in-to-azure-using-a-service-principal"/>使用服务主体登录到 Azure

若要使用服务主体登录到 Azure 订阅，请调用 [Connect-AzAccount](/powershell/module/az.accounts/Connect-AzAccount)，指定类型为 [PsCredential](/dotnet/api/system.management.automation.pscredential) 的对象。

1. 启动 PowerShell。

1. 使用以下方法之一获取 [PsCredential](/dotnet/api/system.management.automation.pscredential) 对象。

    1. 调用 [Get-Credential](/powershell/module/microsoft.powershell.security/get-credential) 并输入服务主体名称和密码（如有要求）：

        ```powershell
        $spCredential = Get-Credential
        ```

    1. 在内存中构造 `PsCredential` 对象。 将占位符替换为服务主体的相应值。 此模式是从脚本登录的方式。

        ```powershell
        $spName = "<service_principal_name>"
        $spPassword = ConvertTo-SecureString "<service_principal_password>" -AsPlainText -Force
        $spCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

1. 调用 `Connect-AzAccount`，同时传递 `PsCredential` 对象。 将 `<azure_subscription_tenant_id>` 占位符替换为 Azure 订阅租户 ID。

    ```powershell
    Connect-AzAccount -Credential $spCredential -Tenant "<azure_subscription_tenant_id>" -ServicePrincipal
    ```

## <a name="set-environment-variables"></a>设置环境变量

为了让 Terraform 使用预期的 Azure 订阅，请设置环境变量。 你可以在 Windows 系统级别或在特定的 PowerShell 会话中设置环境变量。 如果要为特定会话设置环境变量，请使用以下代码。 将占位符替换为你的环境的相应值。

```powershell
$env:ARM_CLIENT_ID="<service_principal_app_id>"
$env:ARM_SUBSCRIPTION_ID="<azure_subscription_id>"
$env:ARM_TENANT_ID="<azure_subscription_tenant_id>"
```

[!INCLUDE [terraform-create-base-config-file.md](includes/terraform-create-base-config-file.md)]

[!INCLUDE [terraform-create-and-apply-execution-plan.md](includes/terraform-create-and-apply-execution-plan.md)]

[!INCLUDE [terraform-reverse-execution-plan.md](includes/terraform-reverse-execution-plan.md)]

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Linux VM](create-linux-virtual-machine-with-infrastructure.md)
