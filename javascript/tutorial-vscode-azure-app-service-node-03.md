---
title: 从 Visual Studio Code 中将 Node.js 应用部署到 Azure 应用服务
description: 教程第 3 部分：部署网站
ms.topic: conceptual
ms.date: 09/20/2019
ms.openlocfilehash: 937eb54e9885e3b5b9fa7be54f551945a54572cd
ms.sourcegitcommit: e77f8f652128b798dbf972078a7b460ed21fb5f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2019
ms.locfileid: "74467199"
---
# <a name="deploy-the-website"></a>部署网站

[上一步：创建应用](tutorial-vscode-azure-app-service-node-02.md)

这一步使用 Visual Studio Code 和 Azure 应用服务扩展部署 Node.js 网站。 本教程使用最基本的部署模型，其中，应用将被压缩并部署到 Linux 上的 Azure 应用服务。

1. 部署应用之前，确保其在侦听 `PORT` 环境变量 `process.env.PORT` 提供的端口。

1. 在应用文件夹（例如上一步的 `myExpressApp`）中，使用以下命令启动该文件夹中的 VS Code：

    ```bash
    code .
    ```

1. 在 VS Code 中，选择 Azure 图标以打开“Azure 应用服务”  资源管理器，然后选择蓝色向上箭头图标，将应用部署到 Azure：

    ![部署到 Web 应用](media/deploy-azure/deploy.png)

    > [!TIP]
    > 也可打开“命令面板”  (**F1**)，键入“deploy to web app”，然后选择 **Azure App Service:** Deploy to Web App”命令。

1. 在出现提示时，输入以下信息：

    a. 选择应用的当前文件夹（在本教程中使用 `myExpressApp`）。

    a. 选择“创建新 Web 应用”  。

    a. 输入应用的全局唯一名称，然后按 **Enter**。 应用名称的有效字符为“a-z”、“0-9”和“-”。

    a. 在靠近你或靠近你可能需要访问的其他服务的 Azure 区域（详见[区域](https://azure.microsoft.com/regions/)）中选择一个位置。

    a. 选择你的 **Node.js 版本**，建议选择“LTS”。

1. 在扩展创建应用时，进度会显示在 VS Code 的“输出”窗口中（可能需要选择“Azure 应用服务”输出）。  

    ![Visual Studio Code 的“Azure 应用服务”输出窗口](media/deploy-azure/output-window.png)

1. 当系统提示你更新配置以在目标服务器上运行 `npm install` 时，选择“是”。  随后将部署应用。

    ![配置的部署](media/deploy-azure/server-build.png)

1. 部署开始后，系统会提示你更新工作区，使所有后续部署自动针对同一应用服务 Web 应用。 选择“是”，以确保将更改部署到正确的应用。 

    ![配置的部署](media/deploy-azure/save-configuration.png)

1. 部署完成后，选择提示中的“浏览网站”以查看全新部署的网站。 

## <a name="troubleshooting"></a>故障排除

错误“你无权查看此目录或页面”表示应用可能未能正常启动。 查看日志（如[下一步](tutorial-vscode-azure-app-service-node-04.md)所述）可能有助于诊断并修复此问题。 如果无法解决此问题，请单击下面的“我遇到了问题”按钮联系我们。  我们很乐意为你提供帮助。

## <a name="updating-the-website"></a>更新网站

要部署对此应用所做的更改，可以使用相同的过程并选择现有应用而不是创建新应用。

----

> [!div class="nextstepaction"]
> [我的站点已部署到 Azure 上](tutorial-vscode-azure-app-service-node-04.md) [我遇到了问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-azureappservice&step=deploy-app)
