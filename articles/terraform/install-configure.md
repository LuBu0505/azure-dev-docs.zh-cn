---
title: 快速入门 - 安装并配置 Terraform 以预配 Azure 资源
description: 了解如何安装和配置 Terraform 以创建 Azure 资源。
keywords: azure devops terraform 安装 配置
ms.topic: quickstart
ms.date: 04/26/2020
ms.openlocfilehash: e395227171417ccf73ef073a6067732fb11a418d
ms.sourcegitcommit: 756e4873f904db954a56c20ebb2f1f5116ee4596
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82169733"
---
# <a name="quickstart-install-and-configure-terraform-to-provision-azure-resources"></a>快速入门：安装并配置 Terraform 以预配 Azure 资源
 
借助 Terraform，可以轻松使用[简单模板语言](https://www.terraform.io/docs/configuration/syntax.html)来定义、预览和部署云基础结构。 本文介绍使用 Terraform 在 Azure 中预配资源的必要步骤。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="install-terraform"></a>安装 Terraform

默认情况下会安装 Terraform 的最新版本，以便在 [Azure Cloud Shell](/azure/cloud-shell/overview) 中使用。 如果选择在本地安装 Terraform，请完成本步骤；否则请继续[配置 Terraform 对 Azure 的访问权限](#configure-terraform-access-to-azure)。

1. [安装 Terraform](https://www.terraform.io/downloads.html)（请指定适用于你的操作系统的程序包）。
1. 下载的程序包中包含一个可执行文件。 基于你的操作系统，定义可执行文件的全局路径：
    - [Linux 或 macOS](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)
    - [Windows](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)。
1. 使用 `terraform` 命令验证全局路径配置。 如果找到了 Terraform 并运行它，则会显示可用 Terraform 选项的列表：

    ```console
    azureuser@Azure:~$ terraform
    Usage: terraform [--version] [--help] <command> [args]
    ```
    
## <a name="configure-terraform-access-to-azure"></a>配置 Terraform 对 Azure 的访问权限

要使 Terraform 能够将资源预配到 Azure，请创建 [Azure AD 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli)。 服务主体允许你的 Terraform 脚本在 Azure 订阅中预配资源。

如果有多个 Azure 订阅，请先使用 [az account list](/cli/azure/account#az-account-list) 查询帐户，以获取订阅 ID 和租户 ID 值列表：

```azurecli-interactive
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

若要使用所选订阅，请使用 [az account set](/cli/azure/account#az-account-set) 为此会话设置订阅。 设置 `SUBSCRIPTION_ID` 环境变量，用于保存从要使用的订阅返回的 `id` 字段值：

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

现在，可以创建一个服务主体以与 Terraform 一起使用。 使用 [az ad sp create-for-rbac](/cli/azure/ad/sp#az-ad-sp-create-for-rbac)，并将“范围”设置为你的订阅，如下所示  ：

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

会返回你的 `appId`、`password`、`sp_name` 和 `tenant`。 记下 `appId` 和 `password`。

## <a name="configure-terraform-environment-variables"></a>配置 Terraform 环境变量

若要配置 Terraform 以使用 Azure AD 服务主体，请设置以下环境变量，然后由 [Azure Terraform 模块](https://registry.terraform.io/modules/Azure)使用这些环境变量。 如果使用的是 Azure 云而不是 Azure 公共域，则还可设置环境。

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

可以使用以下示例 shell 脚本设置这些变量：

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>运行示例脚本

在空白目录下创建文件 `test.tf`，并粘贴到以下脚本中。

```hcl
provider "azurerm" {
  # The "feature" block is required for AzureRM provider 2.x. 
  # If you are using version 1.x, the "features" block is not allowed.
  version = "~>2.0"
  features {}
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

保存该文件，然后初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

```bash
terraform init
```

输出类似于以下示例：

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

可以使用 `terraform plan` 预览 Terraform 脚本要完成的操作。 准备好创建资源组时，请按如下所示应用 Terraform 计划：

```bash
terraform apply
```

输出类似于以下示例：

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>后续步骤

在本文中，你安装了 Terraform 或使用 Cloud Shell 配置了 Azure 凭据并开始在 Azure 订阅中创建资源。 若要在 Azure 中创建更完整的 Terraform 部署，请参阅以下文章：

> [!div class="nextstepaction"]
> [使用 Terraform 创建 Azure VM](create-linux-virtual-machine-with-infrastructure.md)