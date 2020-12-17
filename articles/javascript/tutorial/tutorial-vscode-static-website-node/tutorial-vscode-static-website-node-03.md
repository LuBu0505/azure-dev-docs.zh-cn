---
title: 从 Visual Studio Code 为静态 Node.js 网站创建 Azure 存储帐户
description: 静态 Web 应用教程第 3 部分：创建 Azure 存储帐户
ms.topic: tutorial
ms.date: 09/24/2019
ms.custom: devx-track-js
ms.openlocfilehash: 17f62bfa8f596f540a2b732daf8ea24063063723
ms.sourcegitcommit: f723980ade4cbc13548a5d8ac3f3fa681b8a2dbd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97609215"
---
# <a name="create-an-azure-storage-account"></a>创建 Azure 存储帐户

[上一步：创建应用](tutorial-vscode-static-website-node-02.md)

在此步骤中，将创建一个 Azure 存储帐户，该帐户使用内置 Web 服务器充当简单的文件存储（或 CDN）。 此内置服务器使 Azure 存储成为快速托管静态站点的理想选择。

1. 从上一步中创建的 `my-static-app` 文件夹中，启动 Visual Studio Code，以便它自动打开该文件夹：

    ```bash
    code .
    ```

1. 在 VS Code 中，选择 Azure 徽标以打开 **Azure** 资源管理器。 在 **Azure 存储** 下，右键单击 Azure 订阅，然后选择“创建存储帐户”  ：

    ![在 VS Code 中创建存储帐户](../../media/static-website/create-storage-account.png)

1. 在提示“输入新存储帐户的名称”时，为存储帐户输入全局唯一的名称，然后按 Enter。 应用名称的有效字符包括“a-z”和“0-9”。

    > [!NOTE]
    > 这将创建具有相同名称的存储帐户和资源组。 它会自动将存储帐户置于“美国西部”区域。 若要指定资源组和位置，请从上下文菜单中选择“创建存储帐户(高级)”选项。

1. 创建存储帐户后，进度将显示在 VS Code 的“输出”  面板中：

    ![VS Code 输出窗口 ](../../media/static-website/output-storage.png)

1. 存储帐户完成后，会显示一条消息，指出已为存储帐户启用静态网站托管功能。

    ![创建存储帐户](../../media/static-website/static-website-enabled-notification.png)

    > [!IMPORTANT]
    > 我们将 *index.html* 用于错误文档，因为诸如 React、Angular和 Vue 之类的新式单页应用 (SPA) 可处理客户端中的路由错误。 对于经典静态网站，请使用自定义 404 错误页。

> [!div class="nextstepaction"]
> [我创建了一个存储容器](tutorial-vscode-static-website-node-04.md) [我遇到了一个问题](https://www.research.net/r/PWZWZ52?tutorial=node-deployment-staticwebsite&step=create-storage)
