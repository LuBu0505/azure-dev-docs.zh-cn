---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c8cd0672bfedf640077e9ae3b9272b12d8ce0b61
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738115"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>预配 Azure 容器注册表和 Azure Kubernetes 服务

使用以下命令创建容器注册表和 Azure Kubernetes 群集（带有对注册表具有“读取者”角色的服务主体）。 确保根据群集的网络要求[选择相应的网络模型](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```
