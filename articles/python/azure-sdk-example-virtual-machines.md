---
title: 使用用于 Python 的 Azure SDK 库预配虚拟机
description: 如何使用 Python 和 Azure SDK 管理库预配 Azure 虚拟机。
ms.date: 10/05/2020
ms.topic: conceptual
ms.custom: devx-track-python
ms.openlocfilehash: e01121047d42200e956345df611f82706b1e081e
ms.sourcegitcommit: f460914ac5843eb7392869a08e3a80af68ab227b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92010225"
---
# <a name="example-use-the-azure-libraries-to-provision-a-virtual-machine"></a>示例：使用 Azure 库预配虚拟机

此示例演示如何在 Python 脚本中使用 Azure SDK 管理库来创建包含 Linux 虚拟机的资源组。 （本文中的后面部分提供了[等效的 Azure CLI 命令](#for-reference-equivalent-azure-cli-commands)。 如果你想要使用 Azure 门户，请参阅[创建 Linux VM](/azure/virtual-machines/linux/quick-create-portal) 和[创建 Windows VM](/azure/virtual-machines/windows/quick-create-portal)。）

除非另行说明，否则本文中的所有资源在 Linux/macOS bash 和 Windows 命令行界面上的工作方式相同。

> [!NOTE]
> 通过代码预配虚拟机是一个多步骤过程，其中涉及预配虚拟机所需的多个其他资源。 如果只是从命令行运行此类代码，使用 [`az vm create`](/cli/azure/vm#az-vm-create) 命令会更容易，该命令会自动预配这些辅助资源（对于你选择省略的任何设置，会使用其默认值）。 仅需资源组、VM 名称、映像名称和登录凭据这些参数。 有关详细信息，请参阅[使用 Azure CLI 快速创建虚拟机](/azure/virtual-machines/scripts/virtual-machines-windows-cli-sample-create-vm-quick-create)。

## <a name="1-set-up-your-local-development-environment"></a>1：设置本地开发环境

如果尚未设置，请按照[为 Azure 配置本地 Python 开发环境](configure-local-development-environment.md)中的所有说明进行操作。

务必创建用于本地开发的服务主体，并为此项目创建虚拟环境，然后将其激活。

## <a name="2-install-the-needed-azure-library-packages"></a>2:安装所需的 Azure 库包

1. 创建一个 requirements.txt 文件，其中列出了此示例中使用的管理库：

    ```txt
    azure-mgmt-resource
    azure-mgmt-compute
    azure-mgmt-network
    azure-identity
    ```

1. 在激活了虚拟环境的终端或命令提示符下，安装 requirements.txt 中列出的管理库：

    ```cmd
    pip install -r requirements.txt
    ```

## <a name="3-write-code-to-provision-a-virtual-machine"></a>3：编写代码以预配虚拟机

创建包含以下代码的名为“provision_vm.py”的 Python 文件。 注释对详细信息进行了说明：

```python
# Import the needed credential and management objects from the libraries.
from azure.identity import AzureCliCredential
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.network import NetworkManagementClient
from azure.mgmt.compute import ComputeManagementClient
import os

print(f"Provisioning a virtual machine...some operations might take a minute or two.")

# Acquire a credential object using CLI-based authentication.
credential = AzureCliCredential()

# Retrieve subscription ID from environment variable.
subscription_id = os.environ["AZURE_SUBSCRIPTION_ID"]


# Step 1: Provision a resource group

# Obtain the management object for resources, using the credentials from the CLI login.
resource_client = ResourceManagementClient(credential, subscription_id)

# Constants we need in multiple places: the resource group name and the region
# in which we provision resources. You can change these values however you want.
RESOURCE_GROUP_NAME = "PythonAzureExample-VM-rg"
LOCATION = "centralus"

# Provision the resource group.
rg_result = resource_client.resource_groups.create_or_update(RESOURCE_GROUP_NAME,
    {
        "location": LOCATION
    }
)


print(f"Provisioned resource group {rg_result.name} in the {rg_result.location} region")

# For details on the previous code, see Example: Provision a resource group
# at https://docs.microsoft.com/azure/developer/python/azure-sdk-example-resource-group


# Step 2: provision a virtual network

# A virtual machine requires a network interface client (NIC). A NIC requires
# a virtual network and subnet along with an IP address. Therefore we must provision
# these downstream components first, then provision the NIC, after which we
# can provision the VM.

# Network and IP address names
VNET_NAME = "python-example-vnet"
SUBNET_NAME = "python-example-subnet"
IP_NAME = "python-example-ip"
IP_CONFIG_NAME = "python-example-ip-config"
NIC_NAME = "python-example-nic"

# Obtain the management object for networks
network_client = NetworkManagementClient(credential, subscription_id)

# Provision the virtual network and wait for completion
poller = network_client.virtual_networks.begin_create_or_update(RESOURCE_GROUP_NAME,
    VNET_NAME,
    {
        "location": LOCATION,
        "address_space": {
            "address_prefixes": ["10.0.0.0/16"]
        }
    }
)

vnet_result = poller.result()

print(f"Provisioned virtual network {vnet_result.name} with address prefixes {vnet_result.address_space.address_prefixes}")

# Step 3: Provision the subnet and wait for completion
poller = network_client.subnets.begin_create_or_update(RESOURCE_GROUP_NAME, 
    VNET_NAME, SUBNET_NAME,
    { "address_prefix": "10.0.0.0/24" }
)
subnet_result = poller.result()

print(f"Provisioned virtual subnet {subnet_result.name} with address prefix {subnet_result.address_prefix}")

# Step 4: Provision an IP address and wait for completion
poller = network_client.public_ip_addresses.begin_create_or_update(RESOURCE_GROUP_NAME,
    IP_NAME,
    {
        "location": LOCATION,
        "sku": { "name": "Standard" },
        "public_ip_allocation_method": "Static",
        "public_ip_address_version" : "IPV4"
    }
)

ip_address_result = poller.result()

print(f"Provisioned public IP address {ip_address_result.name} with address {ip_address_result.ip_address}")

# Step 5: Provision the network interface client
poller = network_client.network_interfaces.begin_create_or_update(RESOURCE_GROUP_NAME,
    NIC_NAME, 
    {
        "location": LOCATION,
        "ip_configurations": [ {
            "name": IP_CONFIG_NAME,
            "subnet": { "id": subnet_result.id },
            "public_ip_address": {"id": ip_address_result.id }
        }]
    }
)

nic_result = poller.result()

print(f"Provisioned network interface client {nic_result.name}")

# Step 6: Provision the virtual machine

# Obtain the management object for virtual machines
compute_client = ComputeManagementClient(credential, subscription_id)

VM_NAME = "ExampleVM"
USERNAME = "azureuser"
PASSWORD = "ChangePa$$w0rd24"

print(f"Provisioning virtual machine {VM_NAME}; this operation might take a few minutes.")

# Provision the VM specifying only minimal arguments, which defaults to an Ubuntu 18.04 VM
# on a Standard DS1 v2 plan with a public IP address and a default virtual network/subnet.

poller = compute_client.virtual_machines.begin_create_or_update(RESOURCE_GROUP_NAME, VM_NAME,
    {
        "location": LOCATION,
        "storage_profile": {
            "image_reference": {
                "publisher": 'Canonical',
                "offer": "UbuntuServer",
                "sku": "16.04.0-LTS",
                "version": "latest"
            }
        },
        "hardware_profile": {
            "vm_size": "Standard_DS1_v2"
        },
        "os_profile": {
            "computer_name": VM_NAME,
            "admin_username": USERNAME,
            "admin_password": PASSWORD
        },
        "network_profile": {
            "network_interfaces": [{
                "id": nic_result.id,
            }]
        }
    }
)

vm_result = poller.result()

print(f"Provisioned virtual machine {vm_result.name}")
```

[!INCLUDE [cli-auth-note](includes/cli-auth-note.md)]

### <a name="reference-links-for-classes-used-in-the-code"></a>代码中使用的类的参考链接

- [AzureCliCredential (azure.identity)](/python/api/azure-identity/azure.identity.azureclicredential)
- [ResourceManagementClient (azure.mgmt.resource)](/python/api/azure-mgmt-resource/azure.mgmt.resource.resourcemanagementclient)
- [NetworkManagementClient (azure.mgmt.network)](/python/api/azure-mgmt-network/azure.mgmt.network.networkmanagementclient)
- [ComputeManagementClient (azure.mgmt.compute)](/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient)

## <a name="4-run-the-script"></a>4.运行脚本

```cmd
python provision_vm.py
```

预配过程需要几分钟才能完成。

## <a name="5-verify-the-resources"></a>5.验证资源

打开 [Azure 门户](https://portal.azure.com)，导航到“PythonAzureExample-VM-rg”资源组，并记下虚拟机、虚拟磁盘、网络安全组、公共 IP 地址、网络接口和虚拟网络：

![显示虚拟机和相关资源的新资源组的 Azure 门户页面](media/azure-sdk-example-virtual-machines/portal-vm-resources.png)

### <a name="for-reference-equivalent-azure-cli-commands"></a>有关参考：等效 Azure CLI 命令

# <a name="cmd"></a>[cmd](#tab/cmd)

```azurecli
rem Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

rem Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet ^
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet ^
    --subnet-prefix 10.0.0.0/24

rem Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip ^
    --allocation-method Dynamic --version IPv4

rem Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet ^
    --subnet python-example-subnet -n python-example-nic ^
    --public-ip-address python-example-ip

rem Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" ^
    --nics python-example-nic --image UbuntuLTS ^
    --admin-username azureuser --admin-password ChangePa$$w0rd24
```

# <a name="bash"></a>[bash](#tab/bash)

```azurecli
# Provision the resource group

az group create -n PythonAzureExample-VM-rg -l centralus

# Provision a virtual network and subnet

az network vnet create -g PythonAzureExample-VM-rg -n python-example-vnet \
    --address-prefix 10.0.0.0/16 --subnet-name python-example-subnet \
    --subnet-prefix 10.0.0.0/24

# Provision a public IP address

az network public-ip create -g PythonAzureExample-VM-rg -n python-example-ip \
    --allocation-method Dynamic --version IPv4

# Provision a network interface client

az network nic create -g PythonAzureExample-VM-rg --vnet-name python-example-vnet \
    --subnet python-example-subnet -n python-example-nic \
    --public-ip-address python-example-ip

# Provision the virtual machine

az vm create -g PythonAzureExample-VM-rg -n ExampleVM -l "centralus" \
    --nics python-example-nic --image UbuntuLTS \
    --admin-username azureuser --admin-password ChangePa$$w0rd24

```

---

## <a name="6-clean-up-resources"></a>6：清理资源

```azurecli
az group delete -n PythonAzureExample-VM-rg  --no-wait
```

如果不需要保留在此示例中创建的资源，并想要避免订阅中的持续费用，则运行此命令。

[!INCLUDE [resource_group_begin_delete](includes/resource-group-begin-delete.md)]

## <a name="see-also"></a>另请参阅

- [示例：预配资源组](azure-sdk-example-resource-group.md)
- [示例：列出订阅中的资源组](azure-sdk-example-list-resource-groups.md)
- [示例：预配 Azure 存储](azure-sdk-example-storage.md)
- [示例：使用 Azure 存储](azure-sdk-example-storage-use.md)
- [示例：预配 Web 应用并部署代码](azure-sdk-example-web-app.md)
- [示例：预配和查询数据库](azure-sdk-example-database.md)

以下资源容器使用 Python 创建虚拟机的更全面的示例：

- [在 Azure 中使用 Python 创建和管理 Windows VM](/azure/virtual-machines/windows/python)。 可以通过更改 `storage_profile` 参数，使用此示例创建 Linux VM。
- [Azure 虚拟机管理示例 - Python](https://github.com/Azure-Samples/virtual-machines-python-manage) (GitHub)。 此示例演示了其他管理操作，如启动和重启 VM、停止和删除 VM、增加磁盘大小和管理数据磁盘。
