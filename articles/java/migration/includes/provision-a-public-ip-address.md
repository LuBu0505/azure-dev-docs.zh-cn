---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 92caae7f50ddb0e58f14e8c99a4cd598a72f884f
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738114"
---
### <a name="provision-a-public-ip-address"></a>预配公共 IP 地址

如果想要从内部网络或虚拟网络之外访问应用程序，则需要公共静态 IP 地址。 应在群集的节点资源组内预配此 IP 地址，如以下示例中所示：

```bash
nodeResourceGroup=$(az aks show -g $resourceGroup -n $aksName --query 'nodeResourceGroup' -o tsv)
publicIp=$(az network public-ip create -g $nodeResourceGroup -n applicationIp --sku Standard --allocation-method Static --query 'publicIp.ipAddress' -o tsv)
echo "Your public IP address is ${publicIp}."
```
