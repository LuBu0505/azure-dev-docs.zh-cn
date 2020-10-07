---
title: 将日志从 Azure 应用服务流式传输到 Visual Studio Code 中
description: Node.js 教程第 4 部分：查看或跟踪日志。
ms.topic: tutorial
ms.date: 03/04/2020
ms.custom: devx-track-js
ms.openlocfilehash: eed2663f32571dc40fb402553424c1303d43f7b9
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365230"
---
# <a name="stream-logs-from-azure-app-service"></a>从 Azure 应用服务流式传输日志

[上一步：部署网站](tutorial-vscode-azure-app-service-node-03.md)

此步骤介绍如何通过调用 `console.log` 来查看或“跟踪”正在运行的应用生成的任何输出。 此输出显示在 Visual Studio Code 的“输出”  窗口中。

1.  在“Azure 应用服务”资源管理器中右键单击应用节点，然后选择“开始流式传输日志”。 

    ![查看流日志](media/deploy-azure/start-streaming-logs.png)

1. 出现提示时，请选择启用日志记录并重启应用程序。

    ![提示启用日志记录并重启](media/deploy-azure/enable-restart.png)

1. 重启应用后，随着与日志流建立连接，VS Code 的“输出”  窗口将会打开，其中显示输出。

    <pre>
    Connecting to log stream...
    2020-03-04T19:29:44  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours.
    Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. 在浏览器中刷新网页几次以查看更多日志输出。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-vscode-azure-app-service-node-05.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
