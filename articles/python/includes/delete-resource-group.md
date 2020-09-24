---
ms.openlocfilehash: 4215099ae39963448b7a94d389ded0c9096b1c67
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90772671"
---
可以使用 [Azure 门户](https://portal.azure.com)或 Azure CLI 来删除资源组：

- 在门户中，请从左侧导航窗格中选择“资源组”，接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。********

- 运行以下 Azure CLI 命令（在本地运行，或使用 [Cloud Shell](/azure/cloud-shell/overview) 来运行），将 `<resource_group>` 替换为本教程中使用的组名称：

    ```azurecli
    az group delete --no-wait --name <resource_group>
    ```
