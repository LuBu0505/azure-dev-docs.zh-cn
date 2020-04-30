---
title: 从 Visual Studio Code 中将 Docker 容器部署到 Azure 应用服务
description: 教程第 1 部分：简介和先决条件。
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: bd4110bbbef5e5f65f76cd9329b194646431f91a
ms.sourcegitcommit: e6cdb0ce11a8272195a0072c7c91cc9b7e89b0b6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/24/2020
ms.locfileid: "82138784"
---
# <a name="deploy-containers-to-azure-app-service"></a>将容器部署到 Azure 应用服务

在本教程中，将使用 Visual Studio Code 通过 Docker 创建容器化的 Node.js 应用程序，将容器映像推送到注册表，然后将该映像部署到 Azure 应用服务。

## <a name="walkthrough-video"></a>演练视频

观看此视频，了解本文中内容的完整演练。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-containers-to-Azure-App-Service/player]

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Docker 扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker)。
- [Azure 应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)。
- [Node.js 和 npm](https://nodejs.org/en/download)，即 Node.js 包管理器。
- [Docker](https://www.docker.com/community-edition)。

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-docker">安装 Docker 扩展</a>

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice">安装 Azure 应用服务扩展</a>

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-docker-extension&mktingSource=vscode-tutorial-docker-extension)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

## <a name="verify-docker-install"></a>验证 Docker 安装

通过在终端或命令提示符处运行以下命令，验证是否已正确安装 Docker：

```bash
docker --version
```

输出应如下所示：

<pre>
Docker Version 17.12.0-ce, build c97c6d6
</pre>

> [!div class="nextstepaction"]
> [我已安装 Docker扩展](tutorial-vscode-docker-node-02.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=docker-extension&step=getting-started)
