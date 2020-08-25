---
title: 快速入门 - 使用 Azure Cloud Shell 配置 Terraform
description: 本快速入门介绍如何安装和配置 Terraform 以创建 Azure 资源。
keywords: azure devops terraform 安装 配置 cloud shell init 计划 应用 执行 门户 登录 rbac 服务主体 自动化脚本
ms.topic: quickstart
ms.date: 08/08/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: d8cec2954357269b5605a7b35c96030b8e8b5fa0
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88241169"
---
# <a name="quickstart-configure-terraform-using-azure-cloud-shell"></a>快速入门：使用 Azure Cloud Shell 配置 Terraform
 
[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍如何开始使用 [Azure 上的 Terraform](https://www.terraform.io/docs/providers/azurerm/index.html)。

在本文中，学习如何：
> [!div class="checklist"]
> * 使用 `az login` 对 Azure 进行身份验证
> * 使用 Azure CLI 创建 Azure 服务主体
> * 使用服务主体对 Azure 进行身份验证
> * 设置当前的 Azure 订阅 - 有多个订阅时使用
> * 编写 Terraform 脚本以创建 Azure 资源组
> * 创建并应用 Terraform 执行计划
> * 使用 `terraform plan -destroy` 标志来撤消执行计划

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="configure-your-environment"></a>配置环境

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 如果尚未登录，Azure 门户会显示可用的 Microsoft 帐户的列表。 请选择与一个或多个活动 Azure 订阅相关联的 Microsoft 帐户，并输入你的凭据以继续。

1. 打开 Cloud Shell。

    ![访问 Cloud Shell](media/install-configure/portal-cloud-shell.png)

1. 如果以前未使用过 Cloud Shell，请配置环境和存储设置。 本文使用 Bash 环境。

**注释**：
- Cloud Shell 会自动安装最新版本的 Terraform。 此外，Terraform 还会自动使用来自当前 Azure 订阅的信息。 因此，无需进行安装或配置。

## <a name="authenticate-to-azure"></a>向 Azure 进行身份验证

Cloud Shell 会自动使用你在登录 Azure 门户时所用的 Microsoft 帐户进行身份验证。 如果帐户具有多个 Azure 订阅，可以[切换到你的另一个订阅](#set-the-current-azure-subscription)。

Terraform 支持多种向 Azure 进行身份验证的选项。 本文介绍以下方法：

- 在你以交互方式使用 Terraform 时，我们建议你[通过 Microsoft 帐户进行身份验证](#authenticate-via-microsoft-account)。
- 在你从代码中使用 Terraform 时，我们建议你[通过 Azure 服务主体进行身份验证](#authenticate-via-azure-service-principal)。

### <a name="authenticate-via-microsoft-account"></a>通过 Microsoft 帐户进行身份验证

在不使用任何参数的情况下调用 `az login` 将显示 URL 和代码。 浏览到 URL，输入代码，然后按照说明使用 Microsoft 帐户登录到 Azure。 登录后，返回到门户。

```azurecli
az login
```

**注释**：

- 成功登录后，`az login` 会显示与已登录 Microsoft 帐户相关联的 Azure 订阅的列表。
- 对于每个可用 Azure 订阅，都会显示一个属性列表。 `isDefault` 属性标识所使用的 Azure 订阅。 若要了解如何切换到另一个 Azure 订阅，请参阅[设置当前的 Azure 订阅](#set-the-current-azure-subscription)一节。

### <a name="authenticate-via-azure-service-principal"></a>通过 Azure 服务主体进行身份验证

**创建 Azure 服务主体**：若要使用服务主体登录到 Azure 订阅，首先需要访问服务主体。 如果已有一个服务主体，则可以跳过该节的此部分。

部署或使用 Azure 服务的自动化工具（例如 Terraform）应始终具有受限权限。 Azure 提供了服务主体，而不是让应用程序以具有完全特权的用户身份登录。 但是，如果没有用于登录的服务主体，该怎么办？ 在这种情况下，可以使用用户凭据登录，然后创建服务主体。 一旦创建了服务主体，就可以将其信息用于将来的登录尝试。

[使用 Azure CLI 创建服务主体](/cli/azure/create-an-azure-service-principal-azure-cli?)时，有很多选项。 对于本文，我们将使用 [az ad sp create-for-rbac](/cli/azure/ad/sp?#az-ad-sp-create-for-rbac) 创建一个具有“参与者”角色的服务主体。 此“参与者”角色（默认角色）具有读取和写入到 Azure 帐户的完全权限。 有关基于角色的访问控制 (RBAC) 和角色的详细信息，请参阅 [RBAC：内置角色](/azure/active-directory/role-based-access-built-in-roles)。

输入以下命令，将 `<subscription_id>` 替换为要使用的订阅帐户的 ID。

```azurecli
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription_id>"
```

**注释**：

- 成功完成后，`az ad sp create-for-rbac` 会显示多个值。 下一步中将使用 `name`、`password` 和 `tenant` 值。
- 如果丢失了密码，则无法对其进行检索。 因此，应将密码存储在安全的位置。 如果忘记了密码，则需要[重置服务主体凭据](/cli/azure/create-an-azure-service-principal-azure-cli#reset-credentials)。

**使用 Azure 服务主体登录**：在以下对 `az login` 的调用中，将占位符替换为你的服务主体的信息。

```azurecli
az login --service-principal -u <service_principal_name> -p "<service_principal_password>" --tenant "<service_principal_tenant>"
```

## <a name="set-the-current-azure-subscription"></a>设置当前的 Azure 订阅

Microsoft 帐户可以与多个 Azure 订阅相关联。 以下步骤概述了如何在订阅之间进行切换：

1. 若要查看当前的 Azure 订阅，请使用 [az account show](/cli/azure/account#az-account-show)。

    ```azurecli
    az account show
    ```

1. 如果有权访问多个可用的 Azure 订阅，请使用 [az account list](/cli/azure/account#az-account-list) 显示订阅名称 ID 值的列表：

    ```azurecli
    az account list --query "[].{name:name, subscriptionId:id}"
    ```

1. 若要为当前 Cloud Shell 会话使用特定的 Azure 订阅，请使用 [az account set](/cli/azure/account#az-account-set)。 将 `<subscription_id>` 占位符替换为要使用的订阅的 ID（或名称）：

    ```azurecli
    az account set --subscription="<subscription_id>"
    ```

    **注意**：

    - 调用 `az account set` 不会显示切换到指定的 Azure 订阅的结果。 但是，可以使用 `az account show` 来确认当前的 Azure 订阅是否已更改。

## <a name="create-a-terraform-configuration-file"></a>创建 Terraform 配置文件

本部分介绍如何创建用于创建 Azure 资源组的 Terraform 配置文件。

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

1. 使用你偏好的编辑器创建 Terraform 配置文件。 本文使用内置 [Cloud Shell 编辑器](/azure/cloud-shell/using-cloud-shell-editor)。

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

在本节中，你将创建一个执行计划，并将其应用于云基础结构。

1. 使用 [Terraform init](https://www.terraform.io/docs/commands/init.html) 初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

    ```bash
    terraform init
    ```

1. 运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 以基于 Terraform 配置文件创建一个执行计划。

    ```bash
    terraform plan -out QuickstartTerraformTest.tfplan
    ```

    注意：
    - `terraform plan` 命令将创建一个执行计划，但不会执行它。 它会确定创建配置文件中指定的配置需要执行哪些操作。 此模式允许你在对实际资源进行任何更改之前验证执行计划是否符合预期。
    - 使用可选 `-out` 参数可以为计划指定输出文件。 使用 `-out` 参数可以确保所查看的计划与所应用的计划完全一致。
    - 若要详细了解如何使执行计划和安全性持久化，请参阅[安全警告一节](https://www.terraform.io/docs/commands/plan.html#security-warning)。

1. 运行 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 以应用执行计划。

    ```bash
    terraform apply QuickstartTerraformTest.tfplan
    ```

1. 应用执行计划后，可使用 [az group show](/cli/azure/group?#az-group-show) 测试资源组是否已成功创建。

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **注释**：

    - 如果成功，`az group show` 会显示新创建的资源组的各种属性。

## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。

1. 运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 以创建执行计划，销毁 Terraform 配置文件中指示的资源。

    ```bash
    terraform plan -destroy -out QuickstartTerraformTest.destroy.tfplan
    ```

    **注释**：
    - `terraform plan` 命令将创建一个执行计划，但不会执行它。 它会确定创建配置文件中指定的配置需要执行哪些操作。 此模式允许你在对实际资源进行任何更改之前验证执行计划是否符合预期。
    - `-destroy` 参数会生成一个用于销毁资源的计划。
    - 使用可选 `-out` 参数可以为计划指定输出文件。 使用 `-out` 参数可以确保所查看的计划与所应用的计划完全一致。
    - 若要详细了解如何使执行计划和安全性持久化，请参阅[安全警告一节](https://www.terraform.io/docs/commands/plan.html#security-warning)。

1. 运行 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 以应用执行计划。

    ```bash
    terraform apply QuickstartTerraformTest.destroy.tfplan
    ```

1. 使用 [az group show](/cli/azure/group?#az-group-show) 验证资源组是否已删除。

    ```azurecli
    az group show -n "QuickstartTerraformTest-rg"
    ```

    **注释**：
    - 如果成功，`az group show` 会显示资源组不存在这一事实。

1. 将目录更改为父目录，并删除演示目录。 `-r` 参数会在删除目录之前删除目录内容。 目录内容包括之前创建的配置文件和 Terraform 状态文件。

    ```bash
    cd .. && rm -r QuickstartTerraformTest
    ```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Azure VM](create-linux-virtual-machine-with-infrastructure.md)