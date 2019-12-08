---
title: 从 Visual Studio Code 中部署 Node.js 应用的容器映像
description: 教程第 3 部分：创建 Node.js 应用程序映像
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: c3662c7d21359008bdc0cc5c3050fb2fdc7d6241
ms.sourcegitcommit: 9d0a6de18d9b5180c1cb485eff8e2774de48d225
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2019
ms.locfileid: "74540520"
---
# <a name="create-your-nodejs-application-image"></a>创建 Node.js 应用程序映像

[上一步：使用容器注册表](tutorial-vscode-docker-node-02.md)

在此步骤中，将使用 Visual Studio Code 中的 Docker 扩展添加所需的文件，以便为你的应用创建映像、生成映像并将映像推送到注册表。

如果还没有适用于本演练的应用，请使用 [Visual Studio Code Node.js 教程](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)中的应用。

## <a name="add-docker-files"></a>添加 Docker 文件

1. 在 Visual Studio Code 中打开命令面板  (**F1**)，键入 `add docker files to workspace`，然后选择 **Docker:Add Docker files to workspace** 命令。

1. 出现提示时，请选择 **Node.js** 作为应用程序类型，然后选择应用程序侦听的端口。

1. 该命令将创建一个 `Dockerfile`，以及一些用于 Docker 撰写的配置文件和一个 `.dockerignore`。

    > [!TIP]
    > VS Code 为 Docker 文件提供了很大的支持。 请参阅 VS Code 文档中的[使用 Docker](https://code.visualstudio.com/docs/azure/docker)，了解丰富的语言功能，例如智能建议、完成和错误检测。

## <a name="build-a-docker-image"></a>生成 Docker 映像

`Dockerfile` 描述你的应用环境，包括源文件的位置和用于在容器中启动应用的命令。

> [!TIP]
> 容器与映像：容器是映像的实例。

1. 打开命令面板  (**F1**)，然后运行 **Docker Images:Build Image** 以生成映像。

1. 出现提示时，选择刚创建的 `Dockerfile`，并为该映像指定名称。 该名称必须包含目标注册表或 Docker Hub 帐户：

    `[registry or username]/[image name]:[tag]`

    在使用 Azure 容器注册表的本教程中，映像名称如下所示：

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    如果使用的是 Docker 中心，请使用 Docker Hub 用户名。 例如：

    `fiveisprime/myexpressapp:latest`

1. 完成后，Visual Studio Code 的“终端”  面板将打开，以便运行 `docker build` 命令。 输出还会显示构成应用环境的每个步骤或层。

1. 生成后，该映像将显示在 **DOCKER** 资源管理器中的“映像”  下。

    ![Visual Studio Code 中的 docker 映像列表](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>将映像推送到注册表

1. 在 Visual Studio Code 中打开命令面板  (**F1**)，然后运行 **Docker Images:Push**，并选择刚生成的映像。 “终端”  面板显示用于此操作的 `docker push` 命令。

1. 完成后，在 Docker 扩展资源管理器中展开“映像”  节点以查看映像。

    ![出现在 Azure 容器注册表中的已推送映像](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [我已为应用程序创建了映像](tutorial-vscode-docker-node-04.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
