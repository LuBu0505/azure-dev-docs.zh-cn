---
title: include 文件 tutorial-azure-web-app-mongodb-04.md
description: include 文件 tutorial-azure-web-app-mongodb-04.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 73e629427359a625ea3ec4796c2ec7000a21536a
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183804"
---
本教程的这一部分使用 Visual Studio Code 扩展和 Docker 容器将本地 MongoDB 数据库添加到应用程序。

## <a name="configure-visual-studio-code-to-run-containers"></a>配置 Visual Studio Code 以运行容器

在本部分中，配置开发环境以运行两个容器，一个用于 Node.js 项目，另一个用于 MongoDB 容器。 由于本部分使用 Visual Studio Code 开发容器，因此容器配置保存在 .devcontainer 文件夹中。 你可以将其提交到源代码管理中，以便团队中的其他人也可以访问本地 MongoDB。  

1. 在 Visual Studio Code 中，使用命令面板 (CTRL+Shift+P) 选择“远程容器: 添加开发容器配置文件”。 

1. 从列表中选择“Node.js 和 Mongo DB”。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-configure-development-container.png" alt-text="Visual Studio Code 命令面板的部分屏幕截图。"::: 

1. 在 \.devcontainer\devcontainer.json 文件中，找到 forwardPorts 属性，取消评论该属性并将 `8080` 添加到数组 。 如果你还需要使用 shell 访问 MongoDB 容器的权限，请将端口 `27017` 添加到数组。  

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-dev-container-configuration-forward-ports.png" alt-text="Visual Studio Code 命令面板的部分屏幕截图。"::: 

## <a name="run-web-app-locally-with-database"></a>使用数据库本地运行 Web 应用

在本部分中，使用这两个容器运行开发环境，并查看网站。 

1. 选择 Visual Studio Code 左下角的绿色“远程容器”图标。 此操作将打开命令面板。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-container-icon.png" alt-text="Visual Studio Code 命令面板的部分屏幕截图。"::: 

1. 在命令面板中，选择“远程容器: 在容器中重新打开”。 首次使用容器打开项目时，将向下拉取并启动 Node.js 和 MongoDB 映像。 这可能需要几分钟的时间。 

    容器运行时，Visual Studio Code 终端将显示 Node.js 容器的终端。 

    可根据需要使用 `ls` 命令来查看文件。 请注意，你的文件正在将共享卷用于本地计算机。 在 Node.js 容器内对代码文件所做的更改将保存在本地文件中。

1. 在终端中通过以下命令启动项目：

    ```console
    npm start
    ```

1. 使用本地 Web 应用 URL 打开浏览器：

    ```
    http://localhost:8080/
    ```

1. 在字段中输入数据并提交窗体。 使用服务器端 React 呈现显示数据。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="Visual Studio Code 命令面板的部分屏幕截图。":::

1. 完成对应用的探索之后，通过使用命令面板选择“远程容器: 在本地重新打开…”，停止容器。该操作将停止容器，但将其保留在本地计算机上。 

## <a name="want-to-know-more"></a>想要了解更多信息？ 

使用 Visual Studio Code 扩展连接到 MongoDB 容器：[用于 VS Code 的 MongoDB](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)，用于查看数据。

如果你更愿意使用 mongo shell，可通过打开新的 Visual Studio Code 窗口，再使用“远程容器: 附加到正在运行的容器…”，借助 Visual Studio Code 终端连接到 MongoDB 容器，然后选择以 `-db` 结尾的容器。 将窗口附加到容器后，打开 Visual Studio Code 终端。 可以使用以下命令立即访问 Mongo shell：

```console
mongo
```

若要清理容器，使用 Visual Studio Code 扩展 [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。