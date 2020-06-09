---
title: 快速入门 - Ansible 入门 - Azure Cloud Shell
description: 本快速入门介绍如何使用 Azure Cloud Shell 中的 Bash 执行各种 Ansible 任务
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 06/01/2020
ms.openlocfilehash: 2d53c2c5e3eb834e0296be167104e484d144fbd9
ms.sourcegitcommit: 367217792f3b16c769e2c39372358bc6b9c9c044
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84301148"
---
# <a name="quickstart-getting-started-with-ansible---azure-cloud-shell"></a>快速入门：Ansible 入门 - Azure Cloud Shell

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