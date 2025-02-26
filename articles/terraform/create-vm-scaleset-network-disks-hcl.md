---
title: 使用 Terraform 创建 Azure 虚拟机规模集
description: 了解如何使用 Terraform 配置 Azure 虚拟机规模集并对其进行版本控制。
ms.topic: how-to
ms.date: 11/07/2019
ms.custom: devx-track-terraform
ms.openlocfilehash: d157a9aa6b59281dc34f172fb58eedd87f06ffae
ms.sourcegitcommit: 5541f993c01ce356e1b0eaa8f95aea9051c3c21e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93278442"
---
# <a name="create-an-azure-virtual-machine-scale-set-using-terraform"></a>使用 Terraform 创建 Azure 虚拟机规模集

[Azure 虚拟机规模集](/azure/virtual-machine-scale-sets)允许配置相同的 VM。 VM 实例的数目可以根据需求或计划进行调整。 有关详细信息，请参阅[自动缩放 Azure 门户中的虚拟机规模集](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-portal)

在本文中，学习如何：

> [!div class="checklist"]
> * 设置 Terraform 部署
> * 使用变量和输出部署 Terraform
> * 创建和部署网络基础结构
> * 创建和部署虚拟机规模集并将其附加到网络
> * 创建和部署 jumpbox 以通过 SSH 连接到 VM

> [!NOTE]
> [GitHub 上的 Awesome Terraform 存储库](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss)中提供了本文所用的最新版本的 Terraform 配置文件。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

- **安装 Terraform** ：遵循 [安装 Terraform 并配置对 Azure 的访问权限](get-started-cloud-shell.md)一文中的指导

- **创建 SSH 密钥对** ：有关详细信息，请参阅 [如何创建和使用适用于 Azure 中 Linux VM 的 SSH 公钥和私钥对](/azure/virtual-machines/linux/mac-create-ssh-keys)。

## <a name="create-the-directory-structure"></a>创建目录结构

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 打开 [Azure Cloud Shell](/azure/cloud-shell/overview)。 如果事先未选择环境，请选择“Bash”作为环境。

    ![Cloud Shell 提示符](./media/create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. 切换到 `clouddrive` 目录。

    ```bash
    cd clouddrive
    ```

1. 创建名为 `vmss` 的目录。

    ```bash
    mkdir vmss
    ```

1. 将目录切换到新目录：

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>创建变量定义文件
在本部分，我们将定义可用于自定义 Terraform 所创建资源的变量。

在 Azure Cloud Shell 中执行以下步骤：

1. 创建名为 `variables.tf` 的文件。

    ```bash
    code variables.tf
    ```

1. 在编辑器中粘贴以下代码：

   ```hcl
   variable "location" {
    description = "The location where resources will be created"
   }

   variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = map(string)

    default = {
      environment = "codelab"
    }
   }

   variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
   }
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

## <a name="create-the-output-definitions-file"></a>创建输出定义文件
在本部分，我们将创建用于描述部署后的输出的文件。

在 Azure Cloud Shell 中执行以下步骤：

1. 创建名为 `output.tf` 的文件。

    ```bash
    code output.tf
    ```

1. 在编辑器中粘贴以下代码，以公开虚拟机的完全限定域名 (FQDN)。
   解码的字符：

   ```hcl
    output "vmss_public_ip" {
        value = azurerm_public_ip.vmss.fqdn
    }
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

## <a name="define-the-network-infrastructure-in-a-template"></a>在模板中定义网络基础结构
在本部分，我们将在新的 Azure 资源组中创建以下网络基础结构：

  - 1 个虚拟网络 (VNET)，其地址空间为 10.0.0.0/16
  - 1 个子网，地址空间为 10.0.2.0/24
  - 2 个公共 IP 地址。 一个供虚拟机规模集负载均衡器使用，另一个用于连接到 SSH jumpbox。

在 Azure Cloud Shell 中执行以下步骤：

1. 创建名为 `vmss.tf` 的文件，用于描述虚拟机规模集基础结构。

    ```bash
    code vmss.tf
    ```

1. 在文件的末尾粘贴以下代码，以公开虚拟机的完全限定域名 (FQDN)。

   ```hcl
   resource "azurerm_resource_group" "vmss" {
    name     = var.resource_group_name
    location = var.location
    tags     = var.tags
   }

   resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
   }

   resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = var.location
    resource_group_name = azurerm_resource_group.vmss.name
    tags                = var.tags
   }

   resource "azurerm_subnet" "vmss" {
    name                 = "vmss-subnet"
    resource_group_name  = azurerm_resource_group.vmss.name
    virtual_network_name = azurerm_virtual_network.vmss.name
    address_prefix       = "10.0.2.0/24"
   }

   resource "azurerm_public_ip" "vmss" {
    name                         = "vmss-public-ip"
    location                     = var.location
    resource_group_name          = azurerm_resource_group.vmss.name
    allocation_method = "Static"
    domain_name_label            = random_string.fqdn.result
    tags                         = var.tags
   }
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

## <a name="provision-the-network-infrastructure"></a>预配网络基础结构
在创建了配置文件 (.tf) 的目录中使用 Azure Cloud Shell 执行以下步骤：

1. 初始化 Terraform。

   ```bash
   terraform init
   ```

1. 运行以下命令，在 Azure 中部署定义的基础结构。

   ```bash
   terraform apply
   ```

   由于 `variables.tf` 中定义了 `location` 变量，但从未设置该变量的值，因此，Terraform 会提示输入 `location` 值。 可以输入任何有效位置（例如“West US”），然后按 Enter。 （请将包含空格的任何值括在括号中。）

1. Terraform 会列显 `output.tf` 文件中定义的输出。 如以下屏幕截图所示，FQDN 采用以下格式：`<ID>.<location>.cloudapp.azure.com`。 ID 是一个计算值，location 是运行 Terraform 时你提供的值。

   ![公共 IP 地址的虚拟机规模集完全限定域名](./media/create-vm-scaleset-network-disks-hcl/fqdn.png)

1. 在 Azure 门户上的主菜单中，选择“资源组”。

1. 在“资源组”选项卡上，选择“myResourceGroup”查看 Terraform 创建的资源。
   ![虚拟机规模集网络资源](./media/create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>添加虚拟机规模集

本部分介绍如何将以下资源添加到模板：

- Azure 负载均衡器和规则，用于为应用程序提供服务并将其附加到本文前面所配置的公共 IP 地址
- Azure 后端地址池，并将其分配到负载均衡器
- 运行状况探测端口，供应用程序使用并在负载均衡器上配置
- 虚拟机规模集，位于本文前面部署的 VNET 上运行的负载均衡器后面
- 使用 [cloud-init](https://cloudinit.readthedocs.io/en/latest/) 在虚拟机规模集节点上添加 [Nginx](https://nginx.org/)。

在 Cloud Shell 中执行以下步骤：

1. 打开 `vmss.tf` 配置文件。

   ```bash
   code vmss.tf
   ```

1. 转到文件末尾，并按 A 键进入追加模式。

1. 在文件的末尾粘贴以下代码：

   ```hcl
   resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = var.location
    resource_group_name = azurerm_resource_group.vmss.name

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = azurerm_public_ip.vmss.id
    }

    tags = var.tags
   }

   resource "azurerm_lb_backend_address_pool" "bpepool" {
    resource_group_name = azurerm_resource_group.vmss.name
    loadbalancer_id     = azurerm_lb.vmss.id
    name                = "BackEndAddressPool"
   }

   resource "azurerm_lb_probe" "vmss" {
    resource_group_name = azurerm_resource_group.vmss.name
    loadbalancer_id     = azurerm_lb.vmss.id
    name                = "ssh-running-probe"
    port                = var.application_port
   }

   resource "azurerm_lb_rule" "lbnatrule" {
      resource_group_name            = azurerm_resource_group.vmss.name
      loadbalancer_id                = azurerm_lb.vmss.id
      name                           = "http"
      protocol                       = "Tcp"
      frontend_port                  = var.application_port
      backend_port                   = var.application_port
      backend_address_pool_id        = azurerm_lb_backend_address_pool.bpepool.id
      frontend_ip_configuration_name = "PublicIPAddress"
      probe_id                       = azurerm_lb_probe.vmss.id
   }

   resource "azurerm_virtual_machine_scale_set" "vmss" {
    name                = "vmscaleset"
    location            = var.location
    resource_group_name = azurerm_resource_group.vmss.name
    upgrade_policy_mode = "Manual"

    sku {
      name     = "Standard_DS1_v2"
      tier     = "Standard"
      capacity = 2
    }

    storage_profile_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_profile_os_disk {
      name              = ""
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    storage_profile_data_disk {
      lun          = 0
      caching        = "ReadWrite"
      create_option  = "Empty"
      disk_size_gb   = 10
    }

    os_profile {
      computer_name_prefix = "vmlab"
      admin_username       = var.admin_user
      admin_password       = var.admin_password
      custom_data          = file("web.conf")
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    network_profile {
      name    = "terraformnetworkprofile"
      primary = true

      ip_configuration {
        name                                   = "IPConfiguration"
        subnet_id                              = azurerm_subnet.vmss.id
        load_balancer_backend_address_pool_ids = [azurerm_lb_backend_address_pool.bpepool.id]
        primary = true
      }
    }

    tags = var.tags
   }
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 创建名为 `web.conf` 的文件，充当规模集中包含的虚拟机的 cloud-init 配置。

    ```bash
    code web.conf
    ```

1. 在编辑器中粘贴以下代码：

   ```hcl
   #cloud-config
   packages:
    - nginx
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 打开 `variables.tf` 配置文件。

    ```bash
    code variables.tf
    ```

1. 转到文件的末尾。

1. 将以下代码粘贴到文件末尾，以自定义部署：

    ```hcl
    variable "application_port" {
       description = "The port that you want to expose to the external load balancer"
       default     = 80
    }

    variable "admin_user" {
       description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
       default     = "azureuser"
    }

    variable "admin_password" {
       description = "Default password for admin account"
    }
    ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 创建 Terraform 计划，以可视化虚拟机规模集部署。 （需要指定密码和资源的位置。）

    ```bash
    terraform plan
    ```

    命令的输出应如以下屏幕截图所示：

    ![创建虚拟机规模集后的输出](./media/create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. 在 Azure 中部署新资源。

    ```bash
    terraform apply
    ```

1. 打开浏览器并连接到该命令返回的 FQDN。

    ![浏览到 FQDN 后的结果](./media/create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>添加 SSH jumpbox
SSH *jumpbox* 是为了访问网络中的其他服务器而“跳转”的单个服务器。 在本步骤中，配置以下资源：

- 网络接口（或 jumpbox），已连接到虚拟机规模集所在的同一子网。

- 虚拟机，它已连接到此网络接口。 此“jumpbox”可远程访问。 连接后，可通过 SSH 访问规模集中的任何虚拟机。

1. 打开 `vmss.tf` 配置文件。

   ```bash
   code vmss.tf
   ```

1. 转到文件的末尾。

1. 在文件的末尾粘贴以下代码：

   ```hcl
   resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = var.location
    resource_group_name          = azurerm_resource_group.vmss.name
    allocation_method = "Static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = var.tags
   }

   resource "azurerm_network_interface" "jumpbox" {
    name                = "jumpbox-nic"
    location            = var.location
    resource_group_name = azurerm_resource_group.vmss.name

    ip_configuration {
      name                          = "IPConfiguration"
      subnet_id                     = azurerm_subnet.vmss.id
      private_ip_address_allocation = "dynamic"
      public_ip_address_id          = azurerm_public_ip.jumpbox.id
    }

    tags = var.tags
   }

   resource "azurerm_virtual_machine" "jumpbox" {
    name                  = "jumpbox"
    location              = var.location
    resource_group_name   = azurerm_resource_group.vmss.name
    network_interface_ids = [azurerm_network_interface.jumpbox.id]
    vm_size               = "Standard_DS1_v2"

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "jumpbox-osdisk"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    os_profile {
      computer_name  = "jumpbox"
      admin_username = var.admin_user
      admin_password = var.admin_password
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = var.tags
   }
   ```
1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 打开 `output.tf` 配置文件。

   ```bash
   code output.tf
   ```

1. 转到文件的末尾。

1. 将以下代码粘贴到文件的末尾，以在部署完成后显示 jumpbox 的主机名：

   ```hcl
   output "jumpbox_public_ip" {
      value = azurerm_public_ip.jumpbox.fqdn
   }
   ```

1. 保存文件 ( **&lt;Ctrl>S** ) 并退出编辑器 ( **&lt;Ctrl>Q** )。

1. 部署 jumpbox。

   ```bash
   terraform apply
   ```

**注释** ：

- 在部署的 jumpbox 和虚拟机规模集上已禁用密码登录功能。 请使用 SSH 登录以访问虚拟机。

## <a name="environment-cleanup"></a>环境清理

若要删除本文中创建的 Terraform 资源，请在 Cloud Shell 中输入以下命令：

```bash
terraform destroy
```

销毁过程可能需要几分钟才能完成。

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [详细了解如何在 Azure 中使用 Terraform](/azure/terraform)
