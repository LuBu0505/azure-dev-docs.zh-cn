---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: bda7ba230d8287101fc8f3f298b08777bb6e1c61
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897464"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>确定是否使用了 EJB 计时器

如果应用程序使用了 EJB 计时器，则需要验证每个 WildFly 实例是否可以独立触发 EJB 计时器代码。 需要进行此验证是因为在 Azure Kubernetes 服务部署方案中，每个 EJB 计时器都将在其自己的 WildFly 实例上触发。
