---
author: mnriem
ms.author: manriem
ms.date: 2/28/2020
ms.openlocfilehash: e4370e6db3589e6050395fe806541505fa3eb8bb
ms.sourcegitcommit: 850856d3fa2ddd8f96616ee6a1f092d8e0aedab3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738113"
---
### <a name="determine-whether-your-application-relies-on-scheduled-jobs"></a>确定应用程序是否依赖于计划的作业

计划的作业（如 Quartz 计划程序任务或 Unix cron 作业）不应与 Azure Kubernetes 服务配合使用。 Azure Kubernetes 服务不会阻止你部署内含计划任务的应用程序。 但是，如果应用程序横向扩展，则同一个计划的作业可能会按照计划期间运行多次。 这种情况可能会导致意外的后果。

若要在 AKS 群集上执行计划的作业，请按需定义 Kubernetes CronJob。 有关详细信息，请参阅[使用 CronJob 运行自动化任务](https://kubernetes.io/docs/tasks/job/automated-tasks-with-cron-jobs/)。
