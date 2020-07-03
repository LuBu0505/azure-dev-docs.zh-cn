---
title: 通过 Visual Studio Code 将静态 Node.js 应用文件部署到 Azure 存储
description: 教程第 4 部分：将文件部署到 Azure 存储
ms.topic: conceptual
ms.date: 09/24/2019
ms.author: buhollan
ms.openlocfilehash: 53578d829167512877c2359d02c3e70d88afe77f
ms.sourcegitcommit: 553da4e9aa988e5bb823364244ea81961cee5bc7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85791510"
---
# <a name="deploy-the-website-to-azure-storage"></a>将网站部署到 Azure 存储

[上一步：创建存储帐户](tutorial-vscode-static-website-node-03.md)

在此步骤中，将使用 Visual Studio Code 将在前面步骤中创建的静态网站文件部署到 Azure 存储，然后在其中托管并处理这些文件。

# <a name="angular"></a>[Angular](#tab/angular)

1. 在 Visual Studio Code 中，转到 **Azure 存储**资源管理器，展开订阅，展开在上一步中创建的 Azure 存储帐户的节点，然后展开“Blob 容器”  节点。 `$web` 容器是你部署应用代码的位置。

   ![Azure 存储资源管理器中的 Azure 存储节点](media/static-website/storage-nodes.png)

1. 选择**文件**资源管理器，右键单击 _dist/my-static-app_ 文件夹，然后选择“部署到静态网站”  ：

    ![“部署到静态网站”命令](media/static-website/deploy-build-angular.png)

1. 出现提示时，选择之前创建的存储帐户。

1. 部署完成后，将显示一条带有“浏览到网站”  按钮的消息。 选择该按钮以打开部署的应用代码的主终结点。

    ![“部署完成”消息](media/static-website/deployment-complete.png)

    ![Azure 中运行的静态网站](media/static-website/azure-app-angular.png)

# <a name="react"></a>[React](#tab/react)

1. 在 Visual Studio Code 中，转到 **Azure 存储**资源管理器，展开订阅，展开在上一步中创建的 Azure 存储帐户的节点，然后展开“Blob 容器”  节点。 `$web` 容器是你部署应用代码的位置。

   ![Azure 存储资源管理器中的 Azure 存储节点](media/static-website/storage-nodes.png)

1. 选择**文件**资源管理器，右键单击 build  文件夹，然后选择“部署到静态网站”  ：

    ![“部署到静态网站”命令](media/static-website/deploy-build-react.png)

1. 出现提示时，选择之前创建的存储帐户。

1. 部署完成后，将显示一条带有“浏览到网站”  按钮的消息。 选择该按钮以打开部署的应用代码的主终结点。

    ![“部署完成”消息](media/static-website/deployment-complete.png)

    ![Azure 中运行的静态网站](media/static-website/azure-app-react.png)

# <a name="vue"></a>[Vue](#tab/vue)

1. 在 Visual Studio Code 中，转到 **Azure 存储**资源管理器，展开订阅，展开在上一步中创建的 Azure 存储帐户的节点，然后展开“Blob 容器”  节点。 `$web` 容器是你部署应用代码的位置。

   ![Azure 存储资源管理器中的 Azure 存储节点](media/static-website/storage-nodes.png)

1. 选择**文件**资源管理器，右键单击 dist  文件夹，然后选择“部署到静态网站”  ：

    ![“部署到静态网站”命令](media/static-website/deploy-build-vue.png)

1. 出现提示时，选择之前创建的存储帐户。

1. 部署完成后，将显示一条带有“浏览到网站”  按钮的消息。 选择该按钮以打开部署的应用代码的主终结点。

    ![“部署完成”消息](media/static-website/deployment-complete.png)

    ![Azure 中运行的静态网站](media/static-website/azure-app-vue.png)

# <a name="svelte"></a>[Svelte](#tab/svelte)

1. 在 Visual Studio Code 中，转到 **Azure 存储**资源管理器，展开订阅，展开在上一步中创建的 Azure 存储帐户的节点，然后展开“Blob 容器”  节点。 `$web` 容器是你部署应用代码的位置。

   ![Azure 存储资源管理器中的 Azure 存储节点](media/static-website/storage-nodes.png)

1. 选择**文件**资源管理器，右键单击 public  文件夹，然后选择“部署到静态网站”  ：

    ![“部署到静态网站”命令](media/static-website/deploy-build-svelte.png)

1. 出现提示时，选择之前创建的存储帐户。

1. 部署完成后，将显示一条带有“浏览到网站”  按钮的消息。 选择该按钮以打开部署的应用代码的主终结点。

    ![“部署完成”消息](media/static-website/deployment-complete-svelte.png)

    ![Azure 中运行的静态网站](media/static-website/azure-app-svelte.png)

---

> [!div class="nextstepaction"]
> [我的站点位于 Azure 上](tutorial-vscode-static-website-node-05.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)