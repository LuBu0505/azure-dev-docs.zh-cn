---
title: 在 Azure 门户中查看日志
description: 了解如何使用 Azure Monitor 和 Application Insights 查看日志记录。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: fd0741cd58e8d2e882a352035cd3c4f9bca956d1
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625066"
---
# <a name="6-view-logs-in-azure-portal"></a>6.在 Azure 门户中查看日志

在本教程的此部分，了解如何使用 Azure Monitor 和 Application Insights 查看日志记录。 

## <a name="count-of-traces-in-logs-with-azure-cli"></a>使用 Azure CLI 查看日志中的跟踪计数

使用 Azure CLI 快速查看日志的重要部分。 例如，使用以下命令查看日志中有多少个跟踪。 

请记住，跟踪仅添加到 `/trace` 路由中。 对 Web 应用的根目录的调用不会生成任何跟踪日志。 

```azurecli
az monitor app-insights metrics show \
    --resource-group rg-demo-vm-eastus \
    --app demoWebAppMonitor \
    --metric traces/count
```

响应如下所示，此示例共有 2 个跟踪： 

```console
{
  "value": {
    "end": "2020-11-11T21:33:40.311000+00:00",
    "interval": null,
    "segments": null,
    "start": "2020-11-11T20:33:40.311000+00:00",
    "traces/count": {
      "sum": 2
    }
  }
}
```

## <a name="view-application-traces-in-azure-portal"></a>在 Azure 门户中查看应用程序跟踪

若要以列表方式查看跟踪，最简单的方法是使用 Azure 门户。 

1. 在 Web 浏览器中打开 [Azure 门户](https://ms.portal.azure.com/#blade/HubsExtension/BrowseAll)。
1. 按资源组 `rg-demo-vm-eastus` 筛选资源列表。 
1. 选择 `demoWebAppMonitor` 资源。 
1. 选择“监视”部分的“日志”项。  如果弹出了一个显示可选择的查询的窗口，请选择角落上的“X”将其关闭。
1. 通过双击选择名为“跟踪”的 Application Insights 项 。 这会将该名称添加到查询窗口。 
1. 选择“运行”按钮以运行查询。
1. 随即显示来自 Web 应用的 Azure Monitor application insights 自定义跟踪的列表。

    :::image type="content" source="../../media/tutorial-vm/azure-portal-application-insights-custom-trace.png" alt-text="显示来自 Web 应用的 Azure Monitor application insights 自定义跟踪的列表。":::

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [清理 Azure 资源](clean-up-resources.md) 