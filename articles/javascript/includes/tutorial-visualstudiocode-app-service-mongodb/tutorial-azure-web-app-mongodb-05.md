---
title: include 文件 tutorial-azure-web-app-mongodb-05.md
description: include 文件 tutorial-azure-web-app-mongodb-05.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 9b8a718ec921050237f911ee4b5c3a47b6bbb45e
ms.sourcegitcommit: 8a2a7df568c69fff2080ffab248409040efda1ac
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2020
ms.locfileid: "92183803"
---
在本教程的这一部分中，创建一个基于云的数据库并连接远程应用以使用该云数据库。 

## <a name="create-a-cosmos-database"></a>创建 Cosmos 数据库

创建 Cosmos 资源来托管基于云的 MongoDB 数据库。 

1. 在 Visual Studio Code 中，在最左侧的菜单中选择“Azure”图标，然后选择“数据库”部分 。 

    如果“数据库”部分不可见，请确保已在顶部 Azure“...”菜单中选中该部分 。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-azure-extension-select-database-section.png" alt-text="Visual Studio Code 的“远程容器”图标的部分屏幕截图"::: 

1. 在 Azure 资源管理器的“数据库”部分，右键单击选择订阅，然后选择“创建服务器” 。
1. 在“创建新的 Azure 数据库服务器”命令面板中，选择“用于 MongoDB 的 Azure Cosmos DB API” 。 
1. 使用下表按照提示进行操作，理解如何使用你的值。 创建数据库最多可能需要 15 分钟。

    |属性|“值”|
    |--|--|
    |为新资源输入全局唯一帐户名称。| 为资源输入一个值，如 `cosmos-mongodb-YOUR-NAME`。 将 `YOUR-NAME` 替换为你的名称或唯一 ID。 此唯一名称还用作 URL 的一部分，用于访问浏览器中的资源。|
    |选择或创建资源组。|如果需要创建资源组，请使用命名约定来标识所有者、用途和区域，如 `westus-cosmostutorial-joesmith`。|
    |位置|资源的位置。 对于本教程，请选择离你近的区域位置。|

    > [!CAUTION]
    > 创建资源最多可能需要 15 分钟。     

1. 在资源管理器中查看新创建的 Cosmos 资源。 其中尚无任何数据库。 
1. 仍处于 Azure 数据库资源管理器中时，右键单击资源名称，然后选择“复制连接字符串”以复制该连接字符串。 本教程后面的步骤中会用到此密钥。

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-databases-extenion-copy-connection-string.png" alt-text="Visual Studio Code 的“远程容器”图标的部分屏幕截图":::

## <a name="optional-use-new-cloud-database-in-local-environment"></a>可选：在本地环境中使用新的云数据库

为了使用新的云数据库，需要将本地应用程序更改为使用新的连接字符串。 

1. 在 Visual Studio Code 中，打开 `.env` 文件，然后将 DATABASE_URL 值添加到新的连接字符串。 
1. 将 `&retrywrites=false` 添加到连接字符串的末尾，以便在 Web 应用首次运行时创建数据库。 
1. 在不使用开发容器的情况下在本地运行 Web 应用，确保应用连接到云数据库。 

## <a name="use-new-cloud-database-in-remote-web-app"></a>在远程 Web 应用中使用新的云数据库

使用名为 `DATABASE_URL` 的环境变量设置与数据库的连接。 若要将远程 Web 应用配置为使用该环境变量，需要在远程 Web 应用上创建该变量。 

1. 在 Visual Studio Code 的 Azure 应用服务资源管理器中，选择并展开 Web 应用服务节点。
1.  右键单击“应用程序设置”节点，为用于 MongoDB 的 Azure Cosmos DB 添加 `DATABASE_URL` 属性和连接字符串。 将 `&retrywrites=false` 添加到连接字符串的末尾，以便在 Web 应用首次运行时创建数据库。 

    :::image type="content" source="../../media/tutorial-end-to-end-app-cosmos/vscode-remote-web-app-application-settings.png" alt-text="Visual Studio Code 的“远程容器”图标的部分屏幕截图"::: 

1. 在浏览器中打开远程网站，并使用窗体添加和删除数据。 

## <a name="want-to-know-more"></a>想要了解更多信息？ 

### <a name="mongodb-connection-strings"></a>MongoDB 连接字符串
首次运行代码时创建数据库可能需要重试，因此连接字符串必须具有 `&retrywrites=false` 设置。 如果想要详细了解这一问题，请从 GitHub 上的[公共问题 #1296](https://github.com/microsoft/vscode-cosmosdb/issues/1296) 开始。 