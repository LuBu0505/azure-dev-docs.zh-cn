---
author: tomarchermsft
ms.service: ansible
ms.topic: include
ms.date: 09/15/2020
ms.author: tarcher
ms.openlocfilehash: 5351445b63618d072ecf4cf8beeffc9a071bd84d
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682039"
---
本部分介绍如何在新的 Ansible 配置内创建测试资源组。 如果无需创建，可跳过本部分。

### <a name="create-an-azure-resource-group"></a>创建 Azure 资源组

1. 将以下代码另存为 `create_rg.yml`。

    ```yaml
    ---
    - hosts: localhost
      connection: local
      tasks:
        - name: Creating resource group - "{{ name }}"
          azure_rm_resourcegroup:
            name: "{{ name }}"
            location: "{{ location }}"
          register: rg
        - debug:
            var: rg
    ```

1. 使用 [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) 运行 playbook。 将占位符替换为要创建的资源组的名称和位置。

    ```bash
    ansible-playbook create_rg.yml --extra-vars "name=<resource_group_name> location=<resource_group_location>"
    ```

    **注释**：

    - 由于 playbook 的 `register` 变量和 `debug` 部分，因此在命令完成时，将显示结果。

### <a name="delete-an-azure-resource-group"></a>删除 Azure 资源组

[!INCLUDE [ansible-delete-resource-group.md](ansible-delete-resource-group.md)]
