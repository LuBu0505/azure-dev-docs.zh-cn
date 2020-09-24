---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: c77a5ae60807cf25415ab86dab9d9891abab13f9
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738458"
---
### <a name="determine-whether-ejb-timers-are-in-use"></a>确定是否使用了 EJB 计时器

如果应用程序使用了 EJB 计时器，则需要验证每个 WildFly 实例是否可以独立触发 EJB 计时器代码。 需要进行此验证是因为在 Azure Kubernetes 服务部署方案中，每个 EJB 计时器都将在其自己的 WildFly 实例上触发。
