---
ms.openlocfilehash: c45bda8b08ec963febbc6497136cda71086423a3
ms.sourcegitcommit: 418e446e6ada5d50df283401df4f6b6370a356b9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96035479"
---
可以使用 [Azure 门户](https://portal.azure.com)或 Azure CLI 来删除资源组：

- 在 [Azure 门户](https://portal.azure.com)中，请从左侧导航窗格中选择“资源组”，接着选择在本教程中创建的资源组，然后使用“Delete resource group”命令 。

- 运行以下 Azure CLI 命令（在本地运行，或使用 [Cloud Shell](/azure/cloud-shell/overview) 来运行），将 `<resource_group>` 替换为本教程中使用的组名称：

    ```azurecli
    az group delete --no-wait --name <resource_group>
    ```
