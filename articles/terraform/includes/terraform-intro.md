---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 05/31/2020
ms.author: tarcher
ms.openlocfilehash: 43e684a44c657ab44f3f8f13719e08f0e339f498
ms.sourcegitcommit: db56786f046a3bde1bd9b0169b4f62f0c1970899
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84329895"
---
通过配置 [Azure 上的 Terraform](https://www.terraform.io/docs/providers/azurerm/index.html) 并创建基本的 Azure 资源组，开始使用 [Terraform](https://www.terraform.io)。

使用 Terraform 可以定义、预览和部署云基础结构。 使用 Terraform 时，请使用 [HCL 语法](https://www.terraform.io/docs/configuration/syntax.html)来创建配置文件。 利用 HCL 语法，可指定 Azure 这样的云提供程序和构成云基础结构的元素。 创建配置文件后，请创建一个执行计划，利用该计划，可在部署基础结构更改之前先预览这些更改。 验证了更改后，请应用该执行计划以部署基础结构。
