---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 99140db00e7a0a72b96db19b4dab9d7476694ab2
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672923"
---
### <a name="determine-whether-jaas-is-in-use"></a>确定是否使用了 JAAS

如果应用程序使用的是 JAAS，则需要捕获 JAAS 的配置方式。 如果使用的是数据库，则可以将其转换为 WildFly 上的 JAAS 域。 如果它是自定义实现，则需要验证它是否可在 WildFly 上使用。
