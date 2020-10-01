---
title: 教程 - 在 Terraform 项目上设置端到端 Terratest 测试
description: 详细了解在 Terraform 项目上使用 Terratest 进行端到端测试的情况。
ms.topic: tutorial
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: b760908bf1950751b93ba1787f444ca37ee8bf83
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401707"
---
# <a name="tutorial-setup-end-to-end-terratest-testing-on-terraform-projects"></a>教程：在 Terraform 项目上设置端到端 Terratest 测试

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

本文介绍如何执行以下任务：

> [!div class="checklist"]
> * 了解使用 [Terratest](https://github.com/gruntwork-io/terratest) 进行端到端测试的基础知识
> * 了解如何使用 Golang 编写端到端测试
> * 了解如何使用 Azure DevOps 在将代码提交到存储库时自动触发端到端测试

## <a name="prerequisites"></a>必备条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **安装 Terraform**：根据你的环境，[下载并安装 Terraform](https://www.terraform.io/downloads.html)。
- 设置测试示例的分支：若要快速入门，建议你在自己的 GitHub 组织中创建[此存储库](https://github.com/Azure/terraform)的分支。
- **Go 编程语言**：Terraform 测试用例以 [Go](https://golang.org/dl/) 编写。 本文中的示例使用 [Go 模块](https://blog.golang.org/using-go-modules)。 本文建议使用 Go 1.13（或更高版本）。

## <a name="what-is-end-to-end-testing"></a>什么是端到端测试

端对端测试会验证系统是否作为整体进行工作。 这种类型的测试与测试特定的模块不同。 在 Terraform 项目中，可通过端到端测试验证已部署的内容。 这种类型的测试与其他很多测试预部署方案的类型不同。 对于测试包含多个模块且作用于多个资源的复杂系统来说，端到端测试至关重要。 在这种情况下，只能使用端到端测试来确定各个模块是否正确交互。

本文重点介绍如何使用 [Terratest](https://github.com/gruntwork-io/terratest) 实现端到端测试。 Terratest 提供执行下列任务所需的全部管道：

- 部署 Terraform 配置
- 使你能够使用 Go 语言编写测试来验证已部署的内容
- 安排分阶段执行测试
- 拆解已部署的基础架构

## <a name="tutorial-scenario"></a>教程方案

在本教程中，我们将使用 [Azure/terraform 示例存储库](https://github.com/Azure/terraform/blob/master/samples/end-to-end-testing/README.md)中提供的示例。

此示例定义了一个 Terraform 配置，它将两个 Linux 虚拟机部署到同一虚拟网络中。 第一个虚拟机 (VM) 名为 `vm-linux-1`，具有公共 IP 地址。 仅打开端口 22 以允许 SSH 连接。 第二个 VM 名为 `vm-linux-2`，没有定义的公共 IP 地址。

我们的测试应验证以下方案：

- 是否已正确部署基础结构
- 使用端口 22 是否可打开到 `vm-linux-1` 的 SSH 会话
- 使用 `vm-linux-1` 上的 SSH 会话是否可对 `vm-linux-2` 执行 ping 操作

![端到端测试方案示例](media/best-practices-end-to-end-testing/scenario.png)

如果[已下载示例](#prerequisites)，则可在 `src/main.tf` 文件中找到此方案的 Terraform 配置。 该文件包含部署上图中所示的 Azure 基础结构所需的全部内容。

如果不熟悉如何创建虚拟机，请参阅[使用 Terraform 在 Azure 中创建带有基础结构的 Linux VM](create-linux-virtual-machine-with-infrastructure.md)。

> [!CAUTION]
> 本文中介绍的示例方案仅用于说明目的。 为了重点介绍端到端测试，我们特意简化了操作。 建议不要使用通过公共 IP 地址公开 SSH 端口的生产虚拟机。

## <a name="end-to-end-test"></a>端到端测试

端到端测试使用 Go 语言编写，并使用 Terratest 框架。 如果[已下载示例](#prerequisites)，则该实例在 `src/test/end2end_test.go` 文件中进行定义。

以下源代码显示了使用 Terratest 的 Golang 测试的标准结构：

```Go
package test

import (
    "testing"

    "github.com/gruntwork-io/terratest/modules/terraform"
    test_structure "github.com/gruntwork-io/terratest/modules/test-structure"
)

func TestEndToEndDeploymentScenario(t *testing.T) {
    t.Parallel()

    fixtureFolder := "../"

    // User Terratest to deploy the infrastructure
    test_structure.RunTestStage(t, "setup", func() {
        terraformOptions := &terraform.Options{
            // Indicate the directory that contains the Terraform configuration to deploy
            TerraformDir: fixtureFolder,
        }

        // Save options for later test stages
        test_structure.SaveTerraformOptions(t, fixtureFolder, terraformOptions)

        // Triggers the terraform init and terraform apply command
        terraform.InitAndApply(t, terraformOptions)
    })

    test_structure.RunTestStage(t, "validate", func() {
        // run validation checks here
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
            publicIpAddress := terraform.Output(t, terraformOptions, "public_ip_address")
    })

    // When the test is completed, teardown the infrastructure by calling terraform destroy
    test_structure.RunTestStage(t, "teardown", func() {
        terraformOptions := test_structure.LoadTerraformOptions(t, fixtureFolder)
        terraform.Destroy(t, terraformOptions)
    })
}
```

如前面的代码片段中所示，该测试由 3 个阶段组成：

- **安装**：运行 Terraform 以部署配置
- **验证**`：执行验证检查和断言
- **拆解**：运行测试后清理基础结构

下表显示了 Terratest 框架提供的一些关键功能：

- **terraform.InitAndApply**：使能够从 Go 代码运行 `terraform init` 和 `terraform apply`
- **terraform.Output**检索部署输出变量的值。
- **terraform.Destroy**：从 Go 代码运行 `terraform destroy` 命令。
- **test_structure.LoadTerraformOptions**：从状态加载 Terraform 选项，例如配置和变量
- **test_structure.SaveTerraformOptions**：将 Terraform 选项（如配置和变量）保存到状态

## <a name="run-the-end-to-end-test"></a>运行端到端测试

在本部分，你将针对示例配置和部署运行测试。 

1. 打开 bash/终端窗口。

1. 登录到 Azure 帐户。 要了解如何登录到 Azure 订阅，请参阅 [Azure 身份验证部分](get-started-cloud-shell.md#authenticate-to-azure)。

1. 若要运行此示例测试，需要在主目录中使用 SSH 私钥/公钥对名称 `id_rsa` 和 `id_rsa.pub`。 将 `USER` 替换为主目录的名称。

```bash
export TEST_SSH_KEY_PATH="/home/USER/.ssh/id_rsa"
```

1. 切换到 `test` 目录。

1. 运行测试。

```go
go test -v ./ -timeout 10m
```

该测试将显示与以下输出类似的结果：

```output
--- PASS: TestEndToEndDeploymentScenario (390.99s)
PASS
ok      test    391.052s
```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Terraform 测试概述](best-practices-testing-overview.md)