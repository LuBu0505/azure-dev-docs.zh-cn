---
title: 教程 - 使用 Terraform 和 Azure 进行集成测试
description: 了解集成测试，学习如何使用 Azure DevOps 为 Terraform 项目配置持续集成。
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: d6c8f9c419070d734c3c848163c52e6255d5512a
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831993"
---
# <a name="tutorial-configure-integration-tests-for-terraform-projects-in-azure"></a>教程：在 Azure 中为 Terraform 项目配置集成测试

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍如何执行以下任务：

> [!div class="checklist"]
> * 了解 Terraform 项目的集成测试基础知识。
> * 使用 Azure DevOps 配置持续集成管道。
> * 对 Terraform 代码运行静态代码分析。
> * 运行 `terraform validate` 以验证本地计算机上的 Terraform 配置文件。
> * 运行 `terraform plan`，从远程服务的角度验证 Terraform 配置文件。
> * 使用 Azure 管道自动进行持续集成。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="prerequisites"></a>必备条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Azure DevOps 组织和项目**：如果没有，请[创建 Azure DevOps 组织](/azure/devops/organizations/projects/create-project?tabs=preview-page&view=azure-devops)。
- **“Terraform 构建和发布任务”扩展**：[将“Terraform 构建/发布任务”扩展](https://marketplace.visualstudio.com/items?itemName=charleszipp.azure-pipelines-tasks-terraform)安装到 Azure DevOps 组织中。
- **向 Azure DevOps 授予访问你的 Azure 订阅的权限**：创建一个名为 `terraform-basic-testing-azure-connection` 的 [Azure 服务连接](/azure/devops/pipelines/library/connect-to-azure?view=azure-devops)，以允许 Azure Pipelines 连接到你的 Azure 订阅
- **安装 Terraform**：根据你的环境，[下载并安装 Terraform](https://www.terraform.io/downloads.html)。
- **创建测试示例的分支**：创建 [GitHub 上的 Terraform 示例](https://github.com/Azure/terraform)的分支，并将其复制到开发/测试计算机。

## <a name="validate-a-local-terraform-configuration"></a>验证本地 Terraform 配置

[terraform validate](https://www.terraform.io/docs/commands/validate.html) 命令是从包含 Terraform 文件的目录中的命令行运行的。 此命令主要用于验证语法。

1. 打开所选的命令行环境。 许多代码编辑器（例如 Visual Studio Code）都提供了命令行接口。

1. 将目录更改为本地存储库的 `samples/integration-testing/src` 目录。 它包含一个简单的 Terraform 项目。

1. 使用 [Terraform init](https://www.terraform.io/docs/commands/init.html) 初始化 Terraform 部署。 此步骤将下载创建 Azure 资源组所需的 Azure 模块。

    ```bash
    terraform init
    ```

1. 使用 [terraform validate](https://www.terraform.io/docs/commands/validate.html) 验证测试 Terraform 文件。

    ```bash
    terraform validate
    ```

    你应会看到一条消息，其中指出配置有效。

1. 在代码编辑器中，打开 `main.tf` 文件。

1. 在第 5 行中，插入一个拼写错误，使语法无效。 例如，将 `var.location` 替换为 `var.loaction`

1. 保存文件。

1. 再次运行验证。

    ```bash
    terraform validate
    ```

    此时应会看到一条错误消息，其中指出出错行和错误说明。

如你所见，Terraform 在配置代码的语法中检测到一个问题。 此问题导致无法部署配置。

在将 Terraform 文件推送到版本控制系统之前，最好始终对这些文件运行 `terraform validate`。 此外，应将此级别的验证添加到持续集成管道中。 在本教程的后面，我们将探讨如何[配置 Azure 管道以自动验证](#automate-integration-tests-using-azure-pipeline)。

## <a name="validate-terraform-configuration-can-be-deployed-on-azure"></a>验证是否可在 Azure 上部署 Terraform 配置

在上一部分，你已了解如何验证 Terraform 配置。 此级别的测试特定于语法。 此测试没有考虑到 Azure 上可能已部署的内容。

Terraform 是一种声明性语言，这表示你将所需的内容声明为最终结果。 例如，假设资源组中有 10 个虚拟机。 你随后创建一个 Terraform 文件来定义 3 个虚拟机。 如果应用此计划，总数不会增加到 13。 相反，Terraform 会删除了其余的 7 个虚拟机，这样就只剩下 3 个。 运行 `terraform plan` 可确认应用执行计划的潜在结果，以避免意外。

若要生成 Terraform 执行计划，请运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html)。 此命令会连接到目标 Azure 订阅，以检查配置中已部署的部分。 然后，Terraform 将确定必要的更改以满足 Terraform 文件中所述的要求。 在此阶段，Terraform 不部署任何内容。 它会告诉你应用计划后将发生什么情况。

如果你正在按照本教程进行操作，而且已完成上一部分中的步骤，请运行 `terraform plan` 命令：

```bash
terraform plan
```

运行 `terraform plan` 后，Terraform 将显示应用执行计划可能产生的结果。 输出指示将添加、更改和销毁的 Azure 资源。

默认情况下，Terraform 会将状态存储在 Terraform 文件所在的本地目录中。 此模式非常适合单用户方案。 但是，当多名用户使用相同的 Azure 资源时，本地状态文件可能不同步。为解决此问题，Terraform 支持将状态文件写入远程数据存储（例如 Azure 存储）。 此情况下，在本地计算机上运行 `terraform plan` 并以远程计算机为目标可能会出现问题。 因此，[在持续集成管道的过程中自动执行此验证步骤](#automate-integration-tests-using-azure-pipeline)是有意义的。

## <a name="run-static-code-analysis"></a>运行静态代码分析

可直接在 Terraform 配置代码上完成静态代码分析，无需执行它。 此分析可用于检测安全问题和符合性不一致等问题。

下列工具为 Terraform 文件提供静态分析：

- [Checkov](https://github.com/bridgecrewio/checkov/)
- [Terrascan](https://github.com/cesar-rodriguez/terrascan)
- [tfsec](https://github.com/liamg/tfsec) 
- [Deepsource](https://deepsource.io/blog/release-terraform-static-analysis/) 

静态分析通常作为持续集成管道的一部分执行。 这些测试不要求创建执行计划或部署。 因此，它们比其他测试运行速度更快，通常在持续集成过程中最先运行。

## <a name="automate-integration-tests-using-azure-pipeline"></a>使用 Azure 管道自动执行集成测试

持续集成包括在引入更改时测试整个系统。 在本部分中，你将看到一个用于实现持续集成的 Azure 管道配置。

1. 使用所选的编辑器浏览到 [GitHub 上的 Terraform 示例项目](https://github.com/Azure/terraform)的本地克隆。

1. 打开 `samples/integration-testing/src/azure-pipeline.yaml` 文件。

1. 向下滚动到“步骤”部分，这里有一组用于运行各种安装和验证例程的标准步骤。

1. 查看显示“步骤 1：运行 Checkov 静态代码分析”的行。 在此步骤中，前面提到的 `Checkov` 项目将对示例 Terraform 配置运行静态代码分析。 

    ```yaml
    - bash: $(terraformWorkingDirectory)/checkov.sh $(terraformWorkingDirectory)
      displayName: Checkov Static Code Analysis
    ```
    
    注意：
    
    - 该脚本负责在装载到 Docker 容器内的 Terraform 工作区中运行 Checkov。 Microsoft 托管的代理已启用 Docker。 在 Docker 容器中运行工具更为简单，无需在 Azure 管道代理上安装 Checkov。
    - `$(terraformWorkingDirectory)` 变量是在 `azure-pipeline.yaml` 文件中定义的。

1. 查看显示“步骤 2：在 Azure Pipelines 代理上安装 Terraform”的行。 先前安装的[“Terraform 构建和发布任务”扩展](https://marketplace.visualstudio.com/items?itemName=charleszipp.azure-pipelines-tasks-terraform)中有一个命令，它用于在运行 Azure 管道的代理上安装 Terraform。 此任务是在此步骤中执行的。

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-installer.TerraformInstaller@0
      displayName: 'Install Terraform'
      inputs:
        terraformVersion: $(terraformVersion)
    ```
    
    注意：

    - 要安装的 Terraform 版本通过名为 `terraformVersion` 的 Azure 管道变量进行指定，并在 `azure-pipeline.yaml` 文件中定义。

1. 查看显示“步骤 3：运行 Terraform init 来初始化工作区”的行。 现已将 Terraform 安装在代理上，接下来可初始化 Terraform 目录。

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform init'
      inputs:
        command: init
        workingDirectory: $(terraformWorkingDirectory)
    ```
    
    注意：

    - `command` 输入指定了要运行的 Terraform 命令。
    - `workingDirectory` 输入指示了 Terraform 目录的路径。
    - `$(terraformWorkingDirectory)` 变量是在 `azure-pipeline.yaml` 文件中定义的。

1. 查看显示“步骤 4：运行 Terraform validate 来验证 HCL 语法”的行。 初始化项目目录后，运行 `terraform validate` 来验证服务器上的配置。

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform validate'
      inputs:
        command: validate
        workingDirectory: $(terraformWorkingDirectory)
    ```
    
1. 查看显示“步骤 5：运行 Terraform plan 来验证 HCL 语法”的行。 如前所述，已生成执行计划来在部署之前验证 Terraform 配置是否有效。

    ```yaml
    - task: charleszipp.azure-pipelines-tasks-terraform.azure-pipelines-tasks-terraform-cli.TerraformCLI@0
      displayName: 'Run terraform plan'
      inputs:
        command: plan
        workingDirectory: $(terraformWorkingDirectory)
        environmentServiceName: $(serviceConnection)
        commandOptions: -var location=$(azureLocation)
    ```
    
    注意：

    - `environmentServiceName` 输入是指在[先决条件](#prerequisites)中创建的 Azure 服务连接的名称。 Terraform 可通过该连接访问你的 Azure 订阅。
    - `commandOptions` 输入用于将参数传递给 Terraform 命令。 在本例中，将指定一个位置。 `$(azureLocation)` 变量之前已在 YAML 文件中进行定义。

### <a name="import-the-pipeline-into-azure-devops"></a>将管道导入到 Azure DevOps

1. 打开 Azure DevOps 项目并转到 Azure Pipelines 部分。
 
1. 选择“创建管道”按钮。

1. 对于“代码位置”选项，请选择“GitHub (YAML)” 。

    ![代码位置](media/best-practices-integration-testing/new-pipeline-where-github-yaml.png)

1. 此时，你可能需要授权 Azure DevOps 来访问你的组织。 有关本主题的详细信息，请参阅[构建 GitHub 存储库](/azure/devops/pipelines/repos/github?view=azure-devops&tabs=yaml)一文。

1. 在存储库列表中，选择你在 GitHub 组织中创建的存储库的分支。

1. 在“配置管道”步骤中，选择从现有的 YAML 管道开始。

    ![现有 YAML 管道](media/best-practices-integration-testing/new-pipeline-existing-yaml.png)

1. 显示“选择现有 YAML 管道”页面时，请指定分支 `master` 并输入 YAML 管道的路径：`samples/integration-testing/src/azure-pipeline.yaml`。

    ![选择现有 YAML 管道](media/best-practices-integration-testing/select-existing-yaml-pipeline.png)

1. 选择“继续”，从 GitHub 加载 Azure YAML 管道。

1. 显示“查看管道 YAML”页面时，请选择“运行”来首次创建并手动触发管道 。

    ![运行 Azure 管道](media/best-practices-integration-testing/run-pipeline.png)

### <a name="run-the-pipeline"></a>运行管道

可通过 Azure DevOps UI 手动运行管道。 不过，本文的重点是介绍自动持续集成。 通过将更改提交到分叉存储库的 `samples/integration-testing/src` 文件夹来测试该过程。 此更改将在你推送代码的分支上自动触发新管道。

![从 GitHub 运行的管道](media/best-practices-integration-testing/pipeline-running-from-github.png)

完成该步骤后，请查看 Azure DevOps 中的详细信息，确保一切正常运行。

![Azure DevOps 绿色管道](media/best-practices-integration-testing/azure-devops-green-pipeline.png)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [在 Terraform 项目中创建和运行符合性测试](best-practices-compliance-testing.md)