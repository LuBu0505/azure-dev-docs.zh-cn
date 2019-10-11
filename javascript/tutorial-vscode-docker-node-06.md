---
title: 将 Node.js 应用的容器化日志流式传输到 Visual Studio Code 中
description: 教程第 5 部分：将日志流式传输到 Visual Studio Code 中
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 9df23cb6aac013006cf0f21871f16eededdcb816
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685985"
---
# <a name="stream-logs-into-visual-studio-code"></a>将日志流式传输到 Visual Studio Code

[上一步：进行更改并重新部署](tutorial-vscode-docker-node-05.md)

在此步骤中，你将了解如何通过调用 `console.log` 来查看或“跟踪”正在运行的网站生成的任何输出。 此输出显示在 Visual Studio Code 的“输出”  窗口中。

1.  在“Azure 应用服务”资源管理器中右键单击应用节点，然后选择“开始流式传输日志”。 

    ![查看流日志](media/deploy-containers/stream-logs-command.png)

1. 出现提示时，请选择启用日志记录并重启应用程序。

    ![提示启用日志记录并重启](media/deploy-azure/enable-restart.png)

1. 重新启动应用后，随着与日志流建立连接，Visual Studio Code 中的  “输出”面板将打开，其中开头为消息 `Starting Live Log Stream`。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-vscode-docker-node-07.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
