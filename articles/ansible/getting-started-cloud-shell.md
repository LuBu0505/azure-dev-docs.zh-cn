---
title: 快速入门 - 使用 Azure Cloud Shell 配置 Ansible
description: 本快速入门介绍如何使用 Azure Cloud Shell 中的 Bash 执行各种 Ansible 任务
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 08/31/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: 42d7b57d9890bce3a432fba3c6fdcf3080ddb63c
ms.sourcegitcommit: 2f98cf2a394d4fd82ddc917ac1041c1dc08473b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2020
ms.locfileid: "89275152"
---
# <a name="quickstart-configure-ansible-using-azure-cloud-shell"></a>快速入门：使用 Azure Cloud Shell 配置 Ansible

[!INCLUDE [annsible-intro.md](includes/ansible-intro.md)]

本文介绍了如何在 [Azure Cloud Shell](/azure/cloud-shell/overview) 环境中开始使用 Ansible。

## <a name="configure-your-environment"></a>配置环境

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../includes/open-source-devops-prereqs-azure-subscription.md)]
- **配置 Azure Cloud Shell** - 如果你是 Azure Cloud Shell 的新手，请参阅 [Azure Cloud Shell 中的 Bash 快速入门](https://docs.microsoft.com/azure/cloud-shell/quickstart)。

[!INCLUDE [open-cloud-shell.md](../includes/open-cloud-shell.md)]

## <a name="automatic-credential-configuration"></a>自动凭据配置

登录 Cloud Shell 后，Ansible 将通过 Azure 进行身份验证以管理基础架构，无需任何其他配置。 

使用多个订阅时，请通过导出 `AZURE_SUBSCRIPTION_ID` 环境变量来指定 Ansible 使用的订阅。 

要列出所有 Azure 订阅，请运行以下命令：

```azurecli-interactive
az account list
```

按如下所示使用 Azure 订阅 ID 设置 `AZURE_SUBSCRIPTION_ID`：

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>验证配置
若要验证配置是否成功，请使用 Ansible 创建一个 Azure 资源组。

[!INCLUDE [create-resource-group-with-ansible.md](includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [快速入门：使用 Ansible 在 Azure 中配置虚拟机](./vm-configure.md)
