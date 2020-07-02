---
title: 在 Visual Studio Code 中创建 Azure 应用服务
description: 教程第 2 部分：创建 Node.js 应用并在本地运行它
ms.topic: conceptual
ms.date: 03/04/2020
ms.openlocfilehash: 86d3801b31f1a0c5fb988940a7c9f550a991f0d2
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85791224"
---
# <a name="create-and-run-a-local-nodejs-app"></a>创建并运行本地 Node.js 应用

[上一步：简介和先决条件](tutorial-vscode-azure-app-service-node-01.md)

在此步骤中，我们使用 Express 应用程序生成器创建一个简单的 Node.js 应用。 然后，在本地运行该应用。

1. 在终端中或命令提示符处，导航到要创建应用文件夹的位置。

1. 运行以下命令，使用 Express Generator 创建一个名为 *expressApp1* 的新 Express 应用。 （`--view pug --git` 参数告知生成器使用 [pug](https://pugjs.org/api/getting-started.html) 模板引擎（以前称为 Jade）并创建 *.gitignore* 文件。）

    ```bash
    npx express-generator expressApp1 --view pug -–git
    ```

1. 导航到应用文件夹：

    ```bash
    cd expressApp1
    ```

1. 安装应用程序的依赖项：

    ```bash
    npm install
    ```

1. 启动服务器：

    ```bash
    npm start
    ```

1. 通过打开浏览器访问 `http://localhost:3000` 来测试应用。 站点应如下所示：

    ![运行 Express 应用程序](media/deploy-azure/express.png)

1. 在终端中按 **Ctrl**+**C**，停止服务器。

> [!div class="nextstepaction"]
> [我创建了 Node.js 应用](tutorial-vscode-azure-app-service-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=create-app)
