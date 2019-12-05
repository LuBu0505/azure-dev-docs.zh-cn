---
title: 在 Visual Studio Code 中将更改部署到静态 Node.js 网站
description: 教程第 5 部分：进行更改并重新部署
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 0db773cdea2e288dc461479c3753b94c9a286e82
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74466640"
---
# <a name="make-changes-and-redeploy"></a>进行更改并重新部署

[上一步：部署到 Azure 存储](tutorial-vscode-static-website-node-04.md)

在此步骤中，将对应用的源代码进行简单更改，并重新部署站点以体验端到端部署工作流。

# <a name="angulartabangular"></a>[Angular](#tab/angular)

1. 在 Visual Studio Code 中，打开 _src/app/app.component.html_ 文件，更改第 305 行以匹配以下内容：

    ```html
    <span>Welcome To Azure</span>
    ```

1. 在终端或命令提示符处，运行 `npm run build`。

1. 在 VS Code 中，右键单击已更新的 _dist/my-static-site_ 文件夹，然后再次选择“部署到静态网站”  。 选择存储帐户，并确认要部署所做的更改。 （在部署更改之前，Azure 扩展会自动删除旧文件以避免缓存问题。）

1. 部署完成后，请在浏览器中刷新站点以观察更改：

    ![重新部署后应用中的更改](media/static-website/updated-azure-app-angular.png)

# <a name="reacttabreact"></a>[React](#tab/react)

1. 在 Visual Studio Code 中，打开 _src/app.js_ 文件，更改第 11 行以匹配以下内容：

    ```html
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. 在终端或命令提示符处，运行 `npm run build`。

1. 在 VS Code 中，右键单击已更新的 _build_ 文件夹，然后再次选择“部署到静态网站”  。 选择存储帐户，并确认要部署所做的更改。 （在部署更改之前，Azure 扩展会自动删除旧文件以避免缓存问题。）

1. 部署完成后，请在浏览器中刷新站点以观察更改：

    ![重新部署后应用中的更改](media/static-website/updated-azure-app-react.png)

# <a name="vuetabvue"></a>[Vue](#tab/vue)

1. 在 Visual Studio Code 中，打开 _src/App.vue_ 文件，更改第 11 行以匹配以下内容：

    ```html
    <HelloWorld msg="Welcome to Azure!" />
    ```

1. 在终端或命令提示符处，运行 `npm run build`。

1. 在 VS Code 中，右键单击已更新的 _dist_ 文件夹，然后再次选择“部署到静态网站”  。 选择存储帐户，并确认要部署所做的更改。 （在部署更改之前，Azure 扩展会自动删除旧文件以避免缓存问题。）

1. 部署完成后，请在浏览器中刷新站点以观察更改：

    ![重新部署后应用中的更改](media/static-website/updated-azure-app-vue.png)

---

> [!div class="nextstepaction"]
> [我部署了更改](tutorial-vscode-static-website-node-06.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
