---
title: 快速入门 - 在 Windows 上开始使用 Terraform
description: 本快速入门介绍如何安装和配置 Terraform 以创建 Azure 资源。
keywords: azure 开发 terraform 安装 配置 windows init 计划 应用 执行 登录 rbac 服务主体 自动化脚本 cli powershell
ms.topic: quickstart
ms.date: 06/12/2020
ms.openlocfilehash: d28a79af9a59551153f297cebfb0c51ed2e72838
ms.sourcegitcommit: 2d6c9687b39e33a6b5e980d9a375c9f8f1f2cab7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783799"
---
# <a name="quickstart-getting-started-with-terraform-using-windows"></a>快速入门：在 Windows 上开始使用 Terraform
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>配置环境

1. 在各大平台上（包括 Windows），建议将 PowerShell 版本 7.x 或更高版本用于 Azure PowerShell。 如果安装了 PowerShell，可以通过在 PowerShell 提示符处输入以下内容来验证版本：

    ```powershell
    $PSVersionTable.PSVersion
    ```
    
1. 按照 [在 Windows 上安装 PowerShell](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-windows?view=powershell-7) 一文中的说明安装最新版本的 PowerShell。

1. 本文使用 [Azure PowerShell Az 模块](https://docs.microsoft.com/powershell/azure/new-azureps-module-az)。 按照[在 Windows 上安装 Azure CLI](/cli/azure/install-azure-cli-windows?view=azure-cli-latest) 一文中的说明进行操作。

1. 打开刚刚安装的 PowerShell 版本的实例。

## <a name="log-into-azure"></a>登录到 Azure

有几个选项可用于登录到 Azure 订阅。

- [登录到 Microsoft 帐户](#log-into-your-microsoft-account)
- [使用 Azure 服务主体登录](#log-into-azure-using-an-azure-service-principal)

### <a name="log-into-your-microsoft-account"></a>登录到 Microsoft 帐户

不带任何参数调用 [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/Connect-AzAccount) 将显示 URL 和代码。 浏览到 URL，输入代码，然后按照说明使用 Microsoft 帐户登录到 Azure。 登录后，返回到门户。

```powershell
Connect-AzAccount
```

说明：
- 成功登录后，`Connect-AzAccount` 会显示与已登录 Microsoft 帐户相关联的默认 Azure 订阅。 若要了解如何切换到另一个 Azure 订阅，请参阅[指定当前的 Azure 订阅](#specify-the-current-azure-subscription)部分。

### <a name="log-into-azure-using-an-azure-service-principal"></a>使用 Azure 服务主体登录到 Azure

**创建 Azure 服务主体**：若要使用服务主体登录到 Azure 订阅，首先需要访问服务主体。 如果已有一个服务主体，则可以跳过该节的此部分。

部署或使用 Azure 服务的自动化工具（例如 Terraform）应始终具有受限权限。 Azure 提供了服务主体，而不是让应用程序以具有完全特权的用户身份登录。 但是，如果没有用于登录的服务主体，该怎么办？ 在这种情况下，可以使用用户凭据登录，然后创建服务主体。 一旦创建了服务主体，就可以将其信息用于将来的登录尝试。

[使用 PowerShell 创建服务主体](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps)时，有很多选项。 对于本文，我们将创建一个具有“参与者”角色的服务主体。 此“参与者”角色（默认角色）具有读取和写入到 Azure 帐户的完全权限。 有关基于角色的访问控制 (RBAC) 和角色的详细信息，请参阅 [RBAC：内置角色](/azure/active-directory/role-based-access-built-in-roles)。

调用 [New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/Az.Resources/New-AzADServicePrincipal) 可为指定的订阅创建服务主体。 成功完成后，将显示服务主体的信息，如服务主体名称和显示名称。 如果在不指定任何身份验证凭据的情况下调用 `New-AzADServicePrincipal`，则会自动生成密码。 但是，此密码不显示，因为它以 `SecureString` 类型返回。 因此，你需要调用 `New-AzADServicePrincipal`，并将结果发送到一个变量。 然后，可以查询该变量以获取密码。

1. 输入以下命令，将 `<subscription_id>` 替换为要使用的订阅帐户的 ID。

    ```powershell
    $sp = New-AzADServicePrincipal -Scope /subscriptions/<subscription_id>
    ```

1. 输入以下内容以显示服务主体的名称：

    ```powershell
    $sp.ServicePrincipalNames
    ```

1. 调用 `ConvertFrom-SecureString`，以文本形式显示密码：

    ```powershell
    $UnsecureSecret = ConvertFrom-SecureString -SecureString $sp.Secret -AsPlainText
    ```

说明：
- 此时，你已知道服务主体名称和密码。 使用服务主体登录到订阅时需要使用这些值。
- 如果丢失了密码，则无法对其进行检索。 因此，应将密码存储在安全的位置。 如果忘记了密码，则需要[重置服务主体凭据](https://docs.microsoft.com/powershell/azure/create-azure-service-principal-azureps#reset-credentials)。

**使用 Azure 服务主体登录**：若要使用服务主体登录到 Azure 订阅，请调用 `Connect-AzAccount`，并传入 [PsCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential) 类型的对象。 有两个选项：交互式和脚本。

- **交互式模式**：调用 [Get-Credential](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential) 并在要求提供凭据时输入。 对 `Get-Credential` 的调用将返回一个随后传递给 `Connect-AzAccount` 的 `PsCredential` 对象。

    1. 调用 `Get-Credential` 并手动输入服务主体名称和密码：

        ```powershell
        $Credential = Get-Credential
        ```

    2. 调用 `Connect-AzAccount`，同时传递 `PsCredential` 对象。 （将 `<azureSubscriptionTenantId>` 占位符替换为 Azure 订阅租户 ID。）

        ```powershell
        Connect-AzAccount -Credential $Credential -Tenant <azureSubscriptionTenantId> -ServicePrincipal
        ```

- **脚本模式**：构造 `PsCredential` 对象并将其传递给 `Connect-AzConnect`。

    1. 构造 `Get-Credential`。 （将占位符替换为你的 Azure 订阅和服务主体的相应值。）

        ```powershell
        $spName = "<servicePrincipalName>"
        $spPassword = ConvertTo-SecureString "<servicePrincipalPassword>" -AsPlainText -Force
        $psCredential = New-Object System.Management.Automation.PSCredential($spName , $spPassword)
        ```

    1. 调用 `Connect-AzAccount`，同时传递构造的 `PsCredential` 对象：

        ```powershell
        Connect-AzAccount -Credential $psCredential -TenantId "<azureSubscriptionTenantId>"  -ServicePrincipal
        ```

## <a name="specify-the-current-azure-subscription"></a>指定当前的 Azure 订阅

如前一部分中所述，可以通过以下两种方式登录到 Azure：

- **使用 Microsoft 帐户登录**：Microsoft 帐户可以与多个 Azure 订阅关联 - 其中一个订阅是默认订阅。 默认订阅是在你已登录并且未切换到其他订阅的情况下使用的订阅。
- **使用 Azure 服务主体登录**：服务主体特定于 Azure 订阅。 请记住，当你登录时，将显示订阅信息。

以下步骤适用于采用第一种方式的情况，在这种情况下需要执行以下任务：

- 查看当前的 Azure 订阅
- 列出当前 Microsoft 帐户的所有可用 Azure 订阅
- 切换到另一个 Azure 订阅

1. 若要查看当前的 Azure 订阅，请使用 [Get-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/get-azcontext)。

    ```powershell
    Get-AzContext
    ```

1. 如果有权访问多个可用 Azure 订阅，请使用 `Get-AzContext -ListAvailable`：

    ```powershell
    Get-AzContext -ListAvailable | Select Name
    ```

1. 自动生成的上下文名称可能很复杂。 若要使上下文名称更具可读性（更易于用作参数），可以重命名上下文。 可通过 [Rename-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/rename-azcontext) 重命名上下文。

    ```powershell
    Rename-AzContext -SourceName <current_context_name> -TargetName <new_context_Name>
    ```

1. 若要对当前 PowerShell 会话使用特定的 Azure 订阅，请使用 [Select-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/select-azcontext)。

    ```powershell
    Select-AzContext <context_name>
    ```

## <a name="configure-terraform"></a>配置 Terraform

1. [下载 Terraform](https://www.terraform.io/downloads.html)。

1. 从下载中，将可执行文件提取到所选的目录中。

1. [将系统的全局路径更新到可执行文件](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)。

1. 使用 `terraform` 命令验证全局路径配置。 如果找到 Terraform 可执行文件，则会显示可用 Terraform 选项的列表：

    ```powershell
    terraform
    ```

    如果找到 Terraform 可执行文件，它将列出语法和可用命令：

    ```output
    Usage: terraform [-version] [-help] <command> [args]

    The available commands for execution are listed below.
    The most common, useful commands are shown first, followed by
    less common or more advanced commands. If you're just getting
    started with Terraform, stick with the common commands. For the
    other commands, please read the help and docs before usage.
    ...
    ```

## <a name="create-a-terraform-configuration-file"></a>创建 Terraform 配置文件

本部分介绍如何创建用于创建 Azure 资源组的 Terraform 配置文件。

1. 创建一个目录来保存用于此演示的 Terraform 文件。

1. 将目录更改为演示目录。

1. 使用你偏好的编辑器创建一个名为 `QuickstartTerraformTest.tf` 的 Terraform 配置文件。

1. 将以下 HCL 代码粘贴到新文件中。

    ```hcl
    provider "azurerm" {
      # The "feature" block is required for AzureRM provider 2.x.
      # If you are using version 1.x, the "features" block is not allowed.
      version = "~>2.0"
      features {}
    }
    resource "azurerm_resource_group" "rg" {
            name = "QuickstartTerraformTest-rg"
            location = "eastus"
    }
    ```

    说明：
    - 提供程序块指定使用 [Azure 提供程序 (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html)。
    - 在 azurerm 提供程序块中，设置了版本和功能属性。 作为注释语句，其用法是特定于版本的。 有关如何为环境设置这些属性的详细信息，请参阅 [AzureRM 提供程序的 v2.0](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html)。
    - 唯一的[资源声明](https://www.terraform.io/docs/configuration/resources.html)适用于资源类型 [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html)。 Azure_resource_group 的两个必需参数是名称和位置。

## <a name="create-and-apply-a-terraform-execution-plan"></a>创建并应用 Terraform 执行计划

创建配置文件后，本节介绍如何创建执行计划并将其应用于云基础结构。

1. 使用 [Terraform init](https://www.terraform.io/docs/commands/init.html) 初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

    ```powershell
    terraform init
    ```
    
1. 使用 Terraform 可以预览要通过 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 完成的操作。

    ```powershell
    terraform plan
    ```

    注意：
    - `terraform plan` 命令将创建一个执行计划，但不会执行它。 它会确定创建配置文件中指定的配置需要执行哪些操作。
    - 使用 `terraform plan` 命令，可以在对实际资源进行任何更改之前验证执行计划是否符合预期。
    - 使用可选 `-out` 参数可以为计划指定输出文件。 有关使用 `-out` 参数的详细信息，请参阅[为稍后的部署保存执行计划](#persist-an-execution-plan-for-later-deployment)部分。

1. 使用 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 应用执行计划。

    ```powershell
    terraform apply
    ```
    
1. Terraform 会显示在应用该执行计划后将发生的情况，并会要求你确认运行该计划。 输入 `yes` 并按 **Enter** 键确认执行命令。

1. 在确认执行播放后，请使用 [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) 来测试资源组是否已成功创建。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    如果成功，该命令会显示新创建的资源组的各种属性。

## <a name="persist-an-execution-plan-for-later-deployment"></a>为稍后的部署保存执行计划

在上一部分中，你已了解如何运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 来创建执行计划。 然后，你了解了如何使用 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 应用该计划。 如果步骤是按顺序执行的交互式步骤，此模式适用。

对于更复杂的情况，可以将执行计划保存到文件中。 稍后（或者甚至从其他计算机上）可以应用该执行计划。

如果使用此功能，建议你阅读[自动运行 Terraform](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation) 一文。

以下步骤说明了有关使用此功能的基本模式：

1. 运行 [terraform init](https://www.terraform.io/docs/commands/init.html)。

    ```powershell
    terraform init
    ```

1. 在使用 `-out` 参数的情况下运行 `terraform plan`。

    ```powershell
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. 运行 `terraform apply`（在该命令中指定来自上一步的文件的名称）。

    ```powershell
    terraform apply QuickstartTerraformTest.tfplan
    ```

说明：
- 为了实现自动化，运行 `terraform apply <filename>` 不需要确认。
- 如果决定使用此功能，请阅读[安全警告部分](https://www.terraform.io/docs/commands/plan.html#security-warning)。

## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。

1. 运行 [terraform destroy](https://www.terraform.io/docs/commands/destroy.html)，该命令将逆转当前执行计划的结果。

    ```powershell
    terraform destroy
    ```

1. Terraform 会显示在逆转该执行计划的结果后将发生的情况，并会要求你确认。 输入 `yes` 并按 **Enter** 键确认执行命令。

1. 确认执行播放后，输出将类似于以下示例，请使用 [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) 验证资源组是否已删除。

    ```powershell
    Get-AzResourceGroup -Name QuickstartTerraformTest-rg
    ```

    说明：
    - 如果成功，`Get-AzResourceGroup` 会显示资源组不存在这一事实。

1. 将目录更改为父目录，并删除演示目录。 `-r` 参数会在删除目录之前删除目录内容。 目录内容包括之前创建的配置文件和 Terraform 状态文件。

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Azure VM](create-linux-virtual-machine-with-infrastructure.md)