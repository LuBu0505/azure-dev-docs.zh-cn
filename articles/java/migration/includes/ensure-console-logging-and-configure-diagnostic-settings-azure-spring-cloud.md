---
author: yevster
ms.author: yebronsh
ms.date: 4/15/2020
ms.openlocfilehash: 3ff76d977c231b4b238f9dbec7f3bfaddfe5e484
ms.sourcegitcommit: 81577378a4c570ced1e9c6765f4a9eee8453c889
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84507591"
---
### <a name="ensure-console-logging-and-configure-diagnostic-settings"></a>确保控制台日志记录并配置诊断设置

配置日志记录，使所有输出都路由到控制台而不是文件。

将应用程序部署到 Azure Spring Cloud 后，[添加诊断设置](/azure/spring-cloud/diagnostic-services)以使记录的事件可供使用，例如通过 Azure Monitor 日志分析。

#### <a name="logstashelk-stack"></a>LogStash/ELK 堆栈

如果使用 LogStash/ELK 堆栈进行日志聚合，请将诊断设置配置为将控制台输出流式传输到 [Azure 事件中心](/azure/event-hubs/)。 然后，使用 [LogStash EventHub 插件](https://github.com/logstash-plugins/logstash-input-azure_event_hubs)将记录的事件引入到 LogStash 中。

#### <a name="splunk"></a>Splunk

如果使用 Splunk 进行日志聚合，请将诊断设置配置为将控制台输出流式传输到 [Azure Blob 存储](/azure/storage/blobs/)。 然后，使用 [Microsoft 云服务的 Splunk 附加产品](https://splunkbase.splunk.com/app/3757/)将记录的事件引入到 Splunk 中。
