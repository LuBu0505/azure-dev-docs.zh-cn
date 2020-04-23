---
title: 使用 Azure CLI 管理 Azure 订阅
description: 使用 Azure CLI 管理 Azure 订阅。
author: dbradish-microsoft
ms.author: dbradish
manager: barbkess
ms.date: 09/09/2018
ms.topic: conceptual
ms.service: azure-cli
ms.devlang: azurecli
ms.openlocfilehash: dad217dff159baa39bd1361258fb308eea872564
ms.sourcegitcommit: 36e02e96b955ed0531f98b9c0f623f4acb508661
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2020
ms.locfileid: "82030899"
---
# <a name="use-multiple-azure-subscriptions"></a>使用多个 Azure 订阅

大多数 Azure 用户永远只有一个订阅。 但是，如果你在多家组织中工作，或者所在的组织已根据不同的组划分了对特定的资源的访问权限，则你在 Azure 中可能就有多个订阅。 CLI 支持全局选择和按命令选择订阅。

有关订阅、计费和成本管理的详细信息，请参阅[计费和成本管理文档](/azure/billing/)。

## <a name="tenants-users-and-subscriptions"></a>租户、用户和订阅

在一定程度上，我们可能会对 Azure 中租户、用户和订阅感到混淆，不理解它们的差别。 租户是包含整个组织的 Azure Active Directory 实体。  此租户至少包含一个订阅和用户。   用户是只与一个租户（即所属的组织）关联的个人。 用户是登录到 Azure 以创建、管理和使用资源的帐户。
用户可能有权访问多个订阅，这些订阅是与 Microsoft 签署的有关使用云服务（包括 Azure）的协议。  每个资源与某个订阅关联。

若要详细了解租户、用户与订阅之间的差别，请参阅 [Azure 云术语字典](/azure/azure-glossary-cloud-terminology)。  若要了解如何将新订阅添加到 Azure Active Directory 租户，请参阅[如何将 Azure 订阅添加到 Azure Active Directory](/azure/active-directory/active-directory-how-subscriptions-associated-directory)。
若要了解如何登录到特定租户，请参阅[使用 Azure CLI 登录](/cli/azure/authenticate-azure-cli)。

## <a name="change-the-active-subscription"></a>更改活动订阅

若要访问订阅的资源，请切换活动订阅或使用 `--subscription` 参数。 可以使用 [az account set](/cli/azure/account#az-account-set) 来切换用于所有命令的订阅。

切换活动订阅：

1. 使用 [az account list](/cli/azure/account#az-account-list) 命令获取订阅列表。

    ```azurecli-interactive
    az account list --output table
    ```
2. 结合你要切换到的订阅 ID 或名称使用 `az account set`。

    ```azurecli-interactive
    az account set --subscription "My Demos"
    ```

如果只是结合不同的订阅运行单个命令，请使用 `--subscription` 参数。 此参数使用订阅 ID 或订阅名称：

```azurecli-interactive
az vm create --subscription "My Demos" --resource-group MyGroup --name NewVM --image Ubuntu
```
