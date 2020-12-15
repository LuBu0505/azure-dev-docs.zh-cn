---
title: 使用 VSCode 部署 Express.js/MongoDB 应用 - 应用服务/CosmosDB
description: 在本教程中，通过 MongoDB 本机 API 将 Node.js 应用和 MongoDB 数据库配合使用。 将 Node.js 应用程序部署到 Azure 应用服务（在 Linux 上），然后验证托管的应用是否正常运行。
ms.topic: tutorial
ms.date: 12/03/2020
ms.custom: scenarios:getting-started, languages:JavaScript, devx-track-javascript
ms.openlocfilehash: 8c40801607e11a4b929f0bb76926122d26217805
ms.sourcegitcommit: 550b165d0b910f4ea9652d8401dd4fc93f057f05
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96610963"
---
# <a name="deploy-expressjs-mongodb-app-to-app-service-from-visual-studio-code"></a>在 Visual Studio Code 中将 Express.js MongoDB 应用部署到应用服务

将连接到 MongoDB 的 Express.js 应用程序部署到 Azure 应用服务（在 Linux 上）和 CosmosDB。 

已为你完成编程工作，本教程重点介绍如何借助 Azure 扩展从 Visual Studio Code 内部成功使用本地和远程 Azure 环境。

## <a name="top-tasks"></a>主要任务

本教程包括几个面向 JavaScript 开发人员的主要 Azure 任务：

* 创建 CosmosDB 资源以托管 MongoDB 数据库
* 创建应用服务资源以托管 Express.js 应用
* 将 Express.js 应用部署到应用服务

## <a name="sample-application"></a>示例应用程序

[示例 Express.js 应用](https://github.com/Azure-Samples/js-e2e-express-mongo)包含以下元素：

* 托管在端口 8080 上的 Express.js 服务器
* 简单的 React.js 服务器端视图引擎
* MongoDB 本机 API 函数，用于插入、删除和查找数据

:::image type="content" source="../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::

## <a name="create-or-use-existing-azure-subscription"></a>创建 Azure 订阅或使用现有 Azure 订阅 

* 具有活动订阅的 Azure 帐户。 [免费创建一个](https://azure.microsoft.com/free/)。

## <a name="install-software"></a>安装软件

- [Node.js 12 (LTS) 和 npm](https://nodejs.org/en/download)，即安装到本地计算机的 Node.js 包管理器。
- 安装到本地计算机中的 [Visual Studio Code](https://code.visualstudio.com/)。 
- Visual Studio Code 扩展：
    - Visual Studio Code 的 [Azure 应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)（从 Visual Studio Code 中安装）。
    - [Azure 数据库](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

## <a name="create-a-cosmosdb-database-resource-for-mongodb"></a>为 MongoDB 创建 CosmosDB 数据库资源

首先创建 Cosmos 资源，因为这将需要几分钟时间。 

1. 在 Visual Studio Code 中，在最左侧的菜单中选择“Azure”图标，然后选择“数据库”部分 。 

    如果“数据库”部分不可见，请确保已在顶部 Azure“...”菜单中选中该部分 。 

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-azure-extension-select-database-section.png" alt-text="Visual Studio Code 的“远程容器”图标的部分屏幕截图"::: 

1. 在 Azure 资源管理器的“数据库”部分，右键单击选择订阅，然后选择“创建服务器” 。
1. 在“创建新的 Azure 数据库服务器”命令面板中，选择“用于 MongoDB 的 Azure Cosmos DB API” 。 
1. 使用下表按照提示进行操作，理解如何使用你的值。 创建数据库最多可能需要 15 分钟。

    |属性|“值”|
    |--|--|
    |为新资源输入全局唯一帐户名称。| 为资源输入一个值，如 `cosmos-mongodb-YOUR-NAME`。 将 `YOUR-NAME` 替换为你的名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。|
    |选择或创建资源组。|如果需要创建资源组，请使用命名约定来标识所有者、用途和区域，如 `westus-cosmostutorial-joesmith`。|
    |位置|资源的位置。 对于本教程，请选择离你近的区域位置。|

    创建资源最多可能需要 15 分钟。 如果时间有限，可跳过下一节，但记得在几分钟后返回来完成下一节。

## <a name="get-cosmosdb-connection-string"></a>获取 CosmosDB 连接字符串

仍处于 Azure 数据库资源管理器中时，右键单击资源名称，然后选择“复制连接字符串”以复制该连接字符串。 在本教程的后面部分，你需要为环境变量文件执行此操作。

:::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-databases-extenion-copy-connection-string.png" alt-text="Express.js Web 应用窗体和来自本地 MongoDB 的数据结果。":::

## <a name="download-and-run-the-sample-expressjs-app"></a>下载并运行示例 Express.js 应用

已为你提供 Express.js Web 应用。 下载应用，安装依赖项并运行应用。 

1. [下载压缩的 GitHub 存储库](https://github.com/Azure-Samples/js-e2e-express-mongo.git)到本地计算机，然后展开到一个文件夹。 
1. 使用 Visual Studio Code 打开该文件夹。 可以右键单击该文件夹并选择“使用代码打开”，或在文件夹内部使用等效的 CLI：

    ```bash
    code .
    ```

1. 编辑环境文件 `.env`，添加 CosmosDB 的连接字符串属性作为 `DATABASE_URL` 属性的值。 

    ```bash
    ENVIRONMENT=development
    DATABASE_NAME=
    DATABASE_COLLECTION_NAME=
    DATABASE_URL=
    ```

1. 在 Visual Studio Code 中，打开终端窗口，然后运行以下命令以安装示例的依赖项并启动 Web 应用。

    ```bash
    npm install && npm start
    ```

1. 在浏览器中查看本地计算机上的 Web 应用。

    ```url
    http://localhost:8080/
    ```

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/nodejs-app-connected-mongodb-form.png" alt-text="连接到 MongoDB 数据库的简单 Node.js 应用。":::

## <a name="create-web-app-resource-and-deploy-expressjs-app"></a>创建 Web 应用资源并部署 Express.js 应用

使用应用服务的 Visual Studio Code 扩展创建应用服务资源，并将 Express.js Web 应用部署到资源。

1. 导航到 Azure 资源管理器。 右键单击该订阅，然后选择 `Create new web app...`。

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/create-web-app-with-extension.png" alt-text="Visual Studio Code 的部分屏幕截图 - 使用 Azure 应用服务扩展创建 Web 应用。":::

1. 使用下表按照提示进行操作，理解如何使用你的值。

    |属性|“值”|
    |--|--|
    |为新 Web 应用输入全局唯一名称。| 为应用服务资源输入一个值，如 `web-app-with-mongodb-YOUR-NAME`。 将 `<YOUR-NAME>` 替换为你的名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。|
    |为 Linux 应用选择运行时。|选择 `Node 12 LTS`。|

1. 应用创建过程完成后，将在 Visual Studio Code 的右下角显示一条状态消息，可选择 `Deploy` 或 `View output`。 选择 `Deploy`。

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-extension-create-web-app-deploy-web-app.png" alt-text="Visual Studio Code 的部分屏幕截图 - 在创建 Web 应用后立即使用 Azure 应用服务扩展来部署 Web 应用。":::

    如果状态消息不再可见，则可以通过选择 Azure 资源管理器进行部署，然后右键单击资源名称，并选择“部署到 Web 应用…”。

1. 在部署过程中，可通过选择显示的通知来查看“输出窗口”。  该操作将显示部署的滚动状态。 

1. 部署完成后，将显示一条通知。 选择“流式传输日志”以查看滚动日志。 

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-service-deployed.png" alt-text="服务已部署。“流式传输日志”。":::

    :::image type="content" source="../media/tutorial-end-to-end-app-cosmos/vscode-app-service-stream-logs.png" alt-text="部署完成后，将显示一条通知，允许你选择“流式传输日志”。":::    

1. 在浏览器中打开该网站，将文本 `YOUR-RESOURCE_NAME` 替换为你自己的资源名称：`https://YOUR-RESOURCE_NAME.azurewebsites.net`。
1. 使用 Web 应用，添加和删除项。 

## <a name="clean-up-resources"></a>清理资源 

完成本教程后，需要删除创建的两个资源，以确保不会向你收费。 

1. 在 Visual Studio Code 中，使用用于数据库的 Azure 资源管理器，右键单击资源并选择“删除帐户…”。
1. 在 Visual Studio Code 中，使用用于应用服务的 Azure 资源管理器，右键单击资源并选择“删除”。

## <a name="next-steps"></a>后续步骤

继续了解应用服务和 CosmosDB：
* [为 Azure 应用服务配置 Node.js 应用](/azure/app-service/configure-language-nodejs?pivots=platform-linux)
* [使用 SSH 进行连接](/azure/app-service/configure-linux-open-ssh-session)
* [将数据迁移到 CosmosDB](/azure/dms/tutorial-mongodb-cosmos-db?toc=/azure/cosmos-db/toc.json?toc=/azure/cosmos-db/toc.json)
