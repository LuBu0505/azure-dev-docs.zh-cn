---
title: 快速入门 - Terraform 入门 - Azure Cloud Shell
description: 本快速入门介绍如何安装和配置 Terraform 以创建 Azure 资源。
keywords: azure devops terraform 安装 配置 cloud shell init 计划 应用 执行 门户 登录 rbac 服务主体 自动化脚本
ms.topic: quickstart
ms.date: 06/01/2020
ms.openlocfilehash: 184d2720e3e2259a6c909d0775ffee20c0f30419
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329905"
---
# <a name="quickstart-getting-started-with-terraform---azure-cloud-shell"></a>快速入门：Terraform 入门 - Azure Cloud Shell
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍了如何在 [Azure Cloud Shell](/azure/cloud-shell/overview) 环境中开始使用 Terraform。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="configure-your-environment"></a>配置环境

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="open-cloud-shell"></a>打开 Cloud Shell

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 如果尚未登录，Azure 门户会显示可用的 Microsoft 帐户的列表。 选择与一个或多个活动 Azure 订阅相关联的 Microsoft 帐户，并输入你的凭据以继续。

1. 打开 Cloud Shell。

    ![访问 Cloud Shell](media/install-configure/portal-cloud-shell.png)

1. 如果以前未使用过 Cloud Shell，请配置环境和存储设置。 本文使用 Bash 环境。

## <a name="log-into-your-microsoft-account"></a>登录到 Microsoft 帐户

Cloud Shell 会自动使用你在登录 Azure 门户时所用的 Microsoft 帐户进行身份验证。 但是，如果你有多个 Microsoft 帐户和 Azure 订阅，可以使用 [az login](/cli/azure/reference-index?view=azure-cli-latest#az-login) 登录到这些帐户之一。 下面是两个使用 `az login` 命令的示例：

根据你的具体情况，选择以下方法之一：
    
- **你希望以用户身份登录**：在不使用任何参数的情况下运行 `az login` 命令将显示 URL 和代码。 浏览到 URL，输入代码，然后按照说明使用 Microsoft 帐户登录到 Azure。 在使用该命令登录后，返回到门户。

    ```azurecli-interactive
    az login
    ```

    **注意**：
    - 成功登录后，`az login` 命令会显示与已登录 Microsoft 帐户相关联的 Azure 订阅的列表。
    - 对于每个可用 Azure 订阅，都会显示一个属性列表。 `isDefault` 属性标识所使用的 Azure 订阅。 若要了解如何切换到另一个 Azure 订阅，请参阅[指定当前的 Azure 订阅](#specify-the-current-azure-subscription)部分。

- **你希望使用服务主体，但尚未拥有一个服务主体**：部署或使用 Azure 服务的自动化工具（例如 Terraform）应始终具有受限权限。 Azure 提供了服务主体，而不是让应用程序以具有完全特权的用户身份登录。 但是，如果没有用于登录的服务主体，该怎么办？ 在这种情况下，可以使用用户凭据登录，然后创建服务主体。 一旦创建了服务主体，就可以将其信息用于将来的登录尝试。

    [创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)时，有很多选项可供使用。 对于本文，我们将创建一个具有**参与者**角色（默认角色）的服务主体。 此**参与者**角色具有读取和写入到 Azure 帐户的完全权限。 有关基于角色的访问控制 (RBAC) 和角色的详细信息，请参阅 [RBAC：内置角色](/azure/active-directory/role-based-access-built-in-roles)。 
    
    使用 [az ad sp create-for-rbac](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) 命令，将 `<subscription_id>` 替换为要使用的订阅帐户的 ID。
    
    ```azurecli-interactive
    az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
    ```

    **注释**：
    - 成功完成后，`az ad sp create-for-rbac` 命令将显示多个值，包括自动生成的密码。 如果丢失了密码，则无法对其进行检索。 因此，应将密码存储在安全的位置。 如果忘记了密码，则需要[重置服务主体凭据](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#reset-credentials)。
    
- **使用 Azure 服务主体登录**：将以下 `az login` 命令中的占位符替换为你的服务主体的信息。

    ```azurecli-interactive
    az login --service-principal -u <service_principal_name> -p "<service_principal_password>" --tenant "<service_principal_tenant>"
    ```

    **注意**：
    - 成功登录后，`az login` 命令将显示 Azure 订阅的各种属性，如 `id` 和 `name`。

## <a name="specify-the-current-azure-subscription"></a>指定当前的 Azure 订阅

如前一部分中所述，可以通过以下两种方式登录到 Azure：

- **使用 Microsoft 帐户登录**：Microsoft 帐户可以与多个 Azure 订阅关联 - 其中一个订阅是默认订阅。 默认订阅是在你已登录并且未切换到其他订阅的情况下使用的订阅。
- **使用 Azure 服务主体登录**：服务主体特定于 Azure 订阅。 请记住，当你登录时，将显示订阅信息。

以下步骤适用于采用第一种方式的情况，在这种情况下需要执行以下任务：

- 验证当前的 Azure 订阅
- 列出当前 Microsoft 帐户的所有可用 Azure 订阅
- 切换到另一个 Azure 订阅

1. 若要验证当前的 Azure 订阅，请使用 [az account show](/cli/azure/account#az-account-show) 命令。

    ```azurecli-interactive
    az account show
    ```
    
1. 如果有权访问多个可用的 Azure 订阅，请使用 [az account list](/cli/azure/account#az-account-list) 显示订阅名称 ID 值的列表：

    ```azurecli-interactive
    az account list --query "[].{name:name, subscriptionId:id}"
    ```

1. 若要为当前 Cloud Shell 会话使用特定的 Azure 订阅，请使用 [az account set](/cli/azure/account#az-account-set) 命令。 将 `<subscription_id>` 占位符替换为要使用的订阅的 ID（或名称）：

    ```azurecli-interactive
    az account set --subscription="<subscription_id>"
    ```

    **注意**：
    - `az account set` 命令不显示切换到指定的 Azure 订阅的结果。 但是，可以使用 `az account show` 命令来确认当前的 Azure 订阅是否已更改。

## <a name="create-a-terraform-configuration-file"></a>创建 Terraform 配置文件

在本部分中，将使用 [Code Shell 编辑器](/azure/cloud-shell/using-cloud-shell-editor)来定义 Terraform 配置文件。

1. 将目录更改为 Cloud Shell 中的工作成果会保存到的已装载文件共享。 有关 Cloud Shell 如何保存文件的详细信息，请参阅[连接 Microsoft Azure 文件存储](/azure/cloud-shell/overview#connect-your-microsoft-azure-files-storage)
    
    ```bash
    cd clouddrive
    ```

1. 创建一个目录来保存用于此演示的 Terraform 文件。

    ```bash
    mkdir QuickstartTerraformTest
    ```

1. 将目录更改为演示目录。

    ```bash
    cd QuickstartTerraformTest
    ```

1. 使用你偏好的编辑器创建 Terraform 配置文件。 本文使用内置 Cloud Shell 编辑器。

    ```bash
    code QuickstartTerraformTest.tf
    ```
 
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

    **注意**：
    - `provider` 块指定使用 [Azure 提供程序 (`azurerm`)](https://www.terraform.io/docs/providers/azurerm/index.html)。
    - 在 `azurerm` 提供程序块中设置了 `version` 和 `features` 属性。 作为注释语句，其用法是特定于版本的。 有关如何为环境设置这些属性的详细信息，请参阅 [AzureRM 提供程序的 v2.0](https://www.terraform.io/docs/providers/azurerm/guides/2.0-upgrade-guide.html)。
    - 唯一的[资源声明](https://www.terraform.io/docs/configuration/resources.html)适用于资源类型 [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html)。 `azure_resource_group` 的两个必需参数是 `name` 和 `location`。

1. 保存文件 ( **&lt;Ctrl>S**)。

1. 退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="create-and-apply-a-terraform-execution-plan"></a>创建并应用 Terraform 执行计划

Cloud Shell 会自动安装最新版本的 Terraform。 此外，Terraform 还会自动使用来自当前 Azure 订阅的信息。 因此，无需进行安装或配置。 创建配置文件后，只需运行几个 Terraform 命令即可创建执行播放。 创建执行计划后，可对其进行验证和部署。

1. 使用 [Terraform init](https://www.terraform.io/docs/commands/init.html) 初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

    ```bash
    terraform init
    ```
    
1. 使用 Terraform 可以预览要通过 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 完成的操作。

    ```bash
    terraform plan
    ```

    注意：
    - `terraform plan` 命令将创建一个执行计划，但不会执行它。 它会确定创建配置文件中指定的配置需要执行哪些操作。
    - 使用 `terraform plan` 命令，可以在对实际资源进行任何更改之前验证执行计划是否符合预期。
    - 使用可选 `-out` 参数可以为计划指定输出文件。 有关使用 `-out` 参数的详细信息，请参阅[为稍后的部署保存执行计划](#persist-an-execution-plan-for-later-deployment)部分。

1. 使用 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 应用执行计划。

    ```bash
    terraform apply
    ```
    
1. Terraform 会显示在应用该执行计划后将发生的情况，并会要求你确认运行该计划。 输入 `yes` 并按 **Enter** 键确认执行命令。

1. 在确认执行播放后，请使用 [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) 来测试资源组是否已成功创建。

    ```azurecli-interactive
    az group show -n "QuickstartTerraformTest-rg"
    ```

    如果成功，该命令会显示新创建的资源组的各种属性。

## <a name="persist-an-execution-plan-for-later-deployment"></a>为稍后的部署保存执行计划

在上一部分中，你已了解如何运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 来创建执行计划。 然后，你了解了如何使用 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 应用该计划。 如果步骤是按顺序执行的交互式步骤，此模式适用。

对于更复杂的情况，可以将执行计划保存到文件中。 稍后（或者甚至从其他计算机上）可以应用该执行计划。

如果使用此功能，建议你阅读[自动运行 Terraform](https://learn.hashicorp.com/terraform/development/running-terraform-in-automation) 一文。

以下步骤说明了有关使用此功能的基本模式：

1. 运行 [terraform init](https://www.terraform.io/docs/commands/init.html)。

    ```bash
    terraform init
    ```

1. 在使用 `-out` 参数的情况下运行 `terraform plan`。

    ```bash
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

1. 运行 `terraform apply`（在该命令中指定来自上一步的文件的名称）。

    ```bash
    terraform apply QuickstartTerraformTest.tfplan
    ```

**注意**：
- 为了实现自动化，运行 `terraform apply <filename>` 不需要确认。
- 如果决定使用此功能，请阅读[安全警告部分](https://www.terraform.io/docs/commands/plan.html#security-warning)。

## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。

1. 运行 [terraform destroy](https://www.terraform.io/docs/commands/destroy.html)，该命令将逆转当前执行计划的结果。

    ```bash
    terraform destroy
    ```

1. Terraform 会显示在逆转该执行计划的结果后将发生的情况，并会要求你确认。 输入 `yes` 并按 **Enter** 键确认执行命令。

1. 确认执行播放后，输出将类似于以下示例，请使用 [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) 验证资源组是否已删除。

    ```azurecli-interactive
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **注意**：
    - 如果成功，`az group show` 命令将显示资源组不存在这一事实。

1. 将目录更改为父目录，并删除演示目录。 `-r` 参数会在删除目录之前删除目录内容。 目录内容包括之前创建的配置文件和 Terraform 状态文件。

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Azure VM](create-linux-virtual-machine-with-infrastructure.md)