---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 2dccfba5cfe61ac862aeabe7b928e121502093b7
ms.sourcegitcommit: 367780fe48d977c82cb84208c128b0bf694b1029
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/29/2020
ms.locfileid: "76830875"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>确定是否使用自己的自定义创建的共享 Java EE 库

如果使用共享 Java EE 库功能，则可使用两个选项：

1. 重构应用程序代码以删除库上的所有依赖项，并改将功能直接合并到应用程序中。
2. 将库添加到服务器 classpath。
