---
title: 在 Visual Studio Code 中将更改部署到静态 Node.js 网站
description: 教程第 5 部分：进行更改并重新部署
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: kraigb
ms.openlocfilehash: 986d2a0f8999d79dfd1d856ed20a053c495a3765
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71685937"
---
# <a name="make-changes-and-redeploy"></a>进行更改并重新部署

[上一步：部署到 Azure 存储](tutorial-vscode-static-website-node-04.md)

在此步骤中，将对应用的源代码进行简单更改，并重新部署站点以体验端到端部署工作流。

1. 在 Visual Studio Code 中，打开 *src/app.js* 文件，更改第 11 行以匹配以下内容：

    ```js
    <h1 className="App-title">Welcome to Azure!</h1>
    ```

1. 在终端或命令提示符处，运行 `npm run build`。

1. 在 VS Code 中，右键单击已更新的 *build* 文件夹，然后再次选择“部署到静态网站”  。 选择存储帐户，并确认要部署所做的更改。 （在部署更改之前，Azure 扩展会自动删除旧文件以避免缓存问题。）

1. 部署完成后，请在浏览器中刷新站点以观察更改：

    ![重新部署后应用中的更改](media/static-website/updated-azure-app.png)

> [!div class="nextstepaction"]
> [我部署了更改](tutorial-vscode-static-website-node-06.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=code-change)
