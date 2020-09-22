---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 4c4bb046496f80a7280aed429a8cd251e533a8db
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90681662"
---
#### <a name="ansible"></a>[Ansible](#tab/ansible)

1. 将以下代码另存为 `delete_rg.yml`。

    ```yml
    ---
    - hosts: localhost
      tasks:
        - name: Deleting resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            state: absent
          register: rg
        - debug:
            var: rg
    ```

1. 使用 [ansible-playbook](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html) 命令运行 playbook。 将占位符替换为要删除的资源组的名称。 将删除资源组内的所有资源。

    ```bash
    ansible-playbook delete_rg.yml --extra-vars "name=<resource_group>"
    ```

    **注释**：

    - 由于 playbook 的 `register` 变量和 `debug` 部分，因此在命令完成时，将显示结果。
    
#### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. 运行 [az group delete](https://docs.microsoft.com/cli/azure/group#az_group_delete) 以删除资源组。 将删除资源组内的所有资源。

    ```azurecli
    az group delete --name <resource_group>
    ```

1. 使用 [az group show](https://docs.microsoft.com/cli/azure/group#az_group_show) 验证资源组是否已删除。

    ```azurecli
    az group show --name <resource_group>
    ```

#### <a name="azure-powershell"></a>[Azure PowerShell](#tab/azure-powershell)

1. 运行 [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Remove-AzResourceGroup) 以删除资源组。 将删除资源组内的所有资源。

    ```azurepowershell
    Remove-AzResourceGroup -Name <resource_group>
    ```

1. 使用 [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/Get-AzResourceGroup) 验证资源组是否已删除。

    ```azurepowershell
    Get-AzResourceGroup -Name <resource_group>
    ```

---
