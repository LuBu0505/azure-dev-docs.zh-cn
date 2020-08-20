---
title: 教程 - Terraform 测试概述
description: 了解可配置来验证 Terraform 项目的不同测试选项。
ms.topic: overview
ms.date: 07/31/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 29f128361030b64da38124f7f7d723619306f582
ms.sourcegitcommit: 16ce1d00586dfa9c351b889ca7f469145a02fad6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88241269"
---
# <a name="tutorial-terraform-testing-overview"></a>教程：Terraform 测试概述

[!INCLUDE [terraform-intro.md](includes/terraform-intro.md)]

Terraform 是一种基础结构即代码 (IaC) 工具。 这类工具指的是你将 Terraform 文件看作是项目的源代码。 在此过程中，包括版本控制和源代码管理。 此外，还应在该过程中包含测试。 本文概述了可对 Terraform 项目运行的不同类型的测试。

[!INCLUDE [hashicorp-support.md](includes/hashicorp-support.md)]

## <a name="integration-testing"></a>集成测试

集成测试可验证新引入的代码更改是否不会破坏现有代码。 在 DevOps 中，持续集成 (CI) 是指每当代码库发生更改（例如有用户要将 PR 合并到 Git 存储库中）时都生成整个系统的过程。 下表包含集成测试的常见示例：

- 静态代码分析工具，例如 lint 和格式。
- 运行 [terraform validate](https://www.terraform.io/docs/commands/validate.html) 来验证配置文件的语法。
- 运行 [terraform plan](https://www.terraform.io/docs/commands/validate.html) 以确保配置将按预期方式工作。

> [!div class="nextstepaction"]
> [详细了解集成测试](best-practices-integration-testing.md)

## <a name="unit-testing"></a>单元测试

单元测试可确保程序的特定部分或功能正常运行。 单元测试由功能的开发人员编写。 这种类型的测试有时也被称为测试驱动开发 (TDD)，它涉及持续的短开发周期。 在 Terraform 项目的上下文中，单元测试可采用 `terraform plan` 的形式来确保生成的计划中可用的实际值等于预期值。 

随着 Terraform 模块开始变得愈发复杂，单元测试将格外有用：

- 生成动态块
- 使用循环
- 计算局部变量

与集成测试一样，持续集成过程中经常包含单元测试。

## <a name="compliance-testing"></a>合规性测试

符合性测试用于确保配置符合你为项目定义的策略。 例如，可定义 Azure 资源的地缘政治命名约定。 或者，你可能希望通过部分已定义的映像创建虚拟机。 符合性测试将用于强制实施这些规则。

符合性测试通常也被定义为持续集成过程的一部分。

> [!div class="nextstepaction"]
> [详细了解符合性测试](best-practices-compliance-testing.md)

## <a name="end-to-end-e2e-testing"></a>端到端 (E2E) 测试

E2E 测试会在部署到生产环境之前验证程序是否正常工作。 示例场景可能是一个将两个虚拟机部署到虚拟网络中的 Terraform 模块。 你可能想要阻止这两个虚拟机相互执行 ping 操作。 在此示例中，可定义一个测试，在部署之前验证预期结果。

E2E 测试通常分三步执行。 首先，将配置应用于测试环境。 然后运行代码来验证结果。 最后，重新初始化或关闭测试环境（例如解除虚拟机的分配）。

> [!div class="nextstepaction"]
> [详细了解端到端测试](best-practices-end-to-end-testing.md)