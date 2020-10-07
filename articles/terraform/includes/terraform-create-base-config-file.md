---
title: include 文件
description: include 文件
author: tomarchermsft
ms.service: terraform
ms.topic: include
ms.date: 09/27/2020
ms.author: tarcher
ms.openlocfilehash: 9fcde450a19515c38eb6653d75e644ca23d08964
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401534"
---
## <a name="create-a-base-terraform-configuration-file"></a>创建基本 Terraform 配置文件

指定提供程序即可开始使用 Terraform 配置文件。 使用 Azure 时，你将在 `provider` 块中指定 [Azure 提供程序 (azurerm)](https://www.terraform.io/docs/providers/azurerm/index.html)。

```terraform
provider "azurerm" {
  version = "~>2.0"
  features {}
}

resource "azurerm_resource_group" "rg" {
  name = "<your_resource_group_name>"
  location = "<your_resource_group_location>"
}

# Your Terraform code goes here...

```

**注释**：

- 虽然 `version` 属性为可选属性，但 HashiCorp 建议固定使用给定版本的提供程序。 
- 如果使用的是 Azure 提供程序 1.x，则不允许使用 `features` 块。
- 如果使用的是 Azure 提供程序 2.x，需要使用 `features` 块。
- [azurerm_resource_group](https://www.terraform.io/docs/providers/azurerm/r/resource_group.html) 的 [资源声明](https://www.terraform.io/docs/configuration/resources.html)有两个参数：`name` 和 `location`。 将占位符设置为你的环境的相应值。
- 引用资源组时，操作说明和教程文章均使用资源组的 [本地命名值](https://www.terraform.io/docs/configuration/expressions.html#references-to-named-values) `rg`。 这与资源组名称无关，仅引用代码中的变量名称。 如果在资源组定义中更改此值，也需要在引用它的代码中更改它。
