---
title: 在 Visual Studio Code 中创建 Azure 应用服务
description: 教程第 2 部分：创建 Node.js 应用
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 96c786b7cc8112c36c0aff06761417a97e30bf44
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466801"
---
# <a name="create-your-nodejs-application"></a>创建 Node.js 应用程序

[上一步：简介和先决条件](tutorial-vscode-azure-app-service-node-01.md)

在此步骤中，将使用 Express 应用程序生成器创建一个简单的 Node.js 应用，然后可以将该应用部署到 Azure。

还可以使用 [Visual Studio Code Node.js 教程](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)中的应用，在这种情况下，可以跳到[部署应用](tutorial-vscode-azure-app-service-node-03.md)。

1. 在终端中或命令提示符下，使用以下命令运行 Express 生成器，并搭建一个名为“myExpressApp”的新 Express 应用。 （`--view pug --git` 参数告知生成器使用 [pug](https://pugjs.org/api/getting-started.html) 模板引擎（以前称为 Jade）并创建 *.gitignore* 文件。）

    ```bash
    npx express-generator myExpressApp --view pug -–git
    ```

1. 通过在应用文件夹中运行 `npm install` 来安装应用程序的依赖项：

    ```bash
    cd myExpressApp
    npm install
    ```

1. 通过运行 `npm start` 启动服务器：

    ```bash
    npm start
    ```

1. 通过打开浏览器访问 [http://localhost:3000](http://localhost:3000) 来测试应用。 站点应如下所示：

    ![运行 Express 应用程序](media/deploy-azure/express.png)

> [!div class="nextstepaction"]
> [我创建了 Node.js 应用](tutorial-vscode-azure-app-service-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
