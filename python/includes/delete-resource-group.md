---
ms.openlocfilehash: 2f54e918e7126123ada68b696ea96f4d5f3dbb83
ms.sourcegitcommit: a8073315f751631ab983618fa9f812eb95d8b2dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125231"
---
可以使用 [Azure 门户](https://portal.azure.com)或 Azure CLI 来删除资源组：

- 在门户中，请从左侧导航窗格中选择“资源组”，接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。  

- 运行以下 Azure CLI 命令（在本地运行，或使用 [Cloud Shell](/cloud-shell/overview) 来运行），将 `<resource_group>` 替换为本教程中使用的组名称：

    ```azurecli
    az group delete --name <resource_group>
    ```
