---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: abf3824dd83d0da27a411694e144a8e6bf7285ac
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738450"
---
### <a name="deploy-to-aks"></a>部署到 AKS

创建并应用 Kubernetes YAML 文件。 有关详细信息，请参阅[快速入门：使用 Azure CLI 部署 Azure Kubernetes 服务群集](/azure/aks/kubernetes-walkthrough#run-the-application)。 如果要创建外部负载均衡器（不管是针对应用程序，还是针对入口控制器），请确保提供在上一部分以 `LoadBalancerIP` 形式预配的 IP 地址。

包括环境变量形式的外部化参数。 有关详细信息，请参阅[为容器定义环境变量](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/)。 不要包括机密（如密码、API 密钥和 JDBC 连接字符串）。 这些在下一部分中有所介绍。

创建部署 YAML 时，请确保包含内存和 CPU 设置，以便正确调整容器的大小。
