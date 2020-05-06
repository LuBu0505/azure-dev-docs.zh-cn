---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: 64a4a0e191c70d930ad8c5f9d19cb81dfb9cb851
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81673413"
---
### <a name="determine-whether-you-are-using-your-own-custom-created-shared-java-ee-libraries"></a>确定是否使用自己的自定义创建的共享 Java EE 库

如果使用共享 Java EE 库功能，则可使用两个选项：

* 重构应用程序代码以删除库上的所有依赖项，并改将功能直接合并到应用程序中。
* 将库添加到服务器 classpath。
