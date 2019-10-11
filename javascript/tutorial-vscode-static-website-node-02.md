---
title: 在 Visual Studio Code 中创建静态 Node.js 应用
description: 教程第 2 部分：创建示例应用
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 566d166a69bbbee59726b8e381bee4a24077d8c6
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685980"
---
# <a name="create-the-app"></a>创建应用程序

[上一步：简介和先决条件](tutorial-vscode-static-website-node-01.md)

在此步骤中，将使用 React 实用程序 CLI [create-react-app](https://github.com/facebook/create-react-app) 创建一个可以部署到 Azure 的简单 React 应用。 可以交替使用 Angular、Vue、其他框架或任何包含几个 HTML 文件的文件夹。 如果已有一个可以部署的应用，可以跳到[创建 Azure 存储帐户](tutorial-vscode-static-website-node-03.md)。

1. 使用 create-react-app 工具通过运行以下命令搭建一个名为 `my-react-app` 的新 React 应用：

    ```bash
    npm create react-app my-react-app
    ```

1. 通过切换到新文件夹并运行 `npm run build` 来生成应用程序：

    ```bash
    cd my-react-app
    npm run build
    ```

1. 现在，应该在 my-react-app  文件夹中有一个 build  文件夹。 build  文件夹包含部署到 Azure 存储的 HTML、CSS 和 JavaScript 文件。

1. 使用以下命令运行应用：

    ```bash
    npm start
    ```

1. 将浏览器打开到 [http://localhost:3000](http://localhost:3000) 以验证该应用正在运行：

    ![正在运行的示例 React 应用](media/static-website/local-app.png)

1. 通过在终端中或命令提示符下按 **Ctrl**+**C** 来停止服务器。

> [!div class="nextstepaction"]
> [我创建了应用](tutorial-vscode-static-website-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
