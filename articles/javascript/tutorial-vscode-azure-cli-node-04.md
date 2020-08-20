---
title: 使用 Azure CLI 将应用代码部署到 Azure 应用服务
description: 教程第 4 部分，部署网站
ms.topic: conceptual
ms.date: 09/24/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: e794388280c537261e428195ac3943379f04866b
ms.sourcegitcommit: 0699b984b85782b1c441289fa756f285eae853c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/14/2020
ms.locfileid: "88217946"
---
# <a name="deploy-the-app-to-app-service"></a>将应用部署到应用服务

[上一步：创建应用服务](tutorial-vscode-azure-cli-node-03.md)

在此步骤中，将使用将本地 Git 存储库推送到 Azure 的基本过程，将 Node.js 应用代码部署到 Azure 应用服务。

1. 在终端中或命令提示符下，运行以下命令以初始化本地 Git 存储库并进行初始提交。 （*node_modules* 文件夹被忽略，因为它是在你之前运行 Express Generator 时创建的 *.gitignore* 文件中指定的。）

    ```bash
    git init
    git add -A
    git commit -m "Initial Commit"
    ```

1. 运行以下命令以在 Azure 中设置部署凭据，将 `username` 和 `pPassword` 替换为凭据。 成功后，该命令将显示 JSON 输出。

    ```azurecli
    az webapp deployment user set --user-name <username> --password <password>
    ```

1. 运行以下命令以检索我们要将应用代码推送到的 Git 终结点，将 `<your_app_name>` 替换为在上一步中创建应用服务时使用的名称：

    ```azurecli
    az webapp deployment source config-local-git --name <your_app_name>
    ```

    该命令的输出类似于以下内容：

    <pre>
    {
      "url": "https://username@msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git"
    }
    </pre>

1. 运行以下命令，使用上一步中的 URL（省略用户名），在 Git 中设置一个名为 `azure` 的新远程存储库  。 使用上一步中的示例，命令将如下所示：

    ```bash
    git remote add azure https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
    ```

1. 运行以下命令，将应用代码从 Git 存储库部署到应用服务。 该命令会提示你输入凭据：

    ```bash
    git push azure master
    ```

1. 在命令运行时，会显示来自远程主机的一系列消息。 该过程完成后，请刷新打开应用 URL 的浏览器，以查看正在运行的代码：

    ![在 Azure 上运行的应用代码](media/azure-cli/remote-app.png)

> [!TIP]
> 如果遇到错误 `Object #<eventemitter> has no method 'hrtime'`，则可能需要在站点上设置节点运行时版本。 以下命令通知站点使用节点版本 `6.9.1`。 如果站点需要其他版本或更高版本的节点，请指定完整的语义版本 `major.minor.patch`。
>
> ```azurecli
> az webapp config appsettings set --name <your_app_name> --settings
> ```

> [!div class="nextstepaction"]
> [我部署了应用](tutorial-vscode-azure-cli-node-05.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=deploy-website)
