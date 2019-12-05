---
title: 更改应用代码并重新部署到 Azure
description: 教程第 6 部分：进行更改并重新部署
ms.topic: conceptual
ms.date: 09/24/2019
ms.openlocfilehash: 9702f10795893004965631fa99dfbfab181f2292
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467161"
---
# <a name="make-changes-and-redeploy"></a>进行更改并重新部署

[上一步：流式传输日志](tutorial-vscode-azure-cli-node-05.md)

在此步骤中，你对应用代码进行更改，将它们提交到本地 Git 存储库，然后通过推送到 Azure 来重新部署站点。

1. 在 `myExpressApp` 文件夹中，打开 *views/index.pug* 文件，并将第 5 行中的消息更改为 `p Welcome to Azure!`。

    ![编辑 index.pug 文件](media/azure-cli/editpugfile.png)

1. 保存文件。

1. 在终端中或命令提示符下，运行以下命令，将更改提交到 git：

    ```bash
    git commit -a -m "Edited message"
    ```

1. 将更改推送到我们之前创建的名为 Azure 的 Git remote：

    ```bash
    git push azure master
    ```

1. 由于应用服务已连接到 Git 存储库，该命令的输出显示更改已自动发布到 Azure： 

    ```output
    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 4 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 405 bytes | 202.00 KiB/s, done.
    Total 4 (delta 2), reused 0 (delta 0)
    remote: Updating branch 'master'.
    remote: Updating submodules.
    remote: Preparing deployment for commit id '9fd89a25b6'.
    remote: Generating deployment script.
    remote: Running deployment command...
    remote: Handling node.js deployment.
    remote: Creating app_offline.htm
    remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
    remote: Copying file: 'views\index.pug'
    remote: Deleting app_offline.htm
    remote: Using start-up script bin/www from package.json.
    remote: Generated web.config.
    remote: The package.json file does not specify node.js engine version constraints.
    remote: The node.js application will run with the default node.js version 6.9.5.
    remote: Selected npm version 3.10.10
    remote: ..
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://msdocs-node-cli.scm.azurewebsites.net/msdocs-node-cli.git
       a98bad8..9fd89a2  master -> master
    ```

1. 在浏览器中刷新应用以查看这些更改：

    ![浏览器中显示的已发布更改](media/azure-cli/remote-app-changes.png)

> [!div class="nextstepaction"]
> [我看到了更改](tutorial-vscode-azure-cli-node-07.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment&step=publishing-changes)
