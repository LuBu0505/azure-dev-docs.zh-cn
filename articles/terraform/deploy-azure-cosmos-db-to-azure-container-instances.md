---
title: 教程 - 将 Azure Cosmos DB 部署到 Azure 容器实例
description: 了解如何在将 Azure Cosmos DB 部署到 Azure 容器实例时创建简单的 Terraform 配置。
ms.topic: tutorial
ms.date: 04/26/2020
ms.openlocfilehash: 88bcb862389a4cbfc611f7beab797e7601c49258
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "82171013"
---
# <a name="tutorial-deploy-an-azure-cosmos-db-to-azure-container-instances"></a>教程：将 Azure Cosmos DB 部署到 Azure 容器实例

在本教程中，请使用 Terraform 创建 Azure 资源。 本文中的相关步骤将引导你逐步创建以下资源：

> [!div class="checklist"]
> * Azure Cosmos DB 实例
> * Azure 容器实例
> * 适用于这两种资源的应用

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="create-first-configuration"></a>创建第一个配置

在本部分中，请为 Azure Cosmos DB 实例创建配置。

1. 登录 [Azure 门户](https://go.microsoft.com/fwlink/p/?LinkID=525040)。

1. 打开 Azure Cloud Shell。

1. 启动 Cloud Shell 编辑器：

    ```bash
    code main.tf
    ```

1. 此步骤中的配置为两个 Azure 资源建模。 这些资源包括一个 Azure 资源组和一个 Azure Cosmos DB 实例。 随机整数用于创建唯一的 Cosmos DB 实例名称。 还可以配置多个 Cosmos DB 设置。 有关详细信息，请参阅 [Cosmos DB Terraform 参考](https://www.terraform.io/docs/providers/azurerm/r/cosmosdb_account.html)。 复制以下 Terraform 配置并将其粘贴到编辑器中：

    ```hcl
    resource "azurerm_resource_group" "vote-resource-group" {
      name     = "vote-resource-group"
      location = "westus"
    }

    resource "random_integer" "ri" {
      min = 10000
      max = 99999
    }

    resource "azurerm_cosmosdb_account" "vote-cosmos-db" {
      name                = "tfex-cosmos-db-${random_integer.ri.result}"
      location            = azurerm_resource_group.vote-resource-group.location
      resource_group_name = azurerm_resource_group.vote-resource-group.name
      offer_type          = "Standard"
      kind                = "GlobalDocumentDB"

      consistency_policy {
        consistency_level       = "BoundedStaleness"
        max_interval_in_seconds = 10
        max_staleness_prefix    = 200
      }

      geo_location {
        location          = "westus"
        failover_priority = 0
      }
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

## <a name="run-the-configuration"></a>运行配置

在本部分中，请使用多个 Terraform 命令运行配置。

1. [terraform init](https://www.terraform.io/docs/commands/init.html) 命令初始化工作目录。 在 Cloud Shell 中运行以下命令：

    ```bash
    terraform init
    ```

1. [terraform plan](https://www.terraform.io/docs/commands/plan.html) 命令可用于验证配置语法。 `-out` 参数可将结果定向到文件。 稍后可以使用输出文件应用配置。 在 Cloud Shell 中运行以下命令：

    ```bash
    terraform plan --out plan.out
    ```

1. [terraform apply](https://www.terraform.io/docs/commands/apply.html) 命令用于应用配置。 指定上一步中的输出文件。 此命令将创建 Azure 资源。 在 Cloud Shell 中运行以下命令：

    ```bash
    terraform apply plan.out
    ```

1. 若要在 Azure 门户中验证结果，请浏览到新资源组。 新的 Azure Cosmos DB 实例位于新的资源组中。

## <a name="update-configuration"></a>更新配置

本部分介绍如何更新配置以包含 Azure 容器实例。 容器将运行一个在 Cosmos DB 中读取和写入数据的应用程序。

1. 启动 Cloud Shell 编辑器：

    ```bash
    code main.tf
    ```

1. 此步骤中的配置设置了两个环境变量：`COSMOS_DB_ENDPOINT` 和 `COSMOS_DB_MASTERKEY`。 这些变量保存位置以及用于访问数据库的密钥。 这些变量的值是从上一步中创建的数据库实例中获取的。 此过程称为内插。 若要详细了解 Terraform 内插，请参阅[内插语法](https://www.terraform.io/docs/configuration/interpolation.html)。 该配置还包含一个输出块，该块返回容器实例的完全限定域名 (FQDN)。 复制以下代码并将其粘贴到编辑器中：

    ```hcl
    resource "azurerm_container_group" "vote-aci" {
      name                = "vote-aci"
      location            = azurerm_resource_group.vote-resource-group.location
      resource_group_name = azurerm_resource_group.vote-resource-group.name
      ip_address_type     = "public"
      dns_name_label      = "vote-aci"
      os_type             = "linux"

      container {
        name   = "vote-aci"
        image  = "microsoft/azure-vote-front:cosmosdb"
        cpu    = "0.5"
        memory = "1.5"
        ports {
          port     = 80
          protocol = "TCP"
        }

        secure_environment_variables = {
          "COSMOS_DB_ENDPOINT"  = azurerm_cosmosdb_account.vote-cosmos-db.endpoint
          "COSMOS_DB_MASTERKEY" = azurerm_cosmosdb_account.vote-cosmos-db.primary_master_key
          "TITLE"               = "Azure Voting App"
          "VOTE1VALUE"          = "Cats"
          "VOTE2VALUE"          = "Dogs"
        }
      }
    }

    output "dns" {
      value = azurerm_container_group.vote-aci.fqdn
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S**) 并退出编辑器 ( **&lt;Ctrl>Q**)。

1. 按上述部分进行操作，运行以下命令以查看要进行的更改：

    ```bash
    terraform plan --out plan.out
    ```

1. 运行 `terraform apply` 命令以应用配置。

    ```bash
    terraform apply plan.out
    ```

1. 记下容器实例 FQDN。

## <a name="test-application"></a>测试应用程序

若要测试应用程序，请导航到容器实例的 FQDN。 应看到结果类似于以下输出：

![Azure Vote 应用程序](media/deploy-azure-cosmos-db-to-azure-container-instances/azure-vote.jpg)

## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。

运行 [terraform destroy](https://www.terraform.io/docs/commands/destroy.html) 命令以删除在本教程中创建的 Azure 资源：

```bash
terraform destroy -auto-approve
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [安装并配置 Terraform 以预配 Azure 资源](install-configure.md)。