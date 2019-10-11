---
title: 在 Visual Studio Code 中本地运行 Azure Functions 应用程序
description: 教程第 3 部分，在本地运行应用进行测试。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 0a2b74d754e8a55afb88f6ef628c5da466e7b2df
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686026"
---
# <a name="test-the-function-locally"></a>在本地测试函数

[上一步：创建 Functions 应用](tutorial-vscode-serverless-node-02.md)

在此步骤中，将在本地运行 Azure Functions 项目，以在部署到 Azure 之前对其进行测试。

创建 Functions 应用时，Azure Functions 扩展会自动将 VS Code 启动配置添加到项目中，该配置位于 .vscode/launch.json  文件中。 此配置使用在 Azure 上运行的相同运行时，因此可以在部署到云之前确保源代码正常工作。

1. 在 Visual Studio Code 中，按 **F5**（或使用“调试”   > “开始调试”  菜单命令）启动调试器并附加到 Azure Functions 主机。 （此命令自动使用 Azure Functions 创建的单个调试配置。）

1. 函数核心工具的输出出现在 VS Code“终端”  面板中。 启动主机后，**Ctrl**+click 输出中显示的本地 URL 以打开浏览器并运行函数：

    ![本地调试时 VS Code“终端”面板中显示的输出](media/functions-extension/local-test-output.png)

1. 默认 HTTP 触发器模板创建的代码分析 `name` 查询参数以自定义响应。 在浏览器中，将 `?name=<yourname>` 添加到浏览器中的 URL 以正确查看响应输出：

    ![分析 URL 参数的 HTTP 触发器函数](media/functions-extension/local-test-browser.png)

1. 当函数在本地运行时，可以在代码的不同部分设置断点。 （若要了解有关 VS Code 中的断点和调试的详细信息，请参阅“调试”。）打开 *index.js*，然后单击编辑器窗口中第 11 行左侧的空白。 出现一个小红点表示断点。 现在，在浏览器中删除 URL 中的 `?name=` 参数。 当浏览器发出该请求时，VS Code 停止该断点上的函数代码：

    ![在断点处停止的 VS Code](media/functions-extension/debugging-breakpoint.png)

> [!div class="nextstepaction"]
> [我在本地运行了函数应用](tutorial-vscode-serverless-node-04.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)