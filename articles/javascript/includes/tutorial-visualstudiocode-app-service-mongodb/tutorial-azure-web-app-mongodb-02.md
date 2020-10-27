---
title: include 文件 tutorial-azure-web-app-mongodb-02.md
description: include 文件 tutorial-azure-web-app-mongodb-02.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: fafb2b452e9e1bfab34d9c2d9d46adaabf4fd022
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183798"
---
在本教程的这一部分中，将示例应用程序下载到本地计算机，并从 Visual Studio Code 终端运行它。 然后可以在浏览器中查看本地运行的应用。 

## <a name="download-and-run-the-initial-expressjs-app"></a>下载并运行初始 Express.js 应用

初始 Express.js Web 应用作为起点提供。 在此过程中，下载应用、安装依赖项并运行应用。 初始应用尝试连接到数据库（如果可用）。 如果数据库不可用，网站仍会成功响应请求。 

1. [下载压缩的 GitHub 存储库](https://github.com/Azure-Samples/js-e2e-express-mongo.git)到本地计算机，然后展开到一个文件夹。 
1. 使用 Visual Studio Code 打开该文件夹。 可以右键单击该文件夹并选择“使用代码打开”，或在文件夹内部使用等效的 CLI：

    ```console
    code .
    ```

1. 在 Visual Studio Code 中，打开终端窗口，然后运行以下命令以安装示例的依赖项。

    ```javascript
    npm install
    ```

1. 在同一终端窗口中，运行命令以运行 Web 应用。

    ```javascript
    npm start
    ```

1. 打开 Web 浏览器并使用以下 url 查看本地计算机上的 Web 应用。

    ```url
    http://localhost:8080/
    ```

    如果在浏览器中看到这一简单的 Web 应用，其中文本显示“找不到数据库”，则本教程的此部分已成功完成。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::