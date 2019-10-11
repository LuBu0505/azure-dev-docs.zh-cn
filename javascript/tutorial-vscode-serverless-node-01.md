---
title: 在 Visual Studio Code 中使用 Node.js 部署 Azure Functions
description: 教程第 1 部分：简介和先决条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: kraigb
ms.openlocfilehash: 5dd41d710df629565699cff3bfd8e4bca7677087
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686093"
---
# <a name="deploy-azure-functions-from-visual-studio-code"></a>在 Visual Studio Code 中部署 Azure Functions

在本教程中，将使用 Visual Studio Code 和 Azure Functions 扩展来创建和部署使用 JavaScript 编写的 Azure Functions 应用程序。 

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure Functions 扩展](vscode:extension/ms-azuretools.vscode-azurefunctions)
- [Node.js 和 npm](https://nodejs.org/en/download)，Node.js 包管理器。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azurefunctions">安装 Azure Functions 扩展</a>

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/en-us/free/?utm_source=campaign&utm_campaign=vscode-tutorial-functions-extension&mktingSource=vscode-tutorial-functions-extension)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="install-the-azure-functions-core-tools"></a>安装 Azure Functions Core Tools

若要启用本地调试，需要安装 [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools)，这可以直接在 Visual Studio Code 中完成。

1. 启动 Visual Studio Code。

1. 打开命令面板  (**F1**)，输入 **Azure Functions:Install or Update Azure Functions Core Tools**，并按 **Enter** 运行该命令。

1. 若要验证安装，请在 VS Code 中选择菜单命令“终端”   >   “新终端”，然后运行命令 `func`。 该命令应显示如下所示的输出（以及使用情况信息）。

    ```output
                      %%%%%%
                     %%%%%%
                @   %%%%%%    @
              @@   %%%%%%      @@
           @@@    %%%%%%%%%%%    @@@
         @@      %%%%%%%%%%        @@
           @@         %%%%       @@
             @@      %%%       @@
               @@    %%      @@
                    %%
                    %

    Azure Functions Core Tools (2.4.419 Commit hash: c9c1724d002bd90b2e6b41393915ea3a26bcf0ce)
    Function Runtime Version: 2.0.12332.0
    ```

> [!div class="nextstepaction"]
> [我安装了必备项](tutorial-vscode-serverless-node-02.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azurefunctions&step=getting-started)
