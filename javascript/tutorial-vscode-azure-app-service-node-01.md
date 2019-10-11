---
title: 在 Visual Studio Code 中将 Node.js 应用部署到 Azure 应用服务
description: 教程第 1 部分：简介和先决条件。
services: app-service
author: kraigb
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: kraigb
ms.openlocfilehash: 9c9c7085788f39e82eaec78b11d9c679b20c24eb
ms.sourcegitcommit: c04984b6367e922dbc5973af44f8cd0ca81ce157
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2019
ms.locfileid: "71686214"
---
# <a name="deploy-to-azure-app-service-using-visual-studio-code"></a>使用 Visual Studio Code 部署到 Azure 应用服务

在本教程中，将使用[应用服务扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice)将 Node.js 应用程序部署到 Linux 上的 Azure 应用服务。

## <a name="prerequisites"></a>先决条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure 应用服务扩展](vscode:extension/ms-azuretools.vscode-azureappservice)
- [Node.js 和 npm](https://nodejs.org/en/download)，Node.js 包管理器。

> <a class="tutorial-install-extension-btn" href="vscode:extension/ms-azuretools.vscode-azureappservice">安装 Azure 应用服务扩展</a>

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-appservice-extension&mktingSource=vscode-tutorial-appservice-extension)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我已安装 Azure 扩展](tutorial-vscode-azure-app-service-node-02.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=getting-started)