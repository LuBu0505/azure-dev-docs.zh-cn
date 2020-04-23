---
author: mriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: 4d2df28d5c752d20454c28a6d4a53698aa7ff5b6
ms.sourcegitcommit: 0af39ee9ff27c37ceeeb28ea9d51e32995989591
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2020
ms.locfileid: "81673013"
---
### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>确定应用程序是否依赖于计划的作业

计划的作业（如 Quartz 计划程序任务或 Unix cron 作业）不应与 Azure Kubernetes 服务配合使用。 Azure Kubernetes 服务不会阻止你部署内含计划任务的应用程序。 但是，如果应用程序横向扩展，则同一个计划的作业可能会按照计划期间运行多次。 这种情况可能会导致意外的后果。

若要在 AKS 群集上执行计划的作业，请按需定义 Kubernetes CronJob。 有关详细信息，请参阅[使用 CronJob 运行自动化任务](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)。
