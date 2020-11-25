---
title: 创建 Azure Monitor 资源
description: 为所有 Azure 资源创建一个 Azure 资源组，并创建一个 Monitor 资源以将 Web 应用的日志文件收集到 Azure 云。 Azure Monitor 是 Azure 服务的名称，而 Application Insights 是本教程使用的客户端库的名称。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: b401953c0e1c972efa0f5d90817f461b858cf04b
ms.sourcegitcommit: a2a51e0c6530eb5794a2fe667cf4c9a60b2a7470
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/13/2020
ms.locfileid: "94625036"
---
# <a name="2-create-application-insights-resource-for-web-pages"></a>2.为网页创建 Application Insights 资源

在本教程的此步骤中，为所有 Azure 资源创建一个 Azure 资源组，并创建一个 Monitor 资源以将 Web 应用的日志文件收集到 Azure 云。 Azure Monitor 是 Azure 服务的名称，而 Application Insights 是本教程使用的客户端库的名称。 

## <a name="create-a-resource-group-for-your-virtual-machine-resources"></a>为虚拟机资源创建资源组

本教程包含多个 Azure 资源。 创建资源组使你能够轻松查找资源，并可在完成查找后将其删除。

1. 在终端或 bash shell 中输入 [Azure CLI 命令以创建 Azure 资源组](/cli/azure/group?view=azure-cli-latest#az_group_create)，并将其命名为 `rg-demo-vm-eastus`：

    ```azurecli
    az group create \
        --location eastus \
        --name rg-demo-vm-eastus 
    ```

## <a name="create-azure-monitor-resource-with-azure-cli"></a>使用 Azure CLI 创建 Azure Monitor 资源

1. 向 Azure CLI 安装 Application Insights 扩展。

    ```azurecli
    az extension add -n application-insights
    ```

1. 使用以下命令[创建监视资源](/cli/azure/ext/application-insights/monitor/app-insights/component?view=azure-cli-latest#ext_application_insights_az_monitor_app_insights_component_create)：


    ```azurecli
    az monitor app-insights component create \
      --app demoWebAppMonitor \
      --location eastus \
      --resource-group rg-demo-vm-eastus
    ```

    在结果中，查找并复制 `instrumentationKey`。 之后需要此 ID。 

1. 使终端保持打开状态，你将在下一步中使用它。

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [创建 Linux 虚拟机](create-linux-virtual-machine-azure-cli.md) 
