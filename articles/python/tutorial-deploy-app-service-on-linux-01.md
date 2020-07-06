---
title: 教程：将 Python 应用从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务
description: 教程步骤 1，简介、先决条件以及登录 Azure。
ms.topic: conceptual
ms.date: 09/12/2019
ms.custom: seo-python-october2019
ms.openlocfilehash: 444dd9737d52de58f71ef9a01aeca6a203e97a92
ms.sourcegitcommit: 7474de4884bce076ce33ca77ae3584ba1598bbc6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/18/2020
ms.locfileid: "85069382"
---
# <a name="tutorial-deploy-python-apps-to-azure-app-service-on-linux-from-visual-studio-code"></a>教程：将 Python 应用从 Visual Studio Code 部署到 Linux 上的 Azure 应用服务

本文详细介绍如何使用 [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)扩展通过 Visual Studio Code 将 Python 应用程序部署到 Linux 上的 Azure 应用服务。

如果你在执行本教程中的任何步骤时遇到问题，请告知我们详情。 请使用每篇文章末尾的“我遇到了问题”链接来提交反馈。

有关演示视频，请观看虚拟 PyCon 2020 中的<a href="https://www.youtube.com/watch?v=dNVvFttc-sA&feature=youtu.be&ocid=AID3006292" target="_blank">使用 VS Code 和 Azure 应用服务生成 WebApps</a> (youtube.com)。

> [!TIP]
> [Linux 上的 Azure 应用服务](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)在预定义的 Docker 容器中运行源代码。 该容器通过 Python 3.7 来运行应用，使用 [Gunicorn](https://gunicorn.org) Web 服务器。 此容器的特征详见[为 Linux 上的应用服务配置 Python 应用](https://docs.microsoft.com/azure/app-service/containers/how-to-configure-python)一文。 容器定义本身位于 [github.com/Azure-App-Service/python](https://github.com/Azure-App-Service/python/tree/master/3.7)。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code 与 Azure 应用服务扩展](#visual-studio-code-python-and-the-azure-app-service-extension)。
- Python 环境

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

### <a name="visual-studio-code-python-and-the-azure-app-service-extension"></a>Visual Studio Code、Python 和 Azure 应用服务扩展

安装以下软件：

- [Visual Studio Code](https://code.visualstudio.com/)。
- Python 和 [Python 扩展](https://marketplace.visualstudio.com/items?itemName=ms-python.python)，详见 [VS Code Python 教程 - 先决条件](https://code.visualstudio.com/docs/python/python-tutorial)。
- [Azure 应用服务](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)扩展，可以通过它在 VS Code 中与 Azure 应用服务交互。 如需常规信息，请浏览[应用服务扩展教程](https://code.visualstudio.com/tutorials/app-service-extension/getting-started)并访问 [vscode-azureappservice GitHub 存储库](https://github.com/Microsoft/vscode-azureappservice)。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已登录到 Azure - 转到步骤 2 >>>](tutorial-deploy-app-service-on-linux-02.md)

[我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=vscode-appservice-python&step=01-verify-prerequisites)
