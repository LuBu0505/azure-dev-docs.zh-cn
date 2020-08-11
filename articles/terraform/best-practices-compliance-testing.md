---
title: 教程 - 使用 Terraform 和 Azure 进行符合性测试
description: 了解如何将行为驱动开发 (BDD) 样式符合性测试应用于 Terraform 配置
ms.topic: tutorial
ms.date: 07/31/2020
ms.openlocfilehash: 38358be8bc7626d7b8a6b2df7e5da9b53d98618e
ms.sourcegitcommit: b224b276a950b1d173812f16c0577f90ca2fbff4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/05/2020
ms.locfileid: "87810610"
---
# <a name="tutorial-compliance-testing-with-terraform-and-azure"></a>教程：使用 Terraform 和 Azure 进行符合性测试

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍如何执行以下任务：

> [!div class="checklist"]
> * 了解何时采用符合性测试
> * 了解如何执行符合性检查

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>必备条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **安装 Terraform**：根据你的环境，[下载并安装 Terraform](https://www.terraform.io/downloads.html)。
- **Docker**：[安装 Docker](https://docs.docker.com/get-docker/)
- **Terraform-compliance**：[安装 Terraform 符合性工具](https://terraform-compliance.com/pages/installation/docker)。
- **创建测试示例的分支**：创建 [GitHub 上的 Terraform 示例](https://github.com/Azure/terraform)的分支，并将其复制到开发/测试计算机。

## <a name="what-is-compliance-testing"></a>什么是符合性测试

符合性测试是一种非功能性测试技术，用于确定系统是否符合规定的标准。 符合性测试也称为一致性测试。

大多数软件团队会进行分析来检查标准是否得到恰当地采用和实施。 通常，这些团队共同协作来提升标准，这转而又会促进质量的提升。

符合性测试可确保每个开发生命周期阶段的结果符合商定的要求。

在项目开始时，应将符合性检查集成到开发周期中。 如果要求本身未得到充分记录，那么尝试在稍后的阶段添加符合性检查会变得愈发困难。

## <a name="understanding-compliance-checks"></a>了解符合性检查

执行符合性检查非常简单。 我们针对开发生命周期的每个阶段制定和记录了一套标准和流程。 每个阶段的结果都与记录的要求进行比较。 测试结果是指不符合预定标准的任何“差距”。 通过检查过程完成符合性测试，评审过程的结果应记录下来。

我们来看一个具体的示例。

一个常见的问题是，当多名开发人员应用不兼容的更改时，环境会崩溃。 假设有个人进行更改并应用了资源，例如在测试环境中创建虚拟机 (VM)。 之后，另一个人应用了不同版本的代码来预配该 VM 的另一版本。 需要在这里进行监督，确保符合设定规则。

要解决此问题，一种方法是定义标记资源的策略，例如使用 `role` 和 `creator` 标记。 定义策略后，[Terraform-compliance](https://terraform-compliance.com) 之类的工具被采用来确保遵循策略。

Terraform-compliance 侧重于负面测试。 负面测试是确保系统可正常处理意外输入或不需要的行为的过程。 模糊处理就是一种负面测试。 通过模糊处理，接收输入的系统会经过测试，以确保它可安全地处理意外的输入。

幸运的是，Terraform 是任何创建、更新或销毁云基础结构实体的 API 的抽象层。 Terraform 还可确保本地配置和远程 API 响应保持同步。Terraform 主要针对云 API 使用，因此我们仍然需要一种方法来确保针对基础架构部署的代码遵循特定策略。 Terraform-compliance 是一种免费的开源工具，它为 Terraform 配置提供了此功能。

采用 VM 示例时，符合性策略可能如下所示：“如果要创建 Azure 资源，则该资源必须包含标记”。

Terraform-compliance 工具提供了一个测试框架，你可在该框架中创建与示例类似的策略。 然后，你可针对 Terraform 执行计划运行这些策略。

通过 Terraform-compliance 可应用 BDD（即行为驱动开发）原则。 BDD 是一个协作过程；在此过程中，所有利益干系人相互协作，共同定义系统应执行的操作。 这些利益干系人通常包括开发人员、测试人员，以及对正在开发的系统有既得利益或将受其影响的任何人。 BDD 的目标是鼓励团队创建具体的示例，来表达对系统行为的共识。

## <a name="looking-at-an-example"></a>查看示例

在本文前面的部分，你了解了为测试环境创建 VM 的符合性测试示例。 本部分介绍如何将该示例转换为 BDD 功能和方案。 首先使用 Cucumber（这是一种用于支持 BDD 的工具）来表示规则。

```Cucumber
when creating Azure resources, every new resource should have a tag
```

上一规则转换如下：

```Cucumber
If the resource supports tags
Then it must contain a tag
And its value must not be null
```

然后，Terraform HCL 代码遵循以下规则。

```hcl
resource "random_uuid" "uuid" {}

resource "azurerm_resource_group" "rg" {
  name     = "rg-hello-tf-${random_uuid.uuid.result}"
  location = var.location

  tags = {
    environment = "dev"
    application = "Azure Compliance"
  } 
}
```

第一个策略可编写作为 [BDD 功能方案](https://cucumber.io/docs/gherkin/reference/)，如下所示：

```Cucumber
Feature: Test tagging compliance  # /target/src/features/tagging.feature
    Scenario: Ensure all resources have tags
        If the resource supports tags
        Then it must contain a tag
        And its value must not be null
```

以下代码显示了针对特定标记的测试：

```Cucumber
Scenario Outline: Ensure that specific tags are defined
    If the resource supports tags
    Then it must contain a tag <tags>
    And its value must match the "<value>" regex

    Examples:
      | tags        | value              |
      | Creator     | .+                 |
      | Application | .+                 |
      | Role        | .+                 |
      | Environment | ^(prod\|uat\|dev)$ |
```

## <a name="running-the-sample"></a>运行示例

在本部分中，你将下载并测试示例。

1. [下载符合性测试示例](https://github.com/Azure/terraform/tree/master/samples/compliance-testing)。

1. 切换到 `src` 目录。

1. 运行 [terraform init](https://www.terraform.io/docs/commands/init.html)，将工作目录进行初始化。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

    ```bash
    terraform init
    ```
    
1. 运行 [terraform validate](https://www.terraform.io/docs/commands/validate.html) 来验证配置文件的语法。

    ```bash
    terraform validate
    ```
    
1. 运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 以创建执行计划。

    ```bash
    terraform plan -out tf.out
    ```
    
1. 运行 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 以应用执行计划。

    ```bash
    terraform apply -target=random_uuid.uuid
    ```
    
1. 运行 [docker pull](https://docs.docker.com/engine/reference/commandline/pull/) 以下载 terraform-compliance 映像。

    ```bash
    docker pull eerkunt/terraform-compliance
    ```
    
1. 运行 [docker run](https://docs.docker.com/engine/reference/commandline/run/) 以在 docker 容器中运行测试。 测试将失败。 要求标记存在的第一个规则会成功。 但是，第二个规则会失败，因为缺少 `Role` 和 `Creator` 标记。

    ```bash
    docker run --rm -v $PWD:/target -it eerkunt/terraform-compliance -f features -p tf.out
    ```
    
    ![失败测试的示例](media/best-practices-compliance-testing/best-practices-compliance-testing-tagging-fail.png)

1. 按如下所示修改 `main.tf` 以修复错误。

    ```terraform
      tags = {
        Environment = "dev"
        Application = "Azure Compliance"
        Creator     = "Azure Compliance"
        Role        = "Azure Compliance"
      } 
    
    ```
    
1. 再次运行 `terraform validate` 以验证语法。

    ```bash
    terraform validate
    ```
    
1. 再次运行 `terraform plan` 以创建新的执行计划。

    ```bash
    terraform plan -out tf.out
    ```
    
1. 再次运行 [docker run](https://docs.docker.com/engine/reference/commandline/run/) 以测试配置。 这一次测试会成功，因为实施了完整的规范。

    ```bash
    docker run --rm -v $PWD:/target -it eerkunt/terraform-compliance -f features -p tf.out
    ```

    ![成功测试的示例](media/best-practices-compliance-testing/best-practices-compliance-testing-tagging-succeed.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [在 Terraform 项目中创建和运行端到端测试](best-practices-end-to-end-testing.md)
