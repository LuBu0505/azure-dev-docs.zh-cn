---
title: 从 Visual Studio Code 部署 Node.js 应用的容器映像
description: 教程第 4 部分，将映像部署到 Azure 应用服务
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 60eb5be0b3d4049c7955195f3bb6bc85dd2b2498
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686016"
---
# <a name="deploy-the-image-to-azure-app-service"></a>将映像部署到 Azure 应用服务

[上一步：创建应用映像](tutorial-vscode-docker-node-03.md)

在此步骤中，你将直接从 Visual Studio Code 将推送到注册表的映像部署到 [Azure 应用服务](https://azure.microsoft.com/en-us/services/app-service/)。

1. 在 **DOCKER** 资源管理器中，展开“注册表”  下的映像节点，右键单击 `:latest`，然后选择“将映像部署到 Azure 应用服务”  。

    ![从资源管理器部署](media/deploy-containers/deploy-image-command.png)

1. 出现提示时，为应用服务提供值：

    - 该名称在 Azure 中必须唯一。
    - 选择现有资源组或创建一个新组。 （**资源组**实际上是 Azure 中应用程序资源的命名集合。）
    - 选择现有的应用服务计划或创建新的计划。 （**应用服务计划**定义了托管网站的物理资源。 可以在本教程中使用基本或免费计划层。）

1. 部署完成后，Visual Studio Code 将显示一个通知，其中包含网站 URL：

    ![成功的部署消息](media/deploy-containers/deploy-successful.png)

1. 还可以在 Visual Studio Code 的“输出”  面板的 **Docker** 部分中查看结果：

    ![成功的部署输出](media/deploy-containers/deploy-output.png)

1. 若要浏览部署的网站，可以 **Ctrl**+**Click**“输出”  面板中的 URL。 新的应用服务还出现在 Visual Studio Code 的 **AZURE** 资源管理器中的“应用服务”  部分下，可以在其中右键单击网站并选择“浏览网站”  。

> [!div class="nextstepaction"]
> [我的站点位于 Azure 上](tutorial-vscode-docker-node-05.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=deploy-app)
