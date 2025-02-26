---
title: 使用 Terraform 在 Azure 中创建带有基础结构的 Linux VM
description: 了解如何使用 Terraform 在 Azure 中创建和管理完整的 Linux 虚拟机环境。
keywords: azure devops terraform linux vm 虚拟机
ms.topic: how-to
ms.date: 06/14/2020
ms.custom: devx-track-terraform
ms.openlocfilehash: 2beb9a98644d19c556342ee98e72c0b8b04ef355
ms.sourcegitcommit: bf911950c432bdce257f9968b294a1c33e74c5f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2020
ms.locfileid: "91717460"
---
# <a name="create-a-linux-vm-with-infrastructure-in-azure-using-terraform"></a>使用 Terraform 在 Azure 中创建带有基础结构的 Linux VM

使用 Terraform 可以在 Azure 中定义和创建完整的基础结构部署。 以用户可读格式生成 Terraform 模板，用于以一致且可重现的方式创建和配置 Azure 资源。 本文介绍了如何使用 Terraform 创建完整的 Linux 环境和支持资源。 另外，你还可以了解如何[安装和配置 Terraform](get-started-cloud-shell.md)。

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]

## <a name="create-azure-connection-and-resource-group"></a>创建 Azure 连接和资源组

我们来详细地了解 Terraform 模板的每个部分。 还可以看到完整版本的 [Terraform 模板](#complete-terraform-script)，可以复制并粘贴这些模板。

`provider` 部分告知 Terraform 使用 Azure 提供程序。 若要获取 `subscription_id`、`client_id`、`client_secret` 和 `tenant_id`的值，请参阅[安装并配置 Terraform](get-started-cloud-shell.md)。

> [!TIP]
> 如果要为值创建环境变量，或要使用 [Azure Cloud Shell Bash 体验](/azure/cloud-shell/overview)，则无需在此节中包括变量声明。

```hcl
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x.
    # If you're using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}
```

以下部分在 `eastus` 位置创建名为 `myResourceGroup` 的资源组：

```hcl
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags = {
        environment = "Terraform Demo"
    }
}
```

在其他部分，我们引用具有 `azurerm_resource_group.myterraformgroup.name` 的资源组。

## <a name="create-virtual-network"></a>创建虚拟网络

以下部分在 `10.0.0.0/16` 地址空间中创建名为 `myVnet` 的虚拟网络：

```hcl
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    tags = {
        environment = "Terraform Demo"
    }
}
```

以下部分在 `myVnet` 虚拟网络中创建名为 `mySubnet` 的子网：

```hcl
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = azurerm_resource_group.myterraformgroup.name
    virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
    address_prefixes       = ["10.0.2.0/24"]
}
```

## <a name="create-public-ip-address"></a>创建公共 IP 地址

若要通过 Internet 访问资源，请创建公共 IP 地址并将其分配到 VM。 以下部分创建名为 `myPublicIP` 的公共 IP 地址：

```hcl
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = azurerm_resource_group.myterraformgroup.name
    allocation_method            = "Dynamic"

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="create-network-security-group"></a>创建网络安全组

网络安全组控制传入和传出 VM 的网络流量。 以下部分创建名为 `myNetworkSecurityGroup` 的网络安全组并定义允许 TCP 端口 22 上的 SSH 流量的规则：

```hcl
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="create-virtual-network-interface-card"></a>创建虚拟网络接口卡

虚拟网络接口卡 (NIC) 将 VM 连接到规定的虚拟网络、公共 IP 地址和网络安全组。 Terraform 模板的以下部分创建名为 `myNIC` 的虚拟 NIC，并连接到已创建的虚拟网络资源：

```hcl
resource "azurerm_network_interface" "myterraformnic" {
    name                        = "myNIC"
    location                    = "eastus"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = azurerm_subnet.myterraformsubnet.id
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = azurerm_public_ip.myterraformpublicip.id
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Connect the security group to the network interface
resource "azurerm_network_interface_security_group_association" "example" {
    network_interface_id      = azurerm_network_interface.myterraformnic.id
    network_security_group_id = azurerm_network_security_group.myterraformnsg.id
}
```

## <a name="create-storage-account-for-diagnostics"></a>创建存储帐户以进行诊断

若要为 VM 存储启动诊断，需要一个存储帐户。 这些启动诊断可帮助你排查问题和监视 VM 状态。 你创建的存储帐户仅用于存储启动诊断数据。 由于每个存储帐户必须具有唯一名称，以下部分会生成一些随机文本：

```hcl
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = azurerm_resource_group.myterraformgroup.name
    }

    byte_length = 8
}
```

现在可以创建存储帐户了。 以下部分创建一个存储帐户，其名称基于上一步中生成的随机文本：

```hcl
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name
    location                    = "eastus"
    account_replication_type    = "LRS"
    account_tier                = "Standard"

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="create-virtual-machine"></a>创建虚拟机

最后一步是创建 VM 并使用所有已创建的资源。 以下部分创建名为 `myVM` 的 VM 并附加名为 `myNIC` 的虚拟 NIC。 将使用最新的 `Ubuntu 18.04-LTS` 映像，并且在禁用密码身份验证的情况下创建名为 `azureuser` 的用户。

 `ssh_keys` 部分提供了 SSH 密钥数据。 在 `key_data` 字段中提供公共 SSH 密钥。

```hcl
resource "tls_private_key" "example_ssh" {
  algorithm = "RSA"
  rsa_bits = 4096
}

output "tls_private_key" { value = tls_private_key.example_ssh.private_key_pem }

resource "azurerm_linux_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = azurerm_resource_group.myterraformgroup.name
    network_interface_ids = [azurerm_network_interface.myterraformnic.id]
    size                  = "Standard_DS1_v2"

    os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        storage_account_type = "Premium_LRS"
    }

    source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "18.04-LTS"
        version   = "latest"
    }

    computer_name  = "myvm"
    admin_username = "azureuser"
    disable_password_authentication = true

    admin_ssh_key {
        username       = "azureuser"
        public_key     = tls_private_key.example_ssh.public_key_openssh
    }

    boot_diagnostics {
        storage_account_uri = azurerm_storage_account.mystorageaccount.primary_blob_endpoint
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>完成 Terraform 脚本

若要将所有这些部分组合在一起，并在操作中看到 Terraform，请创建名为 `terraform_azure.tf` 的文件并粘贴以下内容：

```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you're using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}

# Create a resource group if it doesn't exist
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "eastus"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    tags = {
        environment = "Terraform Demo"
    }
}

# Create subnet
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = azurerm_resource_group.myterraformgroup.name
    virtual_network_name = azurerm_virtual_network.myterraformnetwork.name
    address_prefixes       = ["10.0.1.0/24"]
}

# Create public IPs
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = azurerm_resource_group.myterraformgroup.name
    allocation_method            = "Dynamic"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
    resource_group_name = azurerm_resource_group.myterraformgroup.name

    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
    name                      = "myNIC"
    location                  = "eastus"
    resource_group_name       = azurerm_resource_group.myterraformgroup.name

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = azurerm_subnet.myterraformsubnet.id
        private_ip_address_allocation = "Dynamic"
        public_ip_address_id          = azurerm_public_ip.myterraformpublicip.id
    }

    tags = {
        environment = "Terraform Demo"
    }
}

# Connect the security group to the network interface
resource "azurerm_network_interface_security_group_association" "example" {
    network_interface_id      = azurerm_network_interface.myterraformnic.id
    network_security_group_id = azurerm_network_security_group.myterraformnsg.id
}

# Generate random text for a unique storage account name
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = azurerm_resource_group.myterraformgroup.name
    }

    byte_length = 8
}

# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = azurerm_resource_group.myterraformgroup.name
    location                    = "eastus"
    account_tier                = "Standard"
    account_replication_type    = "LRS"

    tags = {
        environment = "Terraform Demo"
    }
}

# Create (and display) an SSH key
resource "tls_private_key" "example_ssh" {
  algorithm = "RSA"
  rsa_bits = 4096
}
output "tls_private_key" { value = tls_private_key.example_ssh.private_key_pem }

# Create virtual machine
resource "azurerm_linux_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
    resource_group_name   = azurerm_resource_group.myterraformgroup.name
    network_interface_ids = [azurerm_network_interface.myterraformnic.id]
    size                  = "Standard_DS1_v2"

    os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        storage_account_type = "Premium_LRS"
    }

    source_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "18.04-LTS"
        version   = "latest"
    }

    computer_name  = "myvm"
    admin_username = "azureuser"
    disable_password_authentication = true

    admin_ssh_key {
        username       = "azureuser"
        public_key     = tls_private_key.example_ssh.public_key_openssh
    }

    boot_diagnostics {
        storage_account_uri = azurerm_storage_account.mystorageaccount.primary_blob_endpoint
    }

    tags = {
        environment = "Terraform Demo"
    }
}
```

## <a name="build-and-deploy-the-infrastructure"></a>构建并部署基础结构

创建 Terraform 模板后，第一步是初始化 Terraform。 此步骤可确保 Terraform 具有你要在 Azure 中生成模板的所有必备组件。

```bash
terraform init
```

下一步是让 Terraform 检查并验证模板。 此步骤将请求的资源与 Terraform 保存的状态信息进行比较，然后输出计划的执行。 此时不会创建 Azure 资源。

```bash
terraform plan
```

执行上述命令后，应会出现类似于以下屏幕的内容：

```console
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.
...

Note: You didn't specify an "-out" parameter to save this plan, so when
"apply" is called, Terraform can't guarantee this is what will execute.
  + azurerm_resource_group.myterraform
      <snip>
  + azurerm_virtual_network.myterraformnetwork
      <snip>
  + azurerm_network_interface.myterraformnic
      <snip>
  + azurerm_network_security_group.myterraformnsg
      <snip>
  + azurerm_public_ip.myterraformpublicip
      <snip>
  + azurerm_subnet.myterraformsubnet
      <snip>
  + azurerm_virtual_machine.myterraformvm
      <snip>
Plan: 7 to add, 0 to change, 0 to destroy.
```

如果所有内容正确无误，并且你已准备好在 Azure 中构建基础结构，请在 Terraform 中应用模板：

```bash
terraform apply
```

Terraform 完成后，VM 基础结构即已准备完毕。 可使用 [az vm show](/cli/azure/vm#az-vm-show) 获取 VM 的 公共 IP 地址：

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] -o tsv
```

然后，可以通过 SSH 连接到 VM：

```bash
ssh azureuser@<publicIps>
```

[!INCLUDE [terraform-troubleshooting.md](includes/terraform-troubleshooting.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [详细了解如何在 Azure 中使用 Terraform](/azure/terraform)