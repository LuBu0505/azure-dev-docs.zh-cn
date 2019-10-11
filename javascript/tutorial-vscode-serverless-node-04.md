---
title: 从 Visual Studio Code 部署 Azure Functions 应用程序
description: 教程第 4 部分：将 Functions 应用部署到云。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 53d0dd11567084d42de71a0f737cf8b9f5fc5249
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685925"
---
# <a name="deploy-the-functions-app"></a>部署 Functions 应用

[上一步：在本地测试函数](tutorial-vscode-serverless-node-03.md)

1. 在 VS Code 中，选择 Azure 徽标以打开 **Azure Explorer**，然后在“函数”  下，选择蓝色向上箭头以部署应用：

    ![部署到 Azure Functions 命令](media/functions-extension/deploy-app.png)

    或者，可以通过打开“命令面板”  (**F1**)，输入“部署到函数应用”并运行  **Azure Functions 来进行部署：** 部署到函数应用”命令。

1. 在提示“选择 Azure 中的函数应用”  时，然后选择“在 Azure 中创建新的函数应用”  。

1. 出现下一提示时，为函数应用输入一个全局唯一的名称，然后按 **Enter**。 函数应用名称的有效字符包括“a-z”、“0-9”和“-”。

1. 出现下一提示时，选择你附近的 Azure [区域](https://azure.microsoft.com/en-us/regions/)。

1. **Azure Functions** 的 VS Code“输出”  面板显示进度：

    ![显示部署进度的 VS Code 输出面板](media/functions-extension/deploy-progress.png)

1. 部署完成后，请转到“Azure Functions”  资源管理器，展开 Azure 订阅的节点，展开 Functions 应用的节点，然后展开“Functions (只读)”  。 右键单击函数名称并选择“复制函数 URL”  ：

    ![复制函数 URL 命令](media/functions-extension/copy-function-url-command.png)

1. 将 URL 粘贴到浏览器中，并追加 `?name=<yourname>` 参数。 然后，浏览器应显示在云中运行的函数：

    ![在云中运行的函数](media/functions-extension/remote-test-browser.png)

1. 如果需要，请对 index.js  中的函数代码进行一些更改，或使用其他触发器添加其他函数。 在本地测试之后，按照前面的步骤再次部署代码，以在云中测试这些更改。

    > [!TIP]
    > 在部署时，将部署整个 Functions 应用程序，因此对所有单个功能的更改将立即部署。

> [!div class="nextstepaction"]
> [我部署了函数应用](tutorial-vscode-serverless-node-05.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)
