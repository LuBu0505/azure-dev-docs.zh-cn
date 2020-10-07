---
title: 使用 Terraform 配置 Azure VM 群集
description: 了解如何使用 Terraform 模块在 Azure 中创建 Windows 虚拟机群集。
keywords: azure devops terraform vm virtual machine cluster module registry
ms.topic: how-to
ms.date: 09/27/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 73f375090a2178b38b0fc7e0afd4eb8c6b514672
ms.sourcegitcommit: e20f6c150bfb0f76cd99c269fcef1dc5ee1ab647
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91401578"
---
# <a name="configure-an-azure-vm-cluster-using-terraform"></a>使用 Terraform 配置 Azure VM 群集

本文展示在 Azure 上创建 VM 群集的 Terraform 示例代码。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

[!INCLUDE [terraform-configure-environment.md](includes/terraform-configure-environment.md)]

## <a name="configure-an-azure-vm-cluster"></a>配置 Azure VM 群集

```hcl
module "windowsservers" {
  source              = "Azure/compute/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  is_windows_image    = true
  vm_hostname         = "mywinvm"                         // Line can be removed if only one VM module per resource group
  admin_password      = "ComplxP@ssw0rd!"                 // See note following code about storing passwords in config files
  vm_os_simple        = "WindowsServer"
  public_ip_dns       = ["winsimplevmips"]                // Change to a unique name per data center region
  vnet_subnet_id      = module.network.vnet_subnets[0]
    
  depends_on = [azurerm_resource_group.rg]
}

module "network" {
  source              = "Azure/network/azurerm"
  resource_group_name = azurerm_resource_group.rg.name
  subnet_prefixes     = ["10.0.1.0/24"]
  subnet_names        = ["subnet1"]

  depends_on = [azurerm_resource_group.rg]
}

output "windows_vm_public_name" {
  value = module.windowsservers.public_ip_dns_name
}

output "vm_public_ip" {
  value = module.windowsservers.public_ip_address
}

output "vm_private_ips" {
  value = module.windowsservers.network_interface_private_ip
}
```

**注释**：

- 在上面的代码示例中，为了简单起见，为变量 `admin_password` 分配了一个文本值。 存储敏感数据（例如密码）有多种方法。 关于希望使用哪种数据保护方法的决策实际上是个人对于自己的特定环境和公开这些数据的舒适度的抉择。 例如，一个风险是，在源代码管理中像这样存储文件可能会导致他人看见密码。 HashiCorp 已记录了[声明输入变量](https://www.terraform.io/docs/configuration/variables.html)的多种方法以及[管理敏感数据（例如密码）](https://www.terraform.io/docs/state/sensitive-data.html)的技巧，请参阅相关文档，了解有关此主题的更多信息。

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [详细了解如何在 Azure 中使用 Terraform](/azure/terraform)
