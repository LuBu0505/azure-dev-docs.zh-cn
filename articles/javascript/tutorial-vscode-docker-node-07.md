---
title: 将 Node.js 应用的容器化日志流式传输到 Visual Studio Code 中
description: 教程第 7 部分：将日志流式传输到 Visual Studio Code 中
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: ad62a7551013f29d9affad9272b20aad2b1a5dd8
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88218378"
---
# <a name="stream-logs-into-visual-studio-code"></a>将日志流式传输到 Visual Studio Code

[上一步：进行更改并重新部署](tutorial-vscode-docker-node-06.md)

在此步骤中，你将了解如何通过调用 `console.log` 来查看或“跟踪”正在运行的网站生成的任何输出。 此输出显示在 Visual Studio Code 的“输出”  窗口中。

1.  在“Azure 应用服务”资源管理器中右键单击应用节点，然后选择“开始流式传输日志”。 

    ![查看流日志](media/deploy-containers/stream-logs-command.png)

1. 出现提示时，请选择启用日志记录并重启应用程序。

    ![提示启用日志记录并重启](media/deploy-azure/enable-restart.png)

1. 重新启动应用后，随着与日志流建立连接，Visual Studio Code 中的  “输出”面板将打开，其中开头为消息 `Starting Live Log Stream`。

> [!div class="nextstepaction"]
> [我看到了日志](tutorial-vscode-docker-node-08.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-docker-extension&step=tailing-logs)
