---
title: 从 Visual Studio Code 将静态 Node.js 网站部署到 Azure
description: 教程第 1 部分：简介和先决条件。
ms.topic: conceptual
ms.date: 09/23/2019
ms.custom: devx-track-javascript
ms.openlocfilehash: d2c66383e654624cf7edce542461340cc248ba62
ms.sourcegitcommit: 69933dcce571b2686897b295b7822e207d944617
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90773082"
---
# <a name="deploy-a-static-website-to-azure-from-visual-studio-code"></a>从 Visual Studio Code 将静态网站部署到 Azure

在本教程中，你将使用 [Azure 存储](/azure/storage)创建静态网站并将其部署到 Azure。 静态网站由 HTML、CSS、JavaScript 和其他静态文件（如图像或字体）组成。 静态站点通常是用 Angular、React 或 Vue 编写的单页应用程序（或 [SPA](https://en.wikipedia.org/wiki/Single-page_application)）。 无论你如何设计应用，你都是直接从_存储_托管和处理这些文件，而不是使用 Web 服务器。 在存储中托管比维护 Web 服务器更简单，成本更低。

## <a name="walkthrough-video"></a>演练视频

观看此视频，了解本文中内容的完整演练。

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Azure/Deploy-static-website-to-Azure-from-Visual-Studio-Code/player]

> [!NOTE]
> 如果你有自己的服务器代码（例如，Node.js/Express 应用），请改为按照[应用服务教程](tutorial-vscode-azure-app-service-node-01.md)进行操作。

## <a name="prerequisites"></a>必备条件

- 一个 [Azure 订阅](#azure-subscription)。
- [Visual Studio Code](https://code.visualstudio.com/)。
- [Azure 存储扩展](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage)。
- [Node.js 和 npm](https://nodejs.org/en/download)，即 Node.js 包管理器。 （此要求仅用于生成示例项目。 如果已有应用代码，则无需安装 Node.js。）

> <a class="tutorial-install-extension-btn" href="https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage">安装 Azure 存储扩展</a>

### <a name="azure-subscription"></a>Azure 订阅

如果你没有 Azure 订阅，请[立即注册](https://azure.microsoft.com/free/?utm_source=campaign&utm_campaign=vscode-tutorial-static-website&mktingSource=vscode-tutorial-static-website)一个免费使用的帐户来试用任何服务组合，并获得 200 美元的 Azure 额度。

## <a name="sign-in-to-azure"></a>登录 Azure

[!INCLUDE [azure-sign-in](includes/azure-sign-in.md)]

> [!div class="nextstepaction"]
> [我安装了必备项](tutorial-vscode-static-website-node-02.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=getting-started)