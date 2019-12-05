---
title: 将日志从 Azure 应用服务流式传输到 Visual Studio Code 中
description: 教程第 4 部分：查看或跟踪日志。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: cc140d7751f9b014f1a16065fd4c65b481c7d1ae
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466815"
---
# <a name="stream-logs-from-azure-app-service"></a>从 Azure 应用服务流式传输日志

[上一步：部署网站](tutorial-vscode-azure-app-service-node-03.md)

在此步骤中，你将了解如何通过调用 `console.log` 来查看或“跟踪”正在运行的网站生成的任何输出。 此输出显示在 Visual Studio Code 的“输出”  窗口中。

1.  在“Azure 应用服务”资源管理器中右键单击应用节点，然后选择“开始流式传输日志”。 

    ![查看流日志](media/deploy-azure/view-logs.png)

1. 出现提示时，请选择启用日志记录并重启应用程序。

    ![提示启用日志记录并重启](media/deploy-azure/enable-restart.png)

1. 重启应用后，随着与日志流建立连接，VS Code 的“输出”  窗口将会打开，其中显示输出。

    ```bash
    Connecting to log-streaming service...
    2019-09-20 17:33:51.428 INFO  - Container msdocs-vscode-node_2 for site msdocs-vscode-node initialized successfully.
    2019-09-20 17:33:56.500 INFO  - Container logs
    ```

1. 在浏览器中刷新网页几次以查看更多日志输出。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-vscode-azure-app-service-node-05.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=tailing-logs)
