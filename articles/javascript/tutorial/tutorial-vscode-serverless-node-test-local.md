---
title: 在 Visual Studio Code 中本地运行 Azure Functions 应用程序
description: 在本地运行 Azure Functions 项目，以在部署到 Azure 之前对其进行测试。 在无服务器函数返回响应之前设置断点。
ms.topic: tutorial
ms.date: 09/23/2019
ms.custom: devx-track-js, contperf-fy21q2
ms.openlocfilehash: f345e27074c2070c2f8d8939ed09f8b4301a1966
ms.sourcegitcommit: c8330128d5d6a71859933a890ecdf047cb950996
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97522345"
---
# <a name="3-test-the-function-locally"></a>3.在本地测试函数

[上一步：创建 Functions 应用](tutorial-vscode-serverless-node-create-local.md)

在此步骤中，将在本地运行 Azure Functions 项目，以在部署到 Azure 之前对其进行测试。 在无服务器函数返回响应之前设置断点。

创建 Functions 应用时，Azure Functions 扩展会自动将 VS Code 启动配置添加到项目中，该配置位于 .vscode/launch.json  文件中。 此配置使用在 Azure 上运行的相同运行时，因此可以在部署到云之前确保源代码正常工作。

## <a name="run-the-local-serverless-function"></a>运行本地无服务器函数

1. 在 Visual Studio Code 中，按 **F5**（或使用“调试”   > “开始调试”  菜单命令）启动调试器并附加到 Azure Functions 主机。 此命令自动使用 Azure Functions 为你创建的调试配置。

1. 函数核心工具的输出出现在 VS Code“终端”  面板中。 启动主机后，在按住 **Alt** 键的同时单击输出中显示的本地 URL 以打开浏览器并运行函数：

    ![本地调试时 VS Code“终端”面板中显示的输出](../media/functions-extension/local-test-output.png)

1. 默认 HTTP 触发器模板创建的代码分析 `name` 查询参数以自定义响应。 在浏览器中，将 `?name=<yourname>` 添加到浏览器中的 URL 以正确查看响应输出：

    ![分析 URL 参数的 HTTP 触发器函数](../media/functions-extension/local-test-browser.png)

## <a name="set-and-stop-at-break-point-in-serverless-app"></a>在无服务器应用中设置断点并在断点处停止

1. 当函数在本地运行时，可以在代码的不同部分设置断点。 打开 *index.js*，然后单击编辑器窗口中第 11 行左侧的空白。 出现一个小红点表示断点。 现在，在浏览器中删除 URL 中的 `?name=` 参数。 当浏览器发出该请求时，VS Code 停止该断点上的函数代码：

    ![在断点处停止的 VS Code](../media/functions-extension/debugging-breakpoint.png)

    若要了解有关 VS Code 中的断点和调试的详细信息，请参阅[调试](https://code.visualstudio.com/docs/editor/debugging)。

> [!Note]
>
> 如果在此过程中遇到执行策略错误，请尝试使用 npm 来卸载 `azure-functions-core-tools@3`，然后通过提升的权限在终端中来重新安装该包。

> [!div class="nextstepaction"]
> [我在本地运行了 Functions 应用](tutorial-vscode-serverless-node-deploy-hosting.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=run-app)
