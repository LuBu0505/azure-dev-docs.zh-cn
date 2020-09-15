---
title: 教程：使用 Visual Studio Code 将 Docker 容器部署到 Azure 应用服务
description: 教程步骤 1, 使用容器, 简介和先决条件。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: f0fb983a596ca1828809d1d829af5517e8af66df
ms.sourcegitcommit: 9e282fc2ec967bee181c3034e7e70b28ae308905
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/04/2020
ms.locfileid: "89473552"
---
# <a name="tutorial-deploy-docker-containers-to-azure-app-service-with-visual-studio-code"></a>教程：使用 Visual Studio Code 将 Docker 容器部署到 Azure 应用服务

本文详细介绍了如何使用 Visual Studio Code 将容器映像从容器注册表部署到 [Azure 应用服务](/azure/app-service/)，整个过程在 Visual Studio Code 中进行。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”链接来提交反馈。

有关演示视频，请观看虚拟 PyCon 2020 中的 <a href="https://www.youtube.com/watch?v=t79HDLC5kQA&feature=youtu.be&ocid=AID3006292" target="_blank">VS Code 开发容器中的 Django 应用</a> (youtube.com)。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 帐户](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)
- [Visual Studio Code](https://code.visualstudio.com/)
- 一个已上传到容器注册表的合适容器。 有关如何使用 Python Web 应用创建容器的详细信息，可参阅 [Python in containers](https://code.visualstudio.com/docs/containers/quickstart-python)（容器中的 Python）。
- [适用于 VS Code 的 Azure 应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [适用于 VS Code 的 Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登录到 Azure - 转到步骤 2 >>>](tutorial-deploy-containers-02.md)

问题？ 使用页面底部的“此页面”反馈提交 GitHub 问题。
