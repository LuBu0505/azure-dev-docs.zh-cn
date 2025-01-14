---
title: 教程 - 使用 Ansible 配置 Azure 虚拟网络对等互连
description: 了解如何使用 Ansible 通过虚拟网络对等互连来连接虚拟网络。
keywords: ansible, azure, devops, bash, playbook, 网络, 对等互连
ms.topic: tutorial
ms.date: 04/30/2019
ms.custom: devx-track-ansible
ms.openlocfilehash: 747b11c9727e0844ac9d9c7b07a8355e8163c75e
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90681961"
---
# <a name="tutorial-configure-azure-virtual-network-peering-using-ansible"></a>教程：使用 Ansible 配置 Azure 虚拟网络对等互连

[!INCLUDE [ansible-28-note.md](includes/ansible-28-note.md)]

使用[虚拟网络 (VNet) 对等互连](/azure/virtual-network/virtual-network-peering-overview)可以无缝连接两个 Azure 虚拟网络。 对等互连后，出于连接目的，两个虚拟网络会显示为一个。 

将会通过专用 IP 地址在同一虚拟网络中的 VM 之间路由流量。 同样，将会通过 Microsoft 主干基础结构在对等互连虚拟网络中的 VM 之间路由流量。 因此，不同虚拟网络中的 VM 可以相互通信。

[!INCLUDE [ansible-tutorial-goals.md](includes/ansible-tutorial-goals.md)]

> [!div class="checklist"]
>
> * 创建两个虚拟网络
> * 对等互连两个虚拟网络
> * 删除两个网络之间的对等互连

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]

## <a name="create-two-resource-groups"></a>创建两个资源组

资源组是在其中部署和管理 Azure 资源的逻辑容器。

本部分的示例 playbook 代码用于：

- 创建两个资源组 

```yml
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
```

## <a name="create-the-first-virtual-network"></a>创建第一个虚拟网络

本部分的示例 playbook 代码用于：

- 创建虚拟网络
- 在虚拟网络中创建子网

```yml
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
```

## <a name="create-the-second-virtual-network"></a>创建第二个虚拟网络

本部分的示例 playbook 代码用于：

- 创建虚拟网络
- 在虚拟网络中创建子网

```yml
  - name: Create second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
```

## <a name="peer-the-two-virtual-networks"></a>对等互连两个虚拟网络

本部分的示例 playbook 代码用于：

- 初始化虚拟网络对等互连
- 对等互连前面创建的两个虚拟网络

```yml
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true
```

## <a name="delete-the-virtual-network-peering"></a>删除虚拟网络对等互连

本部分的示例 playbook 代码用于：

- 删除前面创建的两个虚拟网络之间的对等互连

```yml
  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="get-the-sample-playbook"></a>获取示例 playbook

可通过两种方法获取完整示例 playbook：

- [下载 playbook](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vnet_peering.yml) 并将其保存到 `vnet_peering.yml`。
- 新建一个名为 `vnet_peering.yml` 的文件，并将以下内容复制到其中：

```yml
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 1000 | random }}"
      run_once: yes

- name: Connect virtual networks with virtual network peering
  hosts: localhost
  connection: local
  vars:
    resource_group: "{{ resource_group_name }}"
    resource_group_secondary: "{{ resource_group_name }}2"
    vnet_name1: "myVnet{{ rpfx }}"
    vnet_name2: "myVnet{{ rpfx }}2"
    peering_name: peer1
    location: eastus2
  tasks:
  - name: Create a resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group }}"
      location: "{{ location }}"
  - name: Create secondary resource group
    azure_rm_resourcegroup:
      name: "{{ resource_group_secondary }}"
      location: "{{ location }}"
  - name: Create first virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name1 }}"
      address_prefix: "10.0.0.0/24"
      virtual_network: "{{ vnet_name1 }}"
  - name: Create second virtual network
    azure_rm_virtualnetwork:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ vnet_name2 }}"
      address_prefixes: "10.1.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: "{{ resource_group }}"
      name: "{{ vnet_name2 }}"
      address_prefix: "10.1.0.0/24"
      virtual_network: "{{ vnet_name2 }}"
  - name: Initial vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group_secondary }}"
        name: "{{ vnet_name2 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Connect vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group_secondary }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name2 }}"
      remote_virtual_network:
        resource_group: "{{ resource_group }}"
        name: "{{ vnet_name1 }}"
      allow_virtual_network_access: true
      allow_forwarded_traffic: true

  - name: Delete vnet peering
    azure_rm_virtualnetworkpeering:
      resource_group: "{{ resource_group }}"
      name: "{{ peering_name }}"
      virtual_network: "{{ vnet_name1 }}"
      state: absent
```

## <a name="run-the-sample-playbook"></a>运行示例 playbook

本部分的示例 playbook 代码用于测试本教程中所示的各项功能。

下面是使用示例 playbook 时要考虑的部分关键注意事项：

- 在 `vars` 节中，将 `{{ resource_group_name }}` 占位符替换为你的资源组名称。

使用 ansible-playbook 命令运行 playbook：

```bash
ansible-playbook vnet_peering.yml
```

运行 playbook 后，可看到类似于以下结果的输出：

```Output
PLAY [localhost] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Prepare random postfix] 
ok: [localhost]

PLAY [Connect virtual networks with virtual network peering] 

TASK [Gathering Facts] 
ok: [localhost]

TASK [Create a resource group] 
changed: [localhost]

TASK [Create secondary resource group] 
changed: [localhost]

TASK [Create first virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Create second virtual network] 
changed: [localhost]

TASK [Add subnet] 
changed: [localhost]

TASK [Initial vnet peering] 
changed: [localhost]

TASK [Connect vnet peering] 
changed: [localhost]

TASK [Delete vnet peering] 
changed: [localhost]

PLAY RECAP 
localhost                  : ok=12   changed=9    unreachable=0    failed=0    skipped=0   rescued=0    ignored=0
```

## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。    

本部分的示例 playbook 代码用于：    

- 删除前面创建的两个资源组   

将以下 playbook 保存为 `cleanup.yml`：   

```bash 
- hosts: localhost  
  vars: 
    resource_group: "{{ resource_group_name-1 }}"   
    resource_group_secondary: "{{ resource_group_name-2 }}" 
  tasks:    
    - name: Delete a resource group 
      azure_rm_resourcegroup:   
        name: "{{ resource_group }}"    
        force_delete_nonempty: yes  
        state: absent   
    - name: Delete a resource group 
      azure_rm_resourcegroup:   
        name: "{{ resource_group_secondary }}"  
        force_delete_nonempty: yes  
        state: absent   
``` 

下面是使用示例 playbook 时要考虑的部分关键注意事项：  

- 请将 `{{ resource_group_name-1 }}` 占位符替换为创建的第一个资源组的名称。  
- 请将 `{{ resource_group_name-2 }}` 占位符替换为创建的第二个资源组的名称。 
- 两个指定资源组中的所有资源将被删除。   

使用 ansible-playbook 命令运行 playbook：    

```bash 
ansible-playbook cleanup.yml    
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [Azure 上的 Ansible](/azure/ansible/)