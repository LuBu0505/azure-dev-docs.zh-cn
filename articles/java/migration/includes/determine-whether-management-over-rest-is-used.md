---
author: edburns
ms.author: edburns
ms.date: 1/21/2020
ms.openlocfilehash: a67bdb749231c39e1b64d7b2a982ad4c0946cc25
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673443"
---
### <a name="determine-whether-management-over-rest-is-used"></a>确定是否使用了基于 REST 的管理

如果应用程序的生命周期包括使用基于 REST 的管理，则需捕获用于访问 REST API 的具体端口，并确定如何对其进行身份验证以及如何将其公开。 迁移后，需确保这些相同的端口和身份验证机制是公开的，这样应用程序生命周期就能以类似于迁移之前的方式运行。 有关详细信息，请参阅[使用 RESTful Management Services 管理 Oracle WebLogic Server](https://docs.oracle.com/middleware/12213/wls/WLRUR/title.htm)。
