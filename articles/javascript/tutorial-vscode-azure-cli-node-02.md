---
title: 使用 Azure CLI 创建要部署到 Azure 的 Node.js 应用
description: Azure CLI 教程第 2 部分：创建应用代码。
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: 89e979dabca91e47083f48e42543453facc57130
ms.sourcegitcommit: 1ddcb0f24d2ae3d1f813ec0f4369865a1c6ef322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92689140"
---
# <a name="create-the-app-code-using-express"></a>使用 Express 创建应用代码

[上一步：简介和先决条件](tutorial-vscode-azure-cli-node-01.md)

在此步骤中，将使用 [Express Generator](https://expressjs.com/en/starter/generator.html) 创建一个带 [Express](https://www.expressjs.com) 的简单 Node.js 应用。

1. 使用以下命令运行 Express Generator 并为一个名为“myExpressApp”的新 Express 应用搭建基架。 （`--view pug --git` 参数告知生成器使用 [pug](https://pugjs.org/api/getting-started.html) 模板引擎（以前称为 Jade）并创建 *.gitignore* 文件。）

    ```bash
    npx express-generator myExpressApp --view pug –git
    ```

1. 通过运行以下命令，导航到应用文件夹并安装应用的依赖项：

    ```bash
    cd myExpressApp
    npm install
    ```

1. 通过运行以下命令来启动应用服务器：

    ```bash
    npm start
    ```

1. 打开浏览器访问 `http://localhost:3000` 以查看正在运行的应用：

    ![在本地运行 Express 应用](media/azure-cli/local-app.png)

1. 完成应用测试后，通过在运行 `npm start` 的终端中按 **Ctrl**+**C** 来停止服务器。

> [!div class="nextstepaction"]
> [我创建了应用](tutorial-vscode-azure-cli-node-03.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=express)
