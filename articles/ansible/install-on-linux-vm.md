---
title: 快速入门 - 使用 Azure CLI 配置 Ansible
description: 本快速入门介绍如何在 Ubuntu、CentOS 和 SLES 上安装和配置 Ansible 以管理 Azure 资源
keywords: ansible, azure, devops, bash, cloudshell, playbook, azure cli
ms.topic: quickstart
ms.date: 08/13/2020
ms.custom: devx-track-ansible,devx-track-cli
ms.openlocfilehash: bdda836789e9230cffdc14a6ee4bd87ddb2ce5ef
ms.sourcegitcommit: 39f3f69e3be39e30df28421a30747f6711c37a7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90831165"
---
# <a name="quickstart-configure-ansible-using-azure-cli"></a>快速入门：使用 Azure CLI 配置 Ansible

本快速入门介绍如何使用 Azure CLI 安装 [Ansible](https://docs.ansible.com/)。

在本快速入门中，我们将完成以下任务：

> [!div class="checklist"]
> * 创建 SSH 密钥对
> * 创建资源组
> * 创建 CentOS 虚拟机 
> * 在虚拟机上安装 Ansible
> * 通过 SSH 连接到虚拟机
> * 在虚拟机上配置 Ansible

## <a name="prerequisites"></a>先决条件

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
[!INCLUDE [open-source-devops-prereqs-create-sp.md](../includes/open-source-devops-prereqs-create-service-principal.md)]
- **对 Linux 或 Linux 虚拟机的访问权限** - 如果没有 Linux 计算机，请创建 [Linux 虚拟机](/azure/virtual-network/quick-create-cli)。

## <a name="create-an-ssh-key-pair"></a>创建 SSH 密钥对

连接到 Linux VM 时，可以使用密码验证或基于密钥的身份验证。 基于密钥的身份验证比使用密码更安全。 因此，本文将使用基于密钥的身份验证。

基于密钥的身份验证有两种密钥：

- **公钥**：公钥存储在主机上（例如在本文中存储在 VM 上）
- **私钥**：使用私钥，可以安全地连接到主机。 私钥实际上就是你的密码，因此应该加以保护。
        
以下步骤将指导你创建 SSH 密钥对。

1. 登录 [Azure 门户](https://portal.azure.com)。

1. 打开 [Azure Cloud Shell](/azure/cloud-shell/overview)，并切换到 Bash（如果尚未切换）。

1. 使用 [ssh-keygen](https://www.ssh.com/ssh/keygen/) 创建 SSH 密钥。

    ```bash
    ssh-keygen -m PEM -t rsa -b 2048 -C "azureuser@azure" -f ~/.ssh/ansible_rsa -N ""
    ```

    **注释**：

    - `ssh-keygen` 命令显示生成的密钥文件的位置。 创建虚拟机时需要此目录名称。
    - 公钥存储在 `ansible_rsa.pub` 中，私钥存储在 `ansible_rsa` 中。

## <a name="create-a-virtual-machine"></a>创建虚拟机

1. 使用 [az group create](/cli/azure/group#az-group-create) 创建资源组。 可能需要将 `--location` 参数替换为你的环境的相应值。

    ```azurecli
    az group create --name QuickstartAnsible-rg --location eastus
    ```

1. 使用 [az vm create](/cli/azure/vm#az-vm-create) 创建虚拟机。

    ```azurecli
    az vm create \
    --resource-group QuickstartAnsible-rg \
    --name QuickstartAnsible-vm \
    --image OpenLogic:CentOS:7.7:latest \
    --admin-username azureuser \
    --ssh-key-values <ssh_public_key_filename>
    ```

1. 使用 [az vm list](/cli/azure/vm#az-vm-list) 验证新虚拟机的创建（和状态）。

    ```azurecli
    az vm list -d -o table --query "[?name=='QuickstartAnsible-vm']"
    ```

    **注释**：

    - `az vm list` 命令的输出包括用于通过 SSH 连接到虚拟机的公共 IP 地址。

## <a name="install-ansible-on-the-virtual-machine"></a>在虚拟机上安装 Ansible

使用 [az vm extension set](/cli/azure/vm/extension?#az-vm-extension-set) 运行 Ansible 安装脚本。

```azurecli
az vm extension set \
 --resource-group QuickstartAnsible-rg \
 --vm-name QuickstartAnsible-vm \
 --name customScript \
 --publisher Microsoft.Azure.Extensions \
 --version 2.1 \
 --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-ansible-control-machine/master/configure-ansible-centos.sh"]}' \
 --protected-settings '{"commandToExecute": "./configure-ansible-centos.sh"}'
```

注意：

- 完成后，`az vm extension` 命令显示运行安装脚本的结果。

## <a name="connect-to-your-virtual-machine-via-ssh"></a>通过 SSH 连接到虚拟机

使用 SSH 命令连接到虚拟机。

```azurecli
ssh -i <ssh_private_key_filename> azureuser@<vm_ip_address>
```

## <a name="create-azure-credentials"></a>创建 Azure 凭据

需要以下信息才能配置 Ansible 凭据：

* Azure 订阅 ID
* 服务主体值

如果使用 Ansible Tower 或 Jenkins，请将服务主体值声明为环境变量。

使用以下方法之一配置 Ansible 凭据：

- [创建 Ansible 凭据文件](#file-credentials)
- [使用 Ansible 环境变量](#env-credentials)

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/>创建 Ansible 凭据文件

在本部分，我们将创建一个本地凭据文件，以便向 Ansible 提供凭据。

有关定义 Ansible 凭据的详细信息，请参阅[为 Azure 模块提供凭据](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules)。

1. 对于开发环境，请主机虚拟机上创建名为 `credentials` 的文件：

    ```bash
    mkdir ~/.azure
    vi ~/.azure/credentials
    ```

1. 将以下代码行插入到该文件中。 请将占位符替换为服务主体值。

    ```bash
    [default]
    subscription_id=<your-subscription_id>
    client_id=<security-principal-appid>
    secret=<security-principal-password>
    tenant=<security-principal-tenant>
    ```

1. 保存并关闭该文件。

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/>使用 Ansible 环境变量

在本部分，我们将导出服务主体值以配置 Ansible 凭据。

1. 打开终端窗口。

1. 导出服务主体值：

    ```bash
    export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
    export AZURE_CLIENT_ID=<security-principal-appid>
    export AZURE_SECRET=<security-principal-password>
    export AZURE_TENANT=<security-principal-tenant>
    ```

## <a name="test-ansible-installation"></a>测试 Ansible 安装

现在你已有一个安装并配置了 Ansible 的虚拟机！

[!INCLUDE [ansible-test-configuration.md](includes/ansible-test-configuration.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [Azure 上的 Ansible](./index.yml)