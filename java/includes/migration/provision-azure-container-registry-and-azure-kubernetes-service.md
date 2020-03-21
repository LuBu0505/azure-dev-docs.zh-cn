---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: e72cefaab3ccdbbaae01992cc11285944fb09a36
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897413"
---
### <a name="provision-azure-container-registry-and-azure-kubernetes-service"></a>预配 Azure 容器注册表和 Azure Kubernetes 服务

使用以下命令创建容器注册表和 Azure Kubernetes 群集（带有对注册表具有“读取者”角色的服务主体）。 确保根据群集的网络要求[选择相应的网络模型](/azure/aks/operator-best-practices-network#choose-the-appropriate-network-model)。

```bash
az group create -g $resourceGroup -l eastus
az acr create -g $resourceGroup -n $acrName --sku Standard
az aks create -g $resourceGroup -n $aksName --attach-acr $acrName --network-plugin azure
```