---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 6f35e42736d538c8bb755a84cd0b082052645b6e
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738453"
---
### <a name="determine-whether-jaas-is-in-use"></a>确定是否使用了 JAAS

如果应用程序使用的是 JAAS，则需要捕获 JAAS 的配置方式。 如果使用的是数据库，则可以将其转换为 WildFly 上的 JAAS 域。 如果它是自定义实现，则需要验证它是否可在 WildFly 上使用。
