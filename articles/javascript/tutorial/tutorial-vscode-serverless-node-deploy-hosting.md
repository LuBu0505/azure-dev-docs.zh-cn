---
title: 将 Functions 应用部署到 Azure 云
description: 使用适用于 Azure Functions 的 Visual Studio Code 扩展，将 Functions 应用部署到 Azure 云。 确认 Functions 应用可通过浏览器公开提供。
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js, contperfq2
ms.openlocfilehash: 4d000d1f8f8b8cf05f6387fb698d4bf2e2e9cdc5
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94338466"
---
# <a name="4-deploy-the-functions-app-to-azure-cloud"></a>4.将 Functions 应用部署到 Azure 云

[上一步：在本地测试函数](tutorial-vscode-serverless-node-test-local.md)

在此步骤中，将使用适用于 Azure Functions 的 Visual Studio Code 扩展，将 Functions 应用部署到 Azure 云。 确认 Functions 应用可通过浏览器公开提供。 

## <a name="use-visual-studio-code-extension-to-deploy-to-hosting-environment"></a>使用 Visual Studio Code 扩展来部署到托管环境

1. 在 VS Code 中，选择 Azure 徽标以打开 **Azure Explorer**，然后在“函数”  下，选择蓝色向上箭头以部署应用：

    ![部署到 Azure Functions 命令](../media/functions-extension/deploy-app.png)

    或者，可以通过打开“命令面板”  (**F1**)，输入“部署到函数应用”并运行  **Azure Functions 来进行部署：** 部署到函数应用”命令。

1. 在提示“选择 Azure 中的函数应用”  时，然后选择“在 Azure 中创建新的函数应用”  。

1. 出现下一提示时，为函数应用输入一个全局唯一的名称，然后按 **Enter**。 函数应用名称的有效字符包括“a-z”、“0-9”和“-”。

1. 选择 Node.js 版本/运行时

    ![显示 Node.js 版本/运行时的 VS Code 输出面板](../media/functions-extension/nodejs-runtime-version.png)

1. 出现下一提示时，选择你附近的 Azure [区域](https://azure.microsoft.com/regions/)。

1. **Azure Functions** 的 VS Code“输出”  面板显示进度：

    ![显示部署进度的 VS Code 输出面板](../media/functions-extension/deploy-progress.png)

## <a name="verify-functions-app-is-publicly-available-with-browser"></a>确认 Functions 应用可通过浏览器公开提供

1. 部署完成后，请转到“Azure Functions”  资源管理器，展开 Azure 订阅的节点，展开 Functions 应用的节点，然后展开“Functions (只读)”  。 右键单击函数名称并选择“复制函数 URL”  ：

    ![复制函数 URL 命令](../media/functions-extension/copy-function-url-command.png)

1. 将 URL 粘贴到浏览器中，并追加 `?name=<yourname>` 参数。 然后，浏览器应显示在云中运行的函数：

    ![在云中运行的函数](../media/functions-extension/remote-test-browser.png)

1. 如果需要，请对 index.js  中的函数代码进行一些更改，或使用其他触发器添加其他函数。 在本地测试之后，按照前面的步骤再次部署代码，以在云中测试这些更改。

    > [!TIP]
    > 在部署时，整个 Functions 应用程序将进行部署，因此对所有单个函数的更改将立即部署。

> [!div class="nextstepaction"]
> [我部署了函数应用](tutorial-vscode-serverless-node-remove-resource.md) [我遇到一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=deploy-app)
