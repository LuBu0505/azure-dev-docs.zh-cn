---
title: Azure 中的日志记录、指标、遥测
description: 了解 Azure 中的日志记录选项
ms.topic: how-to
ms.date: 10/22/2020
ms.custom: devx-track-js
ms.openlocfilehash: 32abae7005cea4076c000ec92039df4d27a0ec10
ms.sourcegitcommit: 03240a3beece1407a140656d246e11856dc72ac7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92493914"
---
# <a name="logging-metrics-and-telemetry-in-azure"></a>Azure 中的日志记录、指标和遥测 

使用 Azure 时，有多种选项可用于日志记录、指标和遥测。 查看以下选项，找到自己要寻找的工具或服务：

* [资源指标](#resource-metrics-provided-by-azure-services) - 使用 Azure 服务时，Azure 会监视你的单个资源并收集指标。  
* [自定义指标](#custom-logging-to-azure) - 应用程序（本地、云或混合）需要记录信息的情况下。
* [Azure SDK 客户端库](#azure-sdk-client-library-logging) - 需要查看已内置于 Azure 客户端库的日志记录的情况下

## <a name="resource-metrics-provided-by-azure-services"></a>Azure 服务提供的资源指标

[Azure Monitor](/azure/azure-monitor/overview) 提供用于收集、分析和处理来自云与本地环境的遥测数据的综合解决方案，可将应用程序和服务的可用性和性能最大化。

Azure 门户中的资源内提供这些指标。 

:::image type="content" source="../media/logging-metrics/azure-resource-metrics-portal.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::

监视数据后，请使用 Azure 门户通过常用查询来查询数据，生成自己的 [Kusto 查询](/azure/data-explorer/kusto/query/)。 

## <a name="custom-logging-to-azure"></a>自定义记录到 Azure 的方式

使用 Azure Monitor 的 [Application Insights](/azure/azure-monitor/app/app-insights-overview)，后者提供[服务器](/azure/azure-monitor/app/nodejs) (node.js) 和[客户端](/azure/azure-monitor/app/javascript)（浏览器）方案：

* 服务器 - 通过 [Application Insights 从 Node.js 记录](/azure/azure-monitor/app/app-insights-overview) - [npm 包](https://www.npmjs.com/package/applicationinsights)
* 客户端 -从客户端代码记录 - [npm 包](https://www.npmjs.com/package/@microsoft/applicationinsights-web)
* 容器和 VM - 从 [Kubernetes 群集](/azure/azure-monitor/insights/container-insights-overview)或 [Azure 虚拟机记录](/azure/azure-monitor/insights/vminsights-overview)
 
## <a name="azure-sdk-client-library-logging"></a>Azure SDK 客户端库日志记录

通常，你不需要访问内部 Azure SDK 客户端库日志记录。 用于日志记录的 Azure 客户端核心库专供 Azure 服务使用。 

[NPM 包](https://www.npmjs.com/package/@azure/logger) | [库源代码](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/core/logger)

### <a name="enable-logging"></a>启用日志记录

可以使用单个环境变量在整个应用程序中启用日志记录，也可以为应用程序的某个部分动态配置日志记录。 本文介绍有关记录器包的关键概念，以及如何使用以下方法启用日志记录：

- 设置 `AZURE_LOG_LEVEL` 环境变量。
- 调用从记录器库导入的 `setLogLevel`。
- 对特定记录器调用 `enable()`。

> [!NOTE]
> 本文适用于使用最新版本的 Azure SDK 的客户端库。 若要查看库是否受支持，请参阅 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/index.html#javascript)列表。 如果应用程序使用旧版 Azure SDK 客户端库，请参阅适用服务文档中的具体说明。

### <a name="log-levels"></a>日志级别

`@azure/logger` 包支持按最详细到最不详细的顺序指定的以下日志级别：

- verbose
- info
- warning
- error

当你以编程方式或通过 `AZURE_LOG_LEVEL` 环境变量设置日志级别时，任何日志级别等于或小于所选的日志级别的日志将被发出。 例如，如果你将日志级别设置为 `warning`，则级别为 `warning` 或 `error` 的所有日志将被发出。

### <a name="install-the-logger-package"></a>安装记录器包

适用于 JavaScript 的 Azure SDK 记录器库以 [npm](https://www.npmjs.com/) 包的形式提供。 使用 npm 安装 `@azure/logger` 包：

```cmd
npm install @azure/logger
```

### <a name="set-the-logging-environment-variable"></a>设置日志记录环境变量

可使用单个 `AZURE_LOG_LEVEL` 环境变量在应用程序中启用日志记录。 日志将输出到 stderr。 设置该环境变量后，你需重启应用程序以开始生成日志。

此 bash 示例将日志级别设置为 verbose：

```bash
export AZURE_LOG_LEVEL="verbose"
```

本示例使用 PowerShell：

```powershell
$env:AZURE_LOG_LEVEL="verbose"
```

本示例使用 CMD：

```cmd
set AZURE_LOG_LEVEL="verbose"
```

### <a name="configure-dynamic-logging"></a>配置动态日志记录

通过适用于 JavaScript 的 Azure SDK，可根据需要或针对特定客户端库动态地启用日志记录。 这适合于那些想要对某些应用程序组件使用自定义日志实现的开发人员，或者需要临时日志进行调试的开发人员。

可以使用  `@azure/logger`  模块在代码中设置日志级别：

```js
import { setLogLevel } from "@azure/logger";

setLogLevel("verbose");
```

若要启用特定的日志通道，请从要为其发出日志的包导入 `logger`。 以下示例仅为事件中心启用信息日志记录：

```js
import { logger } from "@azure/event-hubs";

logger.info.enable();
```

### <a name="redirect-log-output"></a>重定向日志输出

若要自行处理日志消息，请重新分配 `AzureLogger` 上的 `log` 方法或从客户端库导入的任何记录器。

此示例将日志消息重定向到 stderr：

```js
import { AzureLogger } from "@azure/logger";

AzureLogger.log = msg => console.error(msg);
```

此示例仅重定向来自 Azure 事件中心的信息日志消息：

```js
import { logger } from "@azure/event-hubs";
logger.info.log = msg => console.error(msg);
```

## <a name="next-steps"></a>后续步骤

- [在 Azure 应用服务中为应用启用诊断日志记录](/azure/app-service/troubleshoot-diagnostic-logs)
- [查看 Azure 安全日志记录和审核选项](/azure/security/fundamentals/log-audit)
- [了解如何使用 Azure 平台日志](/azure/azure-monitor/platform/platform-logs-overview)