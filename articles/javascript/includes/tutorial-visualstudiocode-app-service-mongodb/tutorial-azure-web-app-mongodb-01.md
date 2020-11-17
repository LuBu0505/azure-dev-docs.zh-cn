---
title: include 文件 tutorial-azure-web-app-mongodb-01.md
description: include 文件 tutorial-azure-web-app-mongodb-01.md
ms.date: 10/13/2020
ms.topic: include
ms.custom: devx-track-javascript
ms.openlocfilehash: 61bb61d147e01061dcf58701132feddd09c53b59
ms.sourcegitcommit: 801682d3fc9651bf95d44e58574d5a4564be6feb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "94341087"
---
在本教程的这一部分，需要一个 Azure 订阅和所有软件才能使用本教程。

## <a name="create-or-use-existing-azure-subscription"></a>创建 Azure 订阅或使用现有 Azure 订阅 

* 具有活动订阅的 Azure 帐户。 [免费创建一个](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)。

## <a name="install-software"></a>安装软件

- [Node.js 8.x+ 和 npm](https://nodejs.org/en/download)，即安装到本地计算机的 Node.js 包管理器。
- [Docker](https://docs.docker.com/get-docker/) - Docker 用于提供本地 MongoDB 数据库，而无需安装 MongoDB。 
    - 如果需要使用 Docker 获取本地 MongoDB 数据库，还需要使用：
        -  Visual Studio [开发容器](https://code.visualstudio.com/docs/remote/containers)提供了几个用于 JavaScript 开发的常用容器。 
        - [远程容器](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
    - 如果你已有本地 MongoDB 并且不想安装 Docker，则仍可执行此步骤。 只要以下 MongoDB URL 可用，使用开发容器访问本地运行的 MongoDB 的任何步骤都可以重新设置为使用你自己的本地 MongoDB： 
        - `mongodb://localhost:27017`
- 安装到本地计算机中的 [Visual Studio Code](https://code.visualstudio.com/)。 
- Visual Studio Code 扩展：
    - Visual Studio Code 的 [Azure 应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)（从 Visual Studio Code 中安装）。
    - [Azure 数据库](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-cosmosdb)

## <a name="want-to-know-more"></a>想要了解更多信息？ 

可选的 Visual Studio Code 扩展：
* [用于 VS Code 的 MongoDB](https://marketplace.visualstudio.com/items?itemName=mongodb.mongodb-vscode)
* [Docker](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)