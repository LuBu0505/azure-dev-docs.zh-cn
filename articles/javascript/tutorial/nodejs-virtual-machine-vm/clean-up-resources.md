---
title: 删除 Linux 虚拟机资源
description: 通过使用 Azure CLI 命令删除资源组来清理 Azure 资源。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5c8c0bb8a1413da72cb2c32d9ce541bee1c36cac
ms.sourcegitcommit: dc74b60217abce66fe6cc93923e869e63ac86a8f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94872818"
---
# <a name="7-clean-up-resources"></a>7.清理资源

完成本教程后，需要删除包含所有资源的资源组，以确保不再支付相关使用费用。 

## <a name="remove-all-the-resources-by-removing-resource-group"></a>通过删除资源组来删除所有资源

在同一终端中，使用 [Azure CLI 命令](/cli/azure/group?view=azure-cli-latest#az_group_delete)删除资源组：

```azurecli
az group delete --name rg-demo-vm-eastus -y
```

此命令需要花费几分钟时间。 

## <a name="next-step"></a>后续步骤

* 详细了解 [Azure Linux VM](/azure/virtual-machines)
