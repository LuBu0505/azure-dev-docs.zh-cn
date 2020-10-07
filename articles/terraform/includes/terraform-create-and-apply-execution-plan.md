---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 23a5bbc2e6a7e364b3c6eab38bfd5e98b97348a6
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401535"
---
## <a name="create-and-apply-a-terraform-execution-plan"></a>创建并应用 Terraform 执行计划

在本节中，你学习如何创建执行计划，并将其应用于云基础结构。

1. 要初始化 Terraform 部署，请运行 [terraform init](https://www.terraform.io/docs/commands/init.html)。 此命令将下载创建 Azure 资源组所需的 Azure 模块。

    ```cmd
    terraform init
    ```

1. 初始化后，运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 即可创建执行计划。

    ```cmd
    terraform plan -out <terraform_plan>.tfplan
    ```

    [!INCLUDE [terraform-plan-notes.md](terraform-plan-notes.md)]

1. 准备好将执行计划应用于云基础结构后，运行 [terraform apply](https://www.terraform.io/docs/commands/apply.html)。

    ```cmd
    terraform apply <terraform_plan>.tfplan
    ```
