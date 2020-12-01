---
title: 删除 Azure 资源
description: 通过使用 Azure CLI 命令删除资源组来清理计费资源。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 0b425ce873c53ca95628cfe8c3f68ea4b010aba3
ms.sourcegitcommit: 4dac39849ba2e48034ecc91ef578d11aab796e58
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94993417"
---
# <a name="6-clean-up-resources"></a>6.清理资源

完成本教程后，需要删除资源组，其中包括计算机视觉资源和静态 Web 应用，确保不再支付相关使用费用。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>通过删除资源组来删除所有资源

在同一终端中，使用 [Azure CLI 命令](/cli/azure/group?view=azure-cli-latest#az_group_delete)删除资源组：

```azurecli
az group delete --name rg-demo  -y
```

此命令可能需要花费几分钟时间。 
