---
title: 简介和先决条件
description: 使用 GitHub 操作在本地构建 React 客户端应用程序并将其部署到 Azure 静态 Web 应用。
ms.topic: tutorial
ms.date: 11/13/2020
ms.custom: devx-track-js
ms.openlocfilehash: 5d70e14a9a5247f99c8b6e033af0a225b27da30b
ms.sourcegitcommit: 291768a67862336267c67819e913c16710e3875e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95820681"
---
# <a name="1-build-and-deploy-a-static-web-app-to-azure"></a>1.构建静态 Web 应用并将其部署到 Azure

在本教程中，使用 GitHub 操作在本地构建 React 客户端应用程序并将其部署到 Azure 静态 Web 应用。 

React (create-react-app) 提供以下功能： 
* 如果找不到用于认知服务计算机视觉的 Azure 密钥和终结点，则显示消息
* 允许使用认知服务计算机视觉分析图像
    * 输入公共图像 URL 或分析集合中的图像
    * 分析完成时
        * 显示图像
        * 显示计算机视觉 JSON 结果 

当推送到特定分支时，GitHub 操作便会启动：
* 在构建中插入计算机视觉密钥和终结点的 GitHub 机密
* 构建 React (create-react-app) 客户端
* 将生成的文件移动到 Azure 静态 Web 应用资源

[!INCLUDE [Create or use existing Azure Subscription ](../../includes/environment-subscription-h2.md)]

## <a name="what-is-an-azure-static-web-app"></a>什么是 Azure 静态 Web 应用

在构建静态 Web 应用时，Azure 根据你感兴趣的功能和控制程度提供了多种选项。 本教程重点介绍最简单的服务，其中提供了许多选项，这样你就可以专注于前端代码而不是宿主环境。

## <a name="prerequisites"></a>先决条件

- [安装 Azure CLI](/cli/azure/install-azure-cli)，或使用 [Azure Cloud Shell](https://shell.azure.com.)
- [Node.js 和 npm](https://nodejs.org/en/download) - 已安装到本地计算机。
- [Visual Studio Code](https://code.visualstudio.com/) - 已安装到本地计算机。 
    - [Azure Static Web Apps（预览版）](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestaticwebapps) - 用于将 React 应用部署到 Azure 静态 Web 应用。
- [Git](https://git-scm.com/downloads) - 用于推送到 GitHub，这将激活 GitHub 操作。
- [GitHub 帐户](https://github.com/join) - 用于创建分支并推送到存储库

## <a name="next-step"></a>后续步骤

> [!div class="nextstepaction"]
> [在本地下载并运行 React 认知服务图像分析器应用](run-the-react-cognitive-services-image-analyzer-app-locally.md) 