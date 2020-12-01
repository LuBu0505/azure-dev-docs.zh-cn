---
title: 教程：将 Python 应用从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务
description: 教程第 1 步：为应用服务配置环境
ms.topic: conceptual
ms.date: 11/20/2020
ms.custom: devx-track-python, seo-python-october2019
ms.openlocfilehash: 31695cb929188723cc608547849eb88a76b2d003
ms.sourcegitcommit: 29930f1593563c5e968b86117945c3452bdefac1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/23/2020
ms.locfileid: "96035506"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>教程：将 Python 应用从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务

本文详细介绍如何使用 [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)扩展通过 Visual Studio Code 将 Python 应用程序部署到 Linux 上的 Azure 应用服务。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的 **遇到问题？请告诉我们。** 链接来提交反馈。

有关演示视频，请观看虚拟 PyCon 2020 中的<a href="https://www.youtube.com/watch?v=dNVvFttc-sA&feature=youtu.be&ocid=AID3006292" target="_blank">使用 VS Code 和 Azure 应用服务生成 WebApps</a> (youtube.com)。

> [!NOTE]
> 如果希望通过 CLI 部署应用，请参阅[快速入门：在 Linux 上的 Azure 应用服务中创建 Python 应用](/azure/app-service/quickstart-python)。

> [!TIP]
> [Linux 上的 Azure 应用服务](/azure/app-service/overview#app-service-on-linux)在预定义的 Docker 容器中运行源代码。 该容器使用 [Gunicorn](https://gunicorn.org) Web 服务器来运行具有 Python 3.6 及更高版本的应用。 此容器的特征详见[为 Linux 上的应用服务配置 Python 应用](/azure/app-service/configure-language-python)一文。 容器定义位于 [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/)。

## <a name="configure-your-environment"></a>配置环境

- 如果没有具备有效订阅的 Azure 帐户，请[免费创建一个](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)。

- 确保[在本地安装了 Python 3.7 或 3.8](https://python.org/downloads)。 若要验证版本，请运行以下命令：

    ```bash
    python --version
    ```

- 安装以下软件：
  - [Visual Studio Code](https://code.visualstudio.com/)。
  - Python 和 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，详见 [VS Code Python 教程 - 先决条件](https://code.visualstudio.com/docs/python/python-tutorial)。
  - [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)扩展，可以通过它在 VS Code 中与 Azure 应用服务交互。 如需常规信息，请浏览[应用服务扩展教程](https://code.visualstudio.com/tutorials/app-service-extension/getting-started)并访问 [vscode-azureappservice GitHub 存储库](https://github.com/Microsoft/vscode-azureappservice)。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登录到 Azure - 转到步骤 2 >>>](tutorial-deploy-app-service-on-linux-02.md)

[存在问题？请告诉我们。](https://aka.ms/FlaskVSCQuickstartHelp)
