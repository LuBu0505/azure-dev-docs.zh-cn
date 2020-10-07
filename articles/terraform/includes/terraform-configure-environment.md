---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: daa57dfc18cae3250c42265ebe8cbf7722e4235b
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401579"
---
## <a name="configure-your-environment"></a>配置环境

根据环境安装并配置 Terraform：

- [使用 Azure Cloud Shell 和 Azure CLI 配置 Terraform](../get-started-cloud-shell.md)
- [使用 Azure PowerShell 配置 Terraform](../get-started-powershell.md)

配置文章还介绍如何执行以下任务：

- 创建基本 Terraform 配置文件。 此文件在 `provider` 块中包含 [Azure 提供程序 (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html)，并定义 [Azure 资源组](/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group)。
- 创建并应用 Terraform 执行计划以运行代码。
- 如果已使用完资源并希望将它们删除，可撤消执行计划。
