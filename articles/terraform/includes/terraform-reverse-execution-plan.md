---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 718ac8c480c5d366e985f9488d81ab626a82b586
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401533"
---
## <a name="reverse-a-terraform-execution-plan"></a>撤消 Terraform 执行计划

1. 要撤消执行计划，请运行 [terraform plan](https://www.terraform.io/docs/commands/plan.html) 并指定 `destroy` 标志，如下所示：

    ```cmd
    terraform plan -destroy -out <terraform_plan>.destroy.tfplan
    ```

    [!INCLUDE [terraform-plan-notes.md](terraform-plan-notes.md)]

1. 运行 [terraform apply](https://www.terraform.io/docs/commands/apply.html) 以应用执行计划。

    ```cmd
    terraform apply <terraform_plan>.destroy.tfplan
    ```
