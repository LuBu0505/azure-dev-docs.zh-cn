---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 99140db00e7a0a72b96db19b4dab9d7476694ab2
ms.sourcegitcommit: 56e5f51daf6f671f7b6e84d4c6512473b35d31d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/07/2020
ms.locfileid: "78897515"
---
### <a name="determine-whether-jaas-is-in-use"></a>确定是否使用了 JAAS

如果应用程序使用的是 JAAS，则需要捕获 JAAS 的配置方式。 如果使用的是数据库，则可以将其转换为 WildFly 上的 JAAS 域。 如果它是自定义实现，则需要验证它是否可在 WildFly 上使用。
