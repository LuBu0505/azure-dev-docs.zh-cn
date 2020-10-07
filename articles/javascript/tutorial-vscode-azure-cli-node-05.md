---
title: 从 Azure 应用服务流式传输日志
description: Azure CLI 教程第 5 部分：查看日志
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js
ms.openlocfilehash: 0cb28cc7bafd7d0d713fc980c72a7a41474d5060
ms.sourcegitcommit: 4dd392ea864be52421d0239e59198bc44b0a5a16
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/25/2020
ms.locfileid: "91365140"
---
# <a name="stream-logs-from-app-service"></a>从应用服务流式传输日志

[上一步：部署应用](tutorial-vscode-azure-cli-node-04.md)

在此步骤中，你将查看（或“跟踪”）来自正在运行的应用服务的日志。 在站点代码中对 `console.log` 的任何调用都将显示在终端中。

1. 将 `<your_app_name>` 替换为应用服务的名称，运行以下命令以启动日志记录：

    ```azurecli
    az webapp log tail --name <your_app_name>
    ```

1. 几秒钟后，输出中应出现一条消息，指示你已连接到日志流服务。

    <pre>
    2019-09-25T13:39:23  Welcome, you are now connected to log-streaming service. The default timeout is 2 hours. Change the timeout with the App Setting SCM_LOGSTREAM_TIMEOUT (in seconds).
    </pre>

1. 在浏览器中刷新页面几次以生成更多输出：

    <pre>
    GET / 304 2.327 ms - -
    GET / 304 0.957 ms - -
    GET / 304 2.435 ms - -
    </pre>

1. 按 **Ctrl**+**C** 结束日志记录会话。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-vscode-azure-cli-node-06.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=tailing-logs)
