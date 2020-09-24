---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 9fa87bf3d0de64271a9ffb0e7e8fbff3cd1afd12
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738451"
---
### <a name="determine-whether-your-application-uses-a-resource-adapter"></a>确定应用程序是否使用资源适配器

如果应用程序需要资源适配器 (RA)，则它需要与 WildFly 兼容。 若要确定此 RA 在独立的 WildFly 实例上是否正常工作，需将其部署到服务器并对其进行正确配置。 如果可以正常使用 RA，则需将这些 JAR 添加到 Docker 映像的服务器 classpath 中，并将所需的配置文件放在 WildFly 服务器目录中的正确位置，使其可用。
