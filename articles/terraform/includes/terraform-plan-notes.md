---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9f3d916bc37cc9d5f86c1f6b2738f419414e5816
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401532"
---
  注意：

  - `terraform plan` 命令将创建一个执行计划，但不会执行它。 它会确定创建配置文件中指定的配置需要执行哪些操作。 此模式允许你在对实际资源进行任何更改之前验证执行计划是否符合预期。
  - 使用可选 `-out` 参数可以为计划指定输出文件。 使用 `-out` 参数可以确保所查看的计划与所应用的计划完全一致。
  - 若要详细了解如何使执行计划和安全性持久化，请参阅[安全警告一节](https://www.terraform.io/docs/commands/plan.html#security-warning)。