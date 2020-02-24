---
title: 在 Visual Studio Code 中创建静态 Node.js 应用
description: 教程第 2 部分：创建示例应用
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 69c0e7d6f43829546e5f23ec63a4ac35b71d7e78
ms.sourcegitcommit: 44d1abfb836f90b8731d7ea5d5a5af09245b2b89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2020
ms.locfileid: "77422521"
---
# <a name="create-the-app"></a>创建应用

[上一步：简介和先决条件](tutorial-vscode-static-website-node-01.md)

在此步骤中，我们将使用 [Angular](https://cli.angular.io/)、[React](https://github.com/facebook/create-react-app)、[Vue](https://cli.vuejs.org/) 或 [Svelte](https://github.com/sveltejs/template) 的命令行界面 (CLI) 创建可部署到 Azure 的简单应用。 可以交替地使用任何其他 JavaScript 框架来生成一组静态文件，或者任何包含 HTML、CSS 或 JavaScript 文件的文件夹。 如果已有一个可以部署的应用，可以跳到[创建 Azure 存储帐户](tutorial-vscode-static-website-node-03.md)。

# <a name="angular"></a>[Angular](#tab/angular)

1. 使用 CLI 通过运行以下命令搭建名为“my-static-app”的新应用：

    ```bash
    npx @angular/cli new my-static-app
    ```

    当 CLI 询问任何配置问题时，按 Enter 选择默认选项。

1. 通过切换到新文件夹并运行 `npm run build` 来生成应用程序：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 现在，应该在 my-static-app  文件夹中有一个 dist  文件夹。 在该 _dist_ 文件夹内，将有一个与你的项目同名的文件夹 _my-static-app_。 _build/my-static-app_ 文件夹包含部署到 Azure 存储的 HTML、CSS 和 JavaScript 文件。

1. 使用以下命令运行应用：

    ```bash
    npm start
    ```

1. 将浏览器打开到 [http://localhost:4200](http://localhost:4200) 以验证该应用正在运行：

    ![正在运行的示例 Angular 应用](media/static-website/local-app-angular.png)

1. 通过在终端中或命令提示符下按 **Ctrl**+**C** 来停止服务器。

# <a name="react"></a>[React](#tab/react)

1. 使用 CLI 通过运行以下命令搭建名为“my-static-app”的新应用：

    ```bash
    npx create-react-app my-static-app
    ```

1. 通过切换到新文件夹并运行 `npm run build` 来生成应用程序：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 现在，应该在 my-static-app  文件夹中有一个 build  文件夹。 build  文件夹包含部署到 Azure 存储的 HTML、CSS 和 JavaScript 文件。

1. 使用以下命令运行应用：

    ```bash
    npm start
    ```

1. 将浏览器打开到 [http://localhost:3000](http://localhost:3000) 以验证该应用正在运行：

    ![正在运行的示例 React 应用](media/static-website/local-app-react.png)

1. 通过在终端中或命令提示符下按 **Ctrl**+**C** 来停止服务器。

# <a name="vue"></a>[Vue](#tab/vue)

1. 使用 CLI 通过运行以下命令搭建名为“my-static-app”的新应用：

    ```bash
    npx @vue/cli create my-static-app
    ```

当 CLI 询问任何配置问题时，按 Enter 选择默认选项。

1. 通过切换到新文件夹并运行 `npm run build` 来生成应用程序：

    ```bash
    cd my-static-app
    npm run build
    ```

1. 现在，应该在 my-static-app  文件夹中有一个 dist  文件夹。 dist  文件夹包含部署到 Azure 存储的 HTML、CSS 和 JavaScript 文件。

1. 使用以下命令运行应用：

     ```bash
     npm run serve
     ```

1. 将浏览器打开到 [http://localhost:8080](http://localhost:8080) 以验证该应用正在运行：

    ![正在运行的示例 Vue 应用](media/static-website/local-app-vue.png)

1. 通过在终端中或命令提示符下按 **Ctrl**+**C** 来停止服务器。

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. 使用 CLI 通过运行以下命令搭建名为“my-static-app”的新应用：

    ```bash
    npx degit sveltejs/template my-static-app
    ```

1. 然后，转到新文件夹并运行 `npm install` 命令：

    ```bash
    cd my-static-app
    npm install
    ```

1. 让我们通过运行 `npm run build` 命令来生成应用程序：

    ```bash
    npm run build
    ```

1. 现在应该在 _public_ 文件夹内有一个 _build_ 文件夹。 build  文件夹包含部署到 Azure 存储的 HTML、CSS 和 JavaScript 文件。

1. 使用以下命令运行应用：

     ```bash
     npm run dev
     ```

1. 将浏览器打开到 [http://localhost:5000](http://localhost:5000) 以验证该应用正在运行：

    ![正在运行的示例 Vue 应用](media/static-website/local-app-svelte.png)

1. 通过在终端中或命令提示符下按 **Ctrl**+**C** 来停止服务器。

---

> [!div class="nextstepaction"]
> [我创建了应用](tutorial-vscode-static-website-node-03.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-app)
