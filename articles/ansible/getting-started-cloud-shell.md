---
title: 快速入门 - 使用 Azure Cloud Shell 配置 Ansible
description: 本快速入门介绍如何使用 Azure Cloud Shell 中的 Bash 执行各种 Ansible 任务
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 09/14/2020
ms.custom: devx-track-ansible
ms.openlocfilehash: 0a03794bdcbd810444f42db045650cdad813724c
ms.sourcegitcommit: bfaeacc2fb68f861a9403585d744e51a8f99829c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90682060"
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

## <a name="test-ansible-installation"></a>测试 Ansible 安装

现在，你已将 Ansible 配置为在 Cloud Shell 中使用！

[!INCLUDE [ansible-test-configuration.md](includes/ansible-test-configuration.md)]

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"] 
> [快速入门：使用 Ansible 在 Azure 中配置虚拟机](./vm-configure.md)
