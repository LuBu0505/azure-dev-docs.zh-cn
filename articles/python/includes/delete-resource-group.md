---
ms.openlocfilehash: 8f757c030849cb89eea36d74b55867dcc2ff98a6
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "80441352"
---
可以使用 [Azure 门户](https://portal.azure.com)或 Azure CLI 来删除资源组：

- 在门户中，请从左侧导航窗格中选择“资源组”，接着选择在本教程中创建的资源组，然后使用“删除资源组”命令。  

- 运行以下 Azure CLI 命令（在本地运行，或使用 [Cloud Shell](/azure/cloud-shell/overview) 来运行），将 `<resource_group>` 替换为本教程中使用的组名称：

    ```azurecli
    az group delete --name <resource_group>
    ```
