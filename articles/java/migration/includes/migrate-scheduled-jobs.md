---
author: yevster
ms.author: yebronsh
ms.date: 1/22/2020
ms.openlocfilehash: 1015c179c0f93485decd77bd89a3ceec8833652e
ms.sourcegitcommit: be67ceba91727da014879d16bbbbc19756ee22e2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/05/2020
ms.locfileid: "81672153"
---
### <a name="migrate-scheduled-jobs"></a>迁移计划的作业

若要在 Azure 上执行计划的作业，请考虑使用 [Azure Functions 的计时器触发器](/azure/azure-functions/functions-bindings-timer)。 无需将作业代码本身迁移到函数中。 函数可以直接在应用程序中调用 URL 来触发作业。 如果必须动态调用和/或集中跟踪此类作业执行情况，请考虑使用 [Spring Batch](https://spring.io/projects/spring-batch)。

也可使用定期触发器来创建逻辑应用，以便调用 URL，而无需在应用程序外编写任何代码。 有关详细信息，请参阅[概述 - 什么是 Azure 逻辑应用？](/azure/logic-apps/logic-apps-overview)和[通过 Azure 逻辑应用中的定期触发器来创建、计划和运行重复执行的任务和工作流](/azure/connectors/connectors-native-recurrence)。

> [!NOTE]
> 若要防止恶意使用，可能需确保作业调用终结点要求使用凭据。 在这种情况下，触发器函数需要提供凭据。
