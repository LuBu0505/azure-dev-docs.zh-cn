---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: d56ba6d8f101b1d90468a436f0a111f78a19fc83
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673403"
---
### <a name="determine-whether-weblogic-has-been-customized"></a>确定是否已自定义 WebLogic

确定进行了以下哪些自定义，并捕获已完成的操作。

* 是否更改了启动脚本？ 此类脚本包括 *setDomainEnv*、*commEnv*、*startWebLogic* 和 *stopWebLogic*。
* 是否有任何特定参数传递到 JVM？
* 是否存在添加到服务器 classpath 中的 JAR？
