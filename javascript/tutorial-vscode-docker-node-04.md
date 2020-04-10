---
title: 从 Visual Studio Code 中部署 Node.js 应用的容器映像
description: 教程第 4 部分：创建 Node.js 应用程序映像
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: a8659edb4d0b3664c7704fd0bedde0c274562f3c
ms.sourcegitcommit: f89c59f772364ec717e751fb59105039e6fab60c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2020
ms.locfileid: "80740689"
---
# <a name="create-your-nodejs-application-image"></a>创建 Node.js 应用程序映像

[上一步：创建并运行本地 Node.js 应用](tutorial-vscode-docker-node-03.md)

## <a name="add-docker-files"></a>添加 Docker 文件

1. 在 Visual Studio Code 中打开命令面板  (**F1**)，键入 `add docker files to workspace`，然后选择 **Docker:Add Docker files to workspace** 命令。

1. 出现提示时，请选择 **Node.js** 作为应用程序类型，对 Docker compose 文件回答“否”  ，然后选择应用程序侦听的端口（应该为 3000）。

1. 该命令将创建一个 `Dockerfile`，以及一些用于 Docker 撰写的配置文件和一个 `.dockerignore`。

    > [!TIP]
    > VS Code 为 Docker 文件提供了很大的支持。 请参阅 VS Code 文档中的[使用 Docker](https://code.visualstudio.com/docs/azure/docker)，了解丰富的语言功能，例如智能建议、完成和错误检测。

## <a name="build-a-docker-image"></a>生成 Docker 映像

`Dockerfile` 描述你的应用环境，包括源文件的位置和用于在容器中启动应用的命令。

> [!TIP]
> 容器与映像：容器是映像的实例。

1. 打开命令面板  (**F1**)，然后运行 **Docker Images:Build Image** 以生成映像。 VS Code 使用当前文件夹中的 Dockerfile，并为映像提供与当前文件夹相同的名称。

1. 完成后，Visual Studio Code 的“终端”  面板将打开，以便运行 `docker build` 命令。 输出还会显示构成应用环境的每个步骤或层。

1. 生成后，该映像将显示在 **DOCKER** 资源管理器中的“映像”  下。

    ![Visual Studio Code 中的 docker 映像列表](media/deploy-containers/image-list.png)

## <a name="push-the-image-to-a-registry"></a>将映像推送到注册表

1. 若要将映像推送到注册表，必须先使用注册表名称标记该映像。 在 **DOCKER** 资源管理器中，右键单击**最新**的映像。

    ![Visual Studio Code 中的映像标记命令](media/deploy-containers/tag-command.png)

1. 在随后出现的提示中，完成标记，然后按 **Enter**。

    按照约定，标记使用以下格式：

    `[registry or username]/[image name]:[tag]`

    如果使用 Azure 容器注册表，映像名称将类似于以下内容：

    `msdocsvscodereg.azurecr.io/myexpressapp:latest`

    如果使用的是 Docker 中心，请使用 Docker Hub 用户名。 例如：

    `fiveisprime/myexpressapp:latest`

1. 新标记的映像现在显示在“映像”  下的节点中，该节点包含注册表名称。 展开该节点，右键单击“最新”  ，然后选择“推送”  。

    ![Visual Studio Code 中的推送映像命令](media/deploy-containers/push-command.png)

1. “终端”  面板显示用于此操作的 `docker push` 命令。 目标注册表由映像名称中指定的注册表确定。

   > [!IMPORTANT]
   > 如果输出显示“需要身份验证”，请在终端中运行 `az acr login --name <your registry name>`。

1. 完成后，在 Docker 扩展资源管理器中展开“注册表”  节点以查看注册表中的映像。

    ![出现在 Azure 容器注册表中的已推送映像](media/deploy-containers/image-in-acr.png)

> [!div class="nextstepaction"]
> [我已为应用程序创建了映像](tutorial-vscode-docker-node-05.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=containerize-app)
