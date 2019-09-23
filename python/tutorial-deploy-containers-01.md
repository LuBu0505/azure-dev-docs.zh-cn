---
title: 使用 Visual Studio Code 将 Docker 容器部署到 Azure 应用服务
description: 教程步骤 1，简介和先决条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: kraigb
ms.openlocfilehash: 37df8bf4a84d13d6fc4e25ea6c91ef14b6f6f502
ms.sourcegitcommit: 74e28a479c87a3a53592646420b78e69852dd86a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2019
ms.locfileid: "71019485"
---
# <a name="deploy-containers-to-azure-app-service"></a>将容器部署到 Azure 应用服务

本教程详细介绍了如何使用 Visual Studio Code 将容器映像从容器注册表部署到 [Azure 应用服务](https://azure.microsoft.com/services/app-service/containers/)，整个过程在 Visual Studio Code 中进行。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”  链接来提交反馈。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 帐户](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- 一个已上传到容器注册表的合适容器。 例如，可以在[创建容器](https://code.visualstudio.com/python/tutorial-create-containers)一文中详细了解如何使用 Python Web 应用创建容器，以及如何将它添加到注册表。
- [适用于 VS Code 的 Azure 应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [适用于 VS Code 的 Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登录到 Azure](tutorial-deploy-containers-02.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-containers&step=01-verify-prerequisites)
