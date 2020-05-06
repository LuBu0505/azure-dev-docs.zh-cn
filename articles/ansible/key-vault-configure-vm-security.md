---
title: 教程 - 通过 Ansible 将 Azure Key Vault 与 Linux 虚拟机配合使用
description: 了解如何使用 Ansible 通过 Azure Key Vault 配置 VM 安全性
keywords: ansible, azure, 开发, 密钥保管库, 安全性, 凭据, 机密, 密钥, 证书, 适用于 azure 的 ansible 模块, 资源组, azure_rm_resourcegroup,
ms.topic: tutorial
ms.date: 04/20/2020
ms.openlocfilehash: ce9adb7ea121425d410665e1f4cc225cfdb82bd8
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81755230"
---
# <a name="tutorial-use-azure-key-vault-with-a-linux-virtual-machine-in-ansible"></a>教程：通过 Ansible 将 Azure Key Vault 与 Linux 虚拟机配合使用

[!INCLUDE [ansible-29-note.md](includes/ansible-29-note.md)]

本教程介绍如何在使用 [Azure Key Vault](/azure/key-vault/general/overview) 的过程中使用适用于 Azure 模块的 Ansible 集合。 Azure Key Vault 用于集中存储凭据（例如应用程序机密、密钥和证书）。 将凭据和应用程序代码分离有助于加强系统的安全性。 另外，如果在使用自动到期日期的情况下实施轮换凭据管理模式，那么可管理性会变得高得多。

> [!div class="checklist"]
>
> * 使用 Azure CLI 获取 Azure 订阅和服务主体值
> * 将键值存储为 Linux 环境变量
> * 从 Ansible playbook 获取 Linux 环境变量
> * 创建 key vault
> * 设置密钥保管库的访问策略
> * 使用 Azure 门户向密钥保管库添加访问策略
> * 创建密钥保管库机密
> * 使用 Ansible shell 模块获取密钥保管库机密
> * 创建虚拟机及其所有构成组件

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [ansible-prereqs-cloudshell-use-or-vm-creation2.md](includes/ansible-prereqs-cloudshell-use-or-vm-creation2.md)]
[!INCLUDE [ansible-configure-azure-collection.md](includes/ansible-configure-azure-collection.md)]
    
## <a name="get-azure-subscription-info"></a>获取 Azure 订阅信息

通过 Azure CLI 获取在使用适用于 Azure 的 Ansible 模块时所需的 Azure 订阅信息。 

1. 使用 `az account show` 命令获取 Azure 订阅 ID 和 Azure 订阅租户 ID。 对于 `<Subscription>` 占位符，请指定 Azure 订阅名称或 Azure 订阅 ID。 此命令会显示与默认 Azure 订阅关联的许多键值。 如果有多个订阅，可能需要通过 [az account set](/cli/azure/account?view=azure-cli-latest#az-account-set) 命令设置当前订阅。 记下该命令的输出中的  ID 值和 tenantID  值。

    ```azurecli
    az account show --subscription "<Subscription>" --query tenantId
    ```

1. 如果没有 Azure 订阅的服务主体，请 [通过 Azure CLI 创建 Azure 服务主体](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)。 记下该命令的输出中的 appId  值。

1. 使用 `az ad sp show` 命令获取服务主体的对象 ID。 对于 `<ApplicationID>` 占位符，请指定服务主体 appId。 `--query` 参数指示哪一个值要输出到 stdout  。 在此示例中，它是服务主体对象 ID。

    ```azurecli
    az ad sp show --id <ApplicationID> --query objectId
    ```
    
1. 将以下值存储为环境变量：Azure 订阅 ID、Azure 订阅租户 ID、服务主体对象 ID。 运行 playbook 的方式、存储订阅和凭据值的位置以及检索这些值的方式取决于你的具体环境。 出于本演示的目的，我使用了 Azure Cloud Shell，并且通过将以下行添加到文件末尾，来将所需的 Azure 值存储到 `~/.bashrc` 中：

    ```bash
    export AZ_SUBSCRIPTION_ID="<subscriptionID>"
    export AZ_TENANT_ID="<tenantID>"
    export AZ_OBJECT_ID="<objectID>"
    export AZ_CLIENT_ID="<appID>"
    ```

## <a name="declare-the-azure-collection"></a>声明 Azure 集合

[下载最新的 Azure 集合](#prerequisites)后，通过 `collections` 键指定其用途。

```yml
- hosts: all
  collections:
    - azure.azcollection
```

## <a name="create-azure-resource-group-for-the-key-vault"></a>为密钥保管库创建 Azure 资源组

以下 playbook 代码片段创建一个有独一无二的名称的资源组，密钥保管库将创建到该组中。 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"

    - name: Create a resource group to hold the key vault
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"
```

注意： 

- 在此演示中，密钥保管库是作为资源组中的唯一资源创建的。 常见做法是将密钥保管库与使用它的资源分开。 这种模式可防止在删除其他资源时意外删除密钥保管库。
- 由于密钥保管库名称必须在 Azure 中独一无二，因此本演示创建一个随机的后缀  值。 此值将追加到密钥保管库资源组和密钥保管库（在下一部分创建）的名称后面。 `Prepare random postfix` 任务中的代码会生成此随机后缀值，此值被赋予 `rpfx` 变量。
- 在 `Set facts` 任务中，`lookup` 命令用于检索作为环境变量存储的 Azure 订阅 ID。
- [azure_rm_resourcegroup 模块](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html)用于创建新资源组。
- playbook 末尾的 `debug` 任务会显示新资源组的名称。

## <a name="create-a-key-vault"></a>创建 key vault

如上一部分所述，密钥保管库名称必须在 Azure 中独一无二。 因此，以下 playbook 代码片段将文本 `kv` 和 `rpfx` 值串联后赋予 `kv` 变量。

```yml
vars:
  kv: "kv{{ rpfx }}"
  kv_rg: "kv_rg_{{ rpfx }}"

tasks:
  - name: Set facts
    set_fact:
      az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
      az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"

  - name: Create a key vault
    azure_rm_keyvault:
      subscription_id: "{{ az_sub_id }}"
      resource_group: "{{ kv_rg }}"
      vault_name: "{{ kv }}"
      vault_tenant: "{{ az_tenant_id }}"
      enabled_for_deployment: yes
      sku:
        name: standard
        family: A
      access_policies:
        - object_id: "{{ az_object_id }}"
          tenant_id: "{{ az_tenant_id }}"
          secrets:
            - get
            - list
            - set
  - debug:
      msg: "New key vault name = {{ kv }} within the {{ kv_rg }} resource group"

```

注意： 

- [azure_rm_keyvault 模块](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvault_module.html)用于创建密钥保管库。
- 在将访问策略作为密钥保管库的一部分创建时，已经提供了对象 ID 和租户 ID。 这些值定义特定服务主体（与某个 Azure 订阅关联的服务主体）的访问策略。 但是，当你浏览到 Azure 门户并尝试查看密钥保管库的机密时，可能会出现错误消息。 这些消息可能类似于“此密钥保管库的访问策略中未启用操作‘列出’。” 和“你无权查看这些内容。” 收到这些消息表明作为 Active Directory 用户的你没有访问权限。 下一部分说明如何将针对你自己的访问策略添加到密钥保管库。
- playbook 末尾的 `debug` 任务会显示新密钥保管库的名称。

## <a name="add-yourself-to-key-vault-access-policy"></a>将自己添加到密钥保管库访问策略

若要查看密钥保管库的机密，或者要完成演示步骤，则需添加针对你的 Azure Active Directory ID 的访问策略。 以下步骤详述如何作为用户向密钥保管库添加针对你自己的访问策略：

1. 浏览到 [Azure 门户](https://portal.azure.com)。

1. 在页面的主搜索框中输入“`key vaults`”，然后在“服务”下选择“密钥保管库”。  

1. 选择在上一部分创建的密钥保管库。 （名称已从 playbook 输出到 stdout。）

1. 选择“访问策略”。 

1. 此时会显示单个访问策略，其中包含表示指定服务主体的 Azure Active Directory ID。

1. 选择“添加访问策略”。 

1. 选择“选择主体”。 

1. 在“主体”选项卡的搜索框中，输入你的电子邮件地址。 

1. 从筛选的列表中选择相应的条目。

1. 所选用户的信息将复制到“所选成员”  列表。 选择“选择”  。

1. 选择“密钥权限”、“机密权限”和“证书权限”的相应选项。    对于本演示，请先选择“机密权限”，然后选择“获取”、“列出”和“设置”即可。    

1. 选择 **添加** 。

1. 所选用户的新访问策略现在会显示在“访问策略”页上。 

1. 选择“保存”。 

1. 选择门户右上角的“通知”。  等到访问策略已更新后，转到下一步。

## <a name="create-a-key-vault-secret"></a>创建密钥保管库机密

以下 Ansible playbook 代码片段演示如何创建密钥保管库机密：

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

  tasks:
    - name: Set facts
      set_fact:
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"
```

注意： 

- [azure_rm_keyvaultsecret 模块](https://docs.ansible.com/ansible/latest/modules/azure_rm_keyvaultsecret_module.html)用于创建密钥保管库机密。
- 为了简单起见，本演示包含 `secret_name` 和 `secret_value`。 不过，就像你的项目的任何源代码一样，playbook 是基础结构即代码 (IaC) 文件。 因此，在生产环境中使用时，这样的值不应存储在纯文本文件中。
- 运行此代码后，密钥保管库的“机密”  选项卡会列出新添加的名为 `testsecret` 的机密。 若要查看它，请依次选择机密、当前版本和“显示机密值”。 

## <a name="get-a-key-vault-secret"></a>获取密钥保管库机密

以下 Ansible playbook 代码片段演示如何获取最新版密钥保管库机密：

```yml
  vars:
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

tasks:
    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']
```

注意： 

- **azure_rm_keyvaultsecret_info 模块**用于获取密钥保管库机密。 只有在使用适用于 Azure 模块的 Ansible 集合的情况下，此模块才可用。 
- 如果在运行此代码片段时收到错误，请确保已遵循[“先决条件”部分](#prerequisites)中的所有说明。
- 为了简单起见，本演示包含 `secret_name` 和 `secret_value`。 不过，就像你的项目的任何源代码一样，playbook 是基础结构即代码 (IaC) 文件。 因此，在生产环境中使用时，这样的值不应存储在纯文本文件中。

## <a name="run-the-complete-playbook"></a>运行完整的 playbook

创建密钥保管库及其机密后，可以在保护虚拟机等 Azure 资源时使用该信息。 以下 Ansible playbook 执行本教程中从头至尾演示的全部任务并创建完整的虚拟机。 

```yml
---
- hosts: localhost
  tasks:
    - name: Prepare random postfix
      set_fact:
        rpfx: "{{ 10000 | random }}"
      run_once: yes
      
- hosts: localhost
  collections:
    - azure.azcollection
  vars:
    kv_rg: kv_rg_{{ rpfx }}
    kv_rg_loc: eastus
    kv: "kv{{ rpfx }}"
    kv_uri: "https://{{ kv }}.vault.azure.net"
    kv_secret_name: testsecret
    kv_secret_value: MySecret007$

    # Test VM vars
    test_vm_rg: kv_test_vm_rg
    test_vm_rg_loc: eastus
    test_vm: kvtestvm
    test_vm_vnet: "kv_test_vm_vnet"
    test_vm_subnet: kv_test_vm_subnet
    test_vm_public_ip: kv_test_vm_public_ip
    test_vm_nsg: kv_test_vm_nsg
    test_vm_nsg_list: 
      - name: Allow-SSH
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 300
        port: 22 
        source_address_prefix: Internet
      - name: Allow-HTTP
        access: Allow
        protocol: Tcp
        direction: Inbound
        priority: 100
        port: 80
        source_address_prefix: Internet 
    test_vm_nic: kv_test_vnic
    admin_username: testadmin

  tasks:
    - name: Set facts
      set_fact:
        az_sub_id: "{{ lookup('env', 'AZ_SUBSCRIPTION_ID') }}"
        az_object_id: "{{ lookup('env', 'AZ_OBJECT_ID') }}"
        az_tenant_id: "{{ lookup('env', 'AZ_TENANT_ID') }}"
        az_client_id: "{{ lookup('env', 'AZ_CLIENT_ID') }}"

    - name: Create a resource group to hold the Key Vault instance
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ kv_rg }}"
        location: "{{ kv_rg_loc }}"

    - debug:
        msg: "New resource group = {{ kv_rg }}"

    - name: Create instance of Key Vault
      azure_rm_keyvault:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ kv_rg }}"
        vault_name: "{{ kv }}"
        vault_tenant: "{{ az_tenant_id }}"
        enabled_for_deployment: yes
        sku:
          name: standard
          family: A
        access_policies:
          - object_id: "{{ az_object_id }}"
            tenant_id: "{{ az_tenant_id }}"
            secrets:
              - get
              - list
              - set

    - debug:
        msg: "New Key Vault instance name = {{ kv }} within the {{ kv_rg }} resource group"

    - name: Create a secret
      azure_rm_keyvaultsecret:
        subscription_id: "{{ az_sub_id }}"
        client_id: "{{ az_client_id }}"
        keyvault_uri: "{{ kv_uri }}"
        secret_name: "{{ kv_secret_name }}"
        secret_value: "{{ kv_secret_value }}"

    - name: Register Key Vault provider.
      shell:
        az provider register -n Microsoft.KeyVault

    - name: Get latest version of a secret
      azure_rm_keyvaultsecret_info:
        vault_uri: "{{ kv_uri }}"
        name: "{{ kv_secret_name }}"
      register: output
    - debug:
        var: output['secrets'][0]['secret']

    - name: Create resource group for test VM.
      azure_rm_resourcegroup:
        subscription_id: "{{ az_sub_id }}"
        name: "{{ test_vm_rg }}"
        location: "{{ test_vm_rg_loc }}"

    - name: Create virtual network.
      azure_rm_virtualnetwork:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_vnet }}"
        address_prefixes: "172.16.0.0/16"

    - name: Create subset within virtual network.
      azure_rm_subnet:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        virtual_network_name: "{{ test_vm_vnet }}"
        name: "{{ test_vm_subnet }}"
        address_prefix_cidr:  "172.16.10.0/24"

    - name: Create public IP address.
      azure_rm_publicipaddress:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        allocation_method: Static
        name: "{{ test_vm_public_ip }}"

    - name: Create Network Security Group and rules.
      azure_rm_securitygroup:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nsg}}"
        rules:
          - name: "{{ item.name }}"
            access: "{{ item.access }}"
            protocol: "{{ item.protocol }}"
            direction: "{{ item.direction }}"
            destination_port_range: "{{ item.port }}"
            priority: "{{ item.priority }}"
            source_address_prefix: "{{ item.source_address_prefix }}"
      loop: "{{ test_vm_nsg_list }}"

    - name: Create virtual network interface card (NIC).
      azure_rm_networkinterface:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm_nic }}"
        virtual_network: "{{ test_vm_vnet }}"
        subnet: "{{ test_vm_subnet }}"
        ip_configurations:
          - name: ipconfig
            public_ip_address_name: "{{ test_vm_public_ip }}"
            primary: yes
        security_group: "{{ test_vm_nsg }}"

    - name: Create virtual machine.
      azure_rm_virtualmachine:
        subscription_id: "{{ az_sub_id }}"
        resource_group: "{{ test_vm_rg }}"
        name: "{{ test_vm }}"
        admin_username: " {{ admin_username }} "
        admin_password: " {{ output['secrets'][0]['secret'] }}"
        vm_size: Standard_B1ms
        network_interfaces: "{{ test_vm_nic }}"
        image:
          offer: UbuntuServer
          publisher: Canonical
          sku: 16.04-LTS
          version: latest

```

注意： 

- 虚拟机的管理员  密码设置为密钥保管库机密。
- 能否一次运行整个 playbook 取决于你的测试环境。 在创建密钥之前，可能需要手动将自己添加到密钥保管库的访问策略。 此任务在[创建密钥保管库](#create-a-key-vault)和[将自己添加到密钥保管库访问策略](#add-yourself-to-key-vault-access-policy)部分进行了介绍。
- 可以看到，我们使用了许多不同的 Ansible 模块来创建 Azure 虚拟机及其所有构成组件。 若要详细了解各种用于创建虚拟机的 Ansible 模块，请使用以下列表：
    - [Azure 资源组模块 (azure_rm_resourcegroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_resourcegroup_module.html)
    - [Azure 虚拟网络模块 (azure_rm_virtualnetwork)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualnetwork_module.html)
    - [Azure 虚拟网络子网模块 (azure_rm_subnet)](https://docs.ansible.com/ansible/latest/modules/azure_rm_subnet_module.html)
    - [Azure 公共 IP 模块 (azure_rm_publicipaddress)](https://docs.ansible.com/ansible/latest/modules/azure_rm_publicipaddress_module.html)
    - [Azure 网络安全组模块 (azure_rm_securitygroup)](https://docs.ansible.com/ansible/latest/modules/azure_rm_securitygroup_module.html)
    - [Azure 网络接口 (azure_rm_networkinterface)](https://docs.ansible.com/ansible/latest/modules/azure_rm_networkinterface_module.html)
    - [Azure 虚拟机 (azure_rm_virtualmachine)](https://docs.ansible.com/ansible/latest/modules/azure_rm_virtualmachine_module.html)
    
## <a name="clean-up-resources"></a>清理资源

如果不再需要本教程中创建的资源，请将其删除。 请将 `<kv_rg>` 占位符替换为用来保存演示版密钥保管库的资源组。

```yml
- hosts: localhost
  vars:
    kv_rg: <kv_rg>
    test_vm_rg: kv_test_vm_rg
  tasks:
    - name: Delete the key vault resource group
      azure_rm_resourcegroup:
        name: "{{ kv_rg }}"
        force_delete_nonempty: yes
        state: absent
    - name: Delete the test vm resource group
      azure_rm_resourcegroup:
        name: "{{ test_vm_rg }}"
        force_delete_nonempty: yes
        state: absent
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [Azure Key Vault 安全性概述](/azure/key-vault/general/overview-security)