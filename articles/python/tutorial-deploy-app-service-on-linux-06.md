---
title: 步骤 6：将日志从 Azure 应用服务传输到 VS Code 中
description: 教程步骤 6，将应用日志流式传输到 Visual Studio Code 中
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 01df1219302f4edf21845c999337354a065352ac
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "95485676"
---
# <a name="6-stream-logs-from-azure-app-service-into-visual-studio-code"></a>6：将日志从 Azure 应用服务流式传输到 Visual Studio Code 中

[上一步：部署应用](tutorial-deploy-app-service-on-linux-05.md)

按照此过程操作可将 Azure 应用服务中的日志流式传输到 Visual Studio Code。

1. 在 Visual Studio Code 中打开“Azure:  应用服务”资源管理器，右键单击应用服务，然后选择“开始流式传输日志”： 

   ![从应用服务资源管理器启动流式传输日志](media/deploy-azure/start-streaming-logs-in-visual-studio-code.png)

   如果系统提示你启用文件日志记录功能并重启 Web 应用，请选择“是”。 当应用重启时，  VS Code 中的“输出”窗口会显示进度。 重启应用后，再次选择“Start streaming logs”命令。 启用日志记录是一次性的操作。

1.  VS Code 中的“输出”窗口会显示“正在启动实时日志流”，日志输出开始显示。 尝试在浏览器中刷新 Web 应用可生成更多日志信息。

1. 若要停止流式传输日志（不禁用日志记录），请在“Azure:  应用服务”资源管理器中右键单击应用，然后选择“停止流式传输日志”。 

> [!div class="nextstepaction"]
> [我看到了日志 - 转到步骤 7 >>>](tutorial-deploy-app-service-on-linux-07.md)

[存在问题？请告诉我们。](https://aka.ms/FlaskVSCQuickstartHelp)
